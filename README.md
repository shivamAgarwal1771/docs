import React, { useState } from 'react';
import Body from './Body';
import { useDispatch, useSelector } from 'react-redux';
import { MdDownload } from "react-icons/md";
import TranscriptPreview from './Preview';
import { FaPlus, FaMinus } from "react-icons/fa6";
import { checkJson } from '../../../utility/customise/checkJson';
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
          }, [8000]);
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
        }, [8000]);
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

export default function Customize({ audio, setAudio }) {
  const demoState = useSelector((state) => state?.demostate);
  const isEditDemo = demoState?.editDemo;
  const [transcript, setTranscript] = useState(demoState?.getDemoData?.transcript);
  const [message, setMessage] = useState(undefined);
  const [index, setIndex] = useState(0);
  const [selectedNudge, setSelectedNudge] = useState('cc'); // Default to Call Context
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
    if (isEditDemo && transcript?.length) {
      dispatch(updateDemoData({ type: "transcript", data: transcript }));
    }
    dispatch(setEditDemoTranscript(false));
    dispatch(setDemoTranscriptSubmitted(true));
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

  return (
    <div className='transcript-container'>
      {/* Nudge Types Section */}
      <div className="nudge-types">
        <div className="options-types">NUDGE TYPE</div>
        <div className="nudge-options">
          {['cc', 'ai', 'ss', 'ka', 'utterance'].map(type => (
            <div key={type} className="nudge-option" onClick={() => handleNudgeType(type)}>
              <span>{type === 'cc' ? 'CALL CONTEXT' : type === 'ai' ? 'AI GUIDANCE' : type === 'ss' ? 'SPEECH SUGGESTION' : type === 'ka' ? 'KNOWLEDGE ARTICLE' : 'UTTERANCES'}</span>
            </div>
          ))}
        </div>
      </div>

      {/* Existing FileUpload and Transcript logic */}
      {!transcript?.length && <FileUpload setTranscript={setTranscript} message={message} setMessage={setMessage} />}
      {transcript?.length &&
        <div style={{ display: 'flex' }}>
          <div className='transcript-body'>
            <Body transcript={JSON.parse(JSON.stringify(transcript))} setTranscript={setTranscript} setIndex={setIndex} index={index} />
          </div>
          <div className='transcript-preview'>
            <TranscriptPreview
              transcript={transcript}
              setTranscript={setTranscript}
              setIndex={setIndex}
              index={index}
            />

            {/* Nudge Fields Section */}
            <div className="nudge-fields">
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
              {isEditDemo &&
                <>
                  <button
                    className='transcript-download-btn'
                    onClick={sendTranscriptData}
                  >
                    Save Changes
                  </button>
                  <button
                    className='transcript-download-btn back-btn-right-align'
                    onClick={handleBack}
                  >
                    Back
                  </button>
                </>
              }
            </div>
          </div>
        </div>}
    </div>
  );
}
