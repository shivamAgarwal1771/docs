import React, { useState } from 'react';
import { useDispatch, useSelector } from 'react-redux';
import { FaPlus, FaMinus } from "react-icons/fa6";
import { setProgressBarTranscript } from "../../../store/callSlice";
import { setDemoTranscriptSubmitted, setEditDemoTranscript, updateDemoData } from '../../../store/demo-slice';

import Body from './Body';
import TranscriptPreview from './Preview';
import AgentWikiBody from "./agentWikiBody";
import AutoAuditBody from "./AutoAuditBody";
import ActionWorkflow from "./actionWorkflow";
import FileUpload from './FileUpload';

function Customize({ transcript: propTranscript, setTranscript: propSetTranscript, handleInput, audio, setAudio, tab }) {
  const demoState = useSelector((state) => state?.demostate);
  const appCallData = useSelector((state) => state?.call);
  const isEditDemo = demoState?.editDemo;

  // Rename local state variable to avoid conflict with prop `transcript`
  const [localTranscript, setLocalTranscript] = useState(propTranscript || []);
  const [message, setMessage] = useState(undefined);
  const [selectedNudge, setSelectedNudge] = useState('cc'); // Default to Call Context
  const [index, setIndex] = useState(0);

  // Fields for each nudge type
  const [fields, setFields] = useState({
    cc: [],
    ai: [],
    ss: [],
    ka: [],
    utterance: [],
  });

  const dispatch = useDispatch();

  // Ensure audio is set when passed
  audio && setAudio(audio);

  // Function to handle submitting the transcript data
  const sendTranscriptData = () => {
    if (isEditDemo && localTranscript?.length) {
      dispatch(updateDemoData({ type: "transcript", data: localTranscript }));
    }
    dispatch(setEditDemoTranscript(false));
    dispatch(setDemoTranscriptSubmitted(true));
  };

  // Function to download the transcript as JSON
  const downloadTranscript = () => {
    if (localTranscript) {
      const transcriptURL = URL.createObjectURL(new Blob([JSON.stringify(localTranscript)], { type: 'application/json' }));
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

  // Function to handle changing nudge type
  const handleNudgeType = (type) => {
    setSelectedNudge(type);
  };

  // Function to add a new field for a specific nudge type
  const addField = (type) => {
    setFields((prev) => ({
      ...prev,
      [type]: [...prev[type], prev[type].length + 1],
    }));
  };

  // Function to remove a field for a specific nudge type
  const removeField = (type, index) => {
    setFields((prev) => ({
      ...prev,
      [type]: prev[type].filter((_, i) => i !== index),
    }));
  };

  return (
    <div className='transcript-container'>
      {/* Nudge Types Section */}
      {tab == 1 && (
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
      {!localTranscript?.length && <FileUpload setTranscript={setLocalTranscript} message={message} setMessage={setMessage} />}
      
      {localTranscript?.length && (
        <div style={{ display: 'flex', justifyContent: 'space-between' }}>
          <div className='transcript-body'>
            {tab == 1 && <Body tabs={1} transcript={localTranscript} setTranscript={setLocalTranscript} selectNudge={selectedNudge} setIndex={setIndex} index={index} />}
            {tab == 2 && <AgentWikiBody tabs={2} transcript={localTranscript} setTranscript={setLocalTranscript} selectNudge={selectedNudge} setIndex={setIndex} index={index} />}
            {tab == 3 && <AutoAuditBody tabs={3} transcript={localTranscript} setTranscript={setLocalTranscript} selectNudge={selectedNudge} setIndex={setIndex} index={index} />}
            {tab == 4 && <ActionWorkflow tabs={4} transcript={localTranscript} setTranscript={setLocalTranscript} selectNudge={selectedNudge} setIndex={setIndex} index={index} />}
          </div>
          <div className={`transcript-preview ${tab == 1 ? "transcript-preview" : "transcript-preview-enlarged"}`}>
            <TranscriptPreview
              transcript={localTranscript}
              setTranscript={setLocalTranscript}
              setIndex={setIndex}
              index={index}
            />
            <div className="nudge-fields">
              {/* Fields for selected Nudge Type */}
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
              {/* Similarly for other nudge types */}
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
                      <input type="text" placeholder="Utterance" />
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

export default Customize;
