import React, { useEffect, useState } from 'react';
import Body from './Body';
import { useDispatch, useSelector } from 'react-redux';
import json from "../../../assets/resource/andydemo.json";
import { MdDownload } from "react-icons/md";
import TranscriptPreview from './Preview';
import { FaPlus, FaMinus } from "react-icons/fa6";
import { checkJson } from '../../../utility/customise/checkJson';
import AgentWikiBody from "./agentWikiBody";
import AutoAuditBody from "./AutoAuditBody";
import ActionWorkflow from "./actionWorkflow";
import { setProgressBarTranscript } from "../../../store/callSlice";
import { setDemoTranscriptSubmitted, setEditDemoTranscript, updateDemoData } from '../../../store/demo-slice';

function FileUpload({ setTranscript, message, setMessage }) {
  const dispatch = useDispatch();
  const onUpload = (e) => {
    const file = e.target.files[0];
    dispatch(setProgressBarTranscript(file));
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
          jsonData.forEach((obj) => {
            if (obj.eventData.Context && !Array.isArray(obj.eventData.Context)) {
              obj.eventData.Context = Array(obj.eventData.Context);
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
  }

  return (
    <form className='body-container'>
      <label htmlFor='json-file' className='upload-btn tab btn-active'>+ Upload</label>
      <input style={{ visibility: 'hidden' }} id='json-file' type='file' accept='application/JSON' onChange={onUpload} />
      {message && <p style={{ color: 'red' }}>{message}</p>}
    </form>
  );
}

export default function Customize({ transcript: propTranscript, setTranscript: setPropTranscript, handleInput, audio, setAudio, tab }) {
  const demoState = useSelector((state) => state?.demostate);
  const appCallData = useSelector((state) => state?.call);
  const isEditDemo = demoState?.editDemo;

  // Renaming `transcript` and `setTranscript` to avoid conflicts with props, so it works without removing code.
  const [transcriptState, setTranscriptState] = useState(propTranscript || []);
  const [message, setMessage] = useState(undefined);
  const [selectedNudge, setSelectedNudge] = useState('cc'); // Default to Call Context
  const [index, setIndex] = useState(0);

  const [fields, setFields] = useState({
    cc: [],
    ai: [],
    ss: [],
    ka: [],
    utterance: [],
  });

  const dispatch = useDispatch();

  audio && setAudio(audio);

  const sendTranscriptData = () => {
    if (isEditDemo && transcriptState?.length) {
      dispatch(updateDemoData({ type: "transcript", data: transcriptState }));
    }
    dispatch(setEditDemoTranscript(false));
    dispatch(setDemoTranscriptSubmitted(true));
  };

  const downloadTranscript = () => {
    if (transcriptState) {
      const transcriptURL = URL.createObjectURL(new Blob([JSON.stringify(transcriptState)], { type: 'application/json' }));

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
    setTranscriptState(propTranscript);
  }, [propTranscript]);

  return (
    <div className='transcript-container'>
      {/* Nudge Types Section */}
      {tab === 1 && (
        <div className="nudge-types">
          <div className="options-types">NUDGE TYPE AND UTTERANCES</div>
          <div className="nudge-options">
            {['cc', 'ai', 'ss', 'ka', 'utterance'].map((type) => (
              <div
                key={type}
                className={`nudge-option ${type === 'cc' ? "first-nudge" : (type === 'utterance' ? "last-nudge" : "mid-nudge")}`}
                onClick={() => handleNudgeType(type)}
              >
                <span>{type === 'cc' ? '+ CALL CONTEXT' : type === 'ai' ? '+ AI GUIDANCE' : type === 'ss' ? '+ SPEECH SUGGESTION' : type === 'ka' ? '+ KNOWLEDGE ARTICLE' : '+ UTTERANCES'}</span>
              </div>
            ))}
          </div>
        </div>
      )}

      {/* File Upload Section */}
      {!transcriptState?.length && <FileUpload setTranscript={setPropTranscript} message={message} setMessage={setMessage} />}
      {transcriptState?.length && (
        <div style={{ display: 'flex', justifyContent: 'space-between' }}>
          <div className='transcript-body'>
            {tab === 1 && <Body tabs={1} transcript={transcriptState} setTranscript={setPropTranscript} selectNudge={selectedNudge} setIndex={setIndex} index={index} />}
            {tab === 2 && <AgentWikiBody tabs={2} transcript={transcriptState} setTranscript={setPropTranscript} selectNudge={selectedNudge} setIndex={setIndex} index={index} />}
            {tab === 3 && <AutoAuditBody tabs={3} transcript={transcriptState} setTranscript={setPropTranscript} selectNudge={selectedNudge} setIndex={setIndex} index={index} />}
            {tab === 4 && <ActionWorkflow tabs={4} transcript={transcriptState} setTranscript={setPropTranscript} selectNudge={selectedNudge} setIndex={setIndex} index={index} />}
          </div>
          <div className={`transcript-preview ${tab === 1 ? "transcript-preview" : "transcript-preview-enlarged"}`}>
            <TranscriptPreview
              transcript={transcriptState}
              setTranscript={setPropTranscript}
              setIndex={setIndex}
              index={index}
            />

            {/* Nudge Fields Section */}
            <div className="nudge-fields">
              {/* Call Context Fields */}
              {selectedNudge === 'cc' && (
                <div className="field-section">
                  <h3>Call Context Fields:</h3>
                  {fields.cc.map((id, index) => (
                    <div key={`cc-${id}`} className="row">
                      <div className="label">CC: Call Context Message {id}</div>
                      <input type="text" placeholder="Call Context Message" />
                      <button className="styled-button remove-button" onClick={() => removeField('cc', index)}>
                        <FaMinus /> Remove
                      </button>
                    </div>
                  ))}
                  <button className="styled-button" onClick={() => addField('cc')}>
                    <FaPlus /> Add More Call Context Fields
                  </button>
                </div>
              )}

              {/* AI Guidance Fields */}
              {selectedNudge === 'ai' && (
                <div className="field-section">
                  <h3>AI Guidance Fields:</h3>
                  {fields.ai.map((id, index) => (
                    <div key={`ai-${id}`} className="row">
                      <div className="label">AI: Message {id}</div>
                      <input type="text" placeholder="Title" />
                      <input type="text" placeholder="Subtitle" />
                      <button className="styled-button remove-button" onClick={() => removeField('ai', index)}>
                        <FaMinus /> Remove
                      </button>
                    </div>
                  ))}
                  <button className="styled-button" onClick={() => addField('ai')}>
                    <FaPlus /> Add More AI Guidance Fields
                  </button>
                </div>
              )}

              {/* Speech Suggestion Fields */}
              {selectedNudge === 'ss' && (
                <div className="field-section">
                  <h3>Speech Suggestion Fields:</h3>
                  {fields.ss.map((id, index) => (
                    <div key={`ss-${id}`} className="row">
                      <div className="label">SS: Speech Suggestion Message {id}</div>
                      <input type="text" placeholder="Title" />
                      <button className="styled-button remove-button" onClick={() => removeField('ss', index)}>
                        <FaMinus /> Remove
                      </button>
                    </div>
                  ))}
                  <button className="styled-button" onClick={() => addField('ss')}>
                    <FaPlus /> Add More Speech Suggestion Fields
                  </button>
                </div>
              )}

              {/* Knowledge Article Fields */}
              {selectedNudge === 'ka' && (
                <div className="field-section">
                  <h3>Knowledge Article Fields:</h3>
                  {fields.ka.map((id, index) => (
                    <div key={`ka-${id}`} className="row">
                      <div className="label">KA: Knowledge Article {id}</div>
                      <input type="text" placeholder="Pointer" />
                      <button className="styled-button remove-button" onClick={() => removeField('ka', index)}>
                        <FaMinus /> Remove
                      </button>
                    </div>
                  ))}
                  <button className="styled-button" onClick={() => addField('ka')}>
                    <FaPlus /> Add More Knowledge Article Fields
                  </button>
                </div>
              )}

              {/* Utterance Fields */}
              {selectedNudge === 'utterance' && (
                <div className="field-section">
                  <h3>Utterance Fields:</h3>
                  {fields.utterance.map((id, index) => (
                    <div key={`utterance-${id}`} className="row">
                      <div className="label">Utterance Message {id}</div>
                      <input type="text" placeholder="Start Time" />
                      <input type="text" placeholder="End Time" />
                      <button className="styled-button remove-button" onClick={() => removeField('utterance', index)}>
                        <FaMinus /> Remove
                      </button>
                    </div>
                  ))}
                  <button className="styled-button" onClick={() => addField('utterance')}>
                    <FaPlus /> Add More Utterance Fields
                  </button>
                </div>
              )}
            </div>

            <div style={{ display: 'flex' }}>
              {isEditDemo && (
                <>
                  <button className='transcript-download-btn' onClick={sendTranscriptData}>
                    Save Changes
                  </button>
                  <button className='transcript-download-btn back-btn-right-align' onClick={() => dispatch(setEditDemoTranscript(false))}>
                    Back
                  </button>
                </>
              )}
            </div>
          </div>
        </div>
      )}
    </div>
  );
}
