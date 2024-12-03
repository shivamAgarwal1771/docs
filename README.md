import React, { useEffect, useState } from 'react';
import Body from './Body';
import { useDispatch, useSelector } from 'react-redux';
// import json from "../../../assets/resource/andydemo.json"
import { MdDownload } from "react-icons/md";
import TranscriptPreview from './Preview';
import { FaPlus, FaMinus } from "react-icons/fa6";
import { checkJson } from '../../../utility/customise/checkJson';
import AgentWikiBody from "./agentWikiBody"
import AutoAuditBody from "./AutoAuditBody"
import ActionWorkflow from "./actionWorkflow"
import {NUDGES_UTTERANCES , NUDGE_TAB} from "../../../utility/constants"
import { setTabsTrnacript } from "../../../store/callSlice"
import { setDemoTranscriptSubmitted, setEditDemoTranscript, updateDemoData } from '../../../store/demo-slice';


function FileUpload({ setTranscript, message, setMessage }) {
  const onUpload = (e) => {
    const file = e.target.files[0];
    if (!file) return;

    const filereader = new FileReader();
    filereader.readAsText(file, 'UTF-8');

    filereader.onload = (e) => {
      let content = e.target.result;

      try {
        let jsonData = JSON.parse(content);
        let reviewJson = checkJson(jsonData);
        if (!reviewJson.value) {
          setMessage(reviewJson.msg);
          setTimeout(() => {
            setMessage('');
          }, 8000);
        } else {
          jsonData?.forEach((obj) => {
            if (obj?.eventData?.Context && !Array.isArray(obj?.eventData?.Context)) {
              obj.eventData.Context = Array(obj?.eventData?.Context);
            }
          });
          setTranscript(jsonData);
        }
      } catch (err) {
        setMessage('Not a valid JSON or format');
        setTimeout(() => {
          setMessage('');
        }, 8000);
        console.error(err);
      }
    };
  };

  return (
    <form className='body-container'>
      <label htmlFor='json-file' className='upload-btn tab btn-active'>+ Upload</label>
      <input style={{ visibility: 'hidden' }} id='json-file' type='file' accept='application/JSON' onChange={onUpload} />
      {message && <p style={{ color: 'red' }}>{message}</p>}
    </form>
  );
}

export default function Customize({ finalTranscript, setFinalTranscript, transcripts, setTranscripts, handleInput, audio, setAudio, tab }) {
  const demoState = useSelector((state) => state?.demostate);
  const appCallData = useSelector((state) => state?.call);
  const speechCallData = useSelector((state) => state?.speechstate);
  const isEditDemo = demoState?.editDemo;
  const [transcript, setTranscript] = useState(demoState?.getDemoData?.transcript || []);
  const [message, setMessage] = useState(undefined);
  const [selectedNudge, setSelectedNudge] = useState('contextNudgeTab'); // Default to Call Context
  const [index, setIndex] = useState(0);
  const [progessScript, setProgessScript] = useState(true);
  const [fields, setFields] = useState({
    contextNudgeTab: [],
    aiNudgeTab: [],
    speechSuggestionNudge: [],
    knowledgeTab: [],
    utterance: [],
  });
  const dispatch = useDispatch();

  audio && setAudio(audio);

  const sendTranscriptData = () => {
    if (isEditDemo && transcript?.length) {
      // dispatch(updateDemoData({ type: "transcript", data: transcript }));
    }
    dispatch(setEditDemoTranscript(false));
    dispatch(setDemoTranscriptSubmitted(true));
  };

  const downloadTranscript = () => {
    if (transcript) {
      const transcriptURL = URL.createObjectURL(new Blob([JSON.stringify(transcript)], { type: 'application/json' }));

      const a = document.createElement('a');
      a.href = transcriptURL;
      a.download = "transcription.json";
      a.style.display = "none";
      document.body.appendChild(a);

      a.click();

      document.body.removeChild(a);
      URL.revokeObjectURL(transcriptURL);
    }
  };

  const handleNudgeType = (type) => {
    setSelectedNudge(type);
  };

  const addField = (type) => {
    setFields((prev) => ({
      ...prev,
      [type]: [...prev[type], prev[type].length + 1],
    }));
  };

  const removeField = (type, index) => {
    setFields((prev) => ({
      ...prev,
      [type]: prev[type].filter((_, i) => i !== index),
    }));
  };

  useEffect(() => {
    transcript?.length && setFinalTranscript(transcript);
    progessScript && speechCallData?.conversation && setFinalTranscript(speechCallData?.conversation);
    progessScript && speechCallData?.conversation && setTranscript(speechCallData?.conversation);
    setProgessScript(false);
    dispatch(setTabsTrnacript(transcript))
  }, [transcript, speechCallData?.conversation]);

  useEffect(()=>{
    if(appCallData.tabsTrnacript && appCallData.progressBarResponse)
    {
      setFinalTranscript(appCallData.tabsTrnacript);
      setTranscript(appCallData.tabsTrnacript);
    }
  },[])

  return (
    <div className='transcript-container'>
      {tab === 1 && (
        <div className="nudge-types">
          <div className="options-types">NUDGE TYPE AND UTTERANCES</div>
          <div className="nudge-options">
            {[NUDGES_UTTERANCES.CONTEXT_TAB, NUDGES_UTTERANCES.AI_TAB, NUDGES_UTTERANCES.SS_TAB, NUDGES_UTTERANCES.KNOWLEDGE_ARTICLE_TAB, NUDGES_UTTERANCES.UTTERANCE].map((type) => (
              <div
                key={type}
                className={`nudge-option ${type === NUDGES_UTTERANCES.CONTEXT_TAB ? "first-nudge" : (type === 'utterance' ? "last-nudge" : "mid-nudge")}`}
                onClick={() => handleNudgeType(type)}
              >
                <span>
                  {type === NUDGES_UTTERANCES.CONTEXT_TAB
                    ? NUDGE_TAB.ADD_CONTEXT
                    : type === NUDGES_UTTERANCES.AI_TAB
                    ? NUDGE_TAB.ADD_GUIDANCE
                    : type === NUDGES_UTTERANCES.SS_TAB
                    ? NUDGE_TAB.ADD_SS
                    : type === NUDGES_UTTERANCES.KNOWLEDGE_ARTICLE_TAB
                    ? NUDGE_TAB.ADD_KNOWLEDGE
                    : NUDGE_TAB.ADD_UTTERANCE}
                </span>
              </div>
            ))}
          </div>
        </div>
      )}

      {!transcript?.length && <FileUpload setTranscript={setTranscript} message={message} setMessage={setMessage} />}
      {transcript?.length && (
        <div style={{ display: 'flex', justifyContent: 'space-between' }}>
          <div className='transcript-body'>
            {tab === 1 && <Body tabs={1} transcript={JSON.parse(JSON.stringify(transcript))} setTranscript={setTranscript} selectNudge={selectedNudge} setIndex={setIndex} index={index} />}
            {tab === 2 && <AgentWikiBody tabs={2} transcript={JSON.parse(JSON.stringify(transcript))} setTranscript={setTranscript} selectNudge={selectedNudge} setIndex={setIndex} index={index} />}
            {tab === 3 && <AutoAuditBody tabs={3} transcript={JSON.parse(JSON.stringify(transcript))} setTranscript={setTranscript} selectNudge={selectedNudge} setIndex={setIndex} index={index} />}
            {tab === 4 && <ActionWorkflow tabs={4} transcript={JSON.parse(JSON.stringify(transcript))} setTranscript={setTranscript} selectNudge={selectedNudge} setIndex={setIndex} index={index} />}
          </div>
          <div className={`transcript-preview ${tab === 1 ? "transcript-preview" : "transcript-preview-enlarged"}`}>
            <TranscriptPreview
              transcript={transcript}
              setTranscript={setTranscript}
              setIndex={setIndex}
              index={index}
            />
          </div>
        </div>
      )}

      {/* Download Button */}
      {transcript?.length > 0 && (
        <div className="download-btn-container">
          <button className="download-btn" onClick={downloadTranscript}>
            <MdDownload style={{ marginRight: '8px' }} /> Download Transcript
          </button>
        </div>
      )}
    </div>
  );
}
