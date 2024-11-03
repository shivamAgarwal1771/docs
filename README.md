import React, { useEffect, useState } from 'react';
import Body from './Body';
import { useDispatch, useSelector } from 'react-redux';
import { MdDownload } from "react-icons/md";
import TranscriptPreview from './Preview';
import { handleAddRecommendation, handleRemove } from './Body';
import { FaArrowRightFromBracket, FaPlus, FaMinus } from "react-icons/fa6";
import { checkJson } from '../../../utility/customise/checkJson';
import { setDemoTranscriptSubmitted, setEditDemoTranscript, updateDemoData } from '../../../store/demo-slice';

function FileUpload({ setTranscript, message, setMessage }) {
  // File upload logic...
}

function JsonPreview({ transcript, index }) {
  // JSON preview logic...
}

export default function Customize({ audio, setAudio }) {
  const demoState = useSelector((state) => state?.demostate);
  const isEditDemo = demoState?.editDemo;
  const [transcript, setTranscript] = useState(demoState?.getDemoData?.transcript);
  const [message, setMessage] = useState(undefined);
  const [index, setIndex] = useState(0);
  const dispatch = useDispatch();

  // New state for nudge type and fields
  const [selectedNudge, setSelectedNudge] = useState('cc'); // Default to Call Context
  const [fields, setFields] = useState({
    cc: [],
    ai: [],
    ss: [],
    ka: [],
  });

  audio && setAudio(audio);

  const downloadTranscript = () => {
    // Download transcript logic...
  };

  const sendTranscriptData = () => {
    // Send transcript data logic...
  };

  const handleBack = () => {
    // Handle back logic...
  };

  // Functions for nudge fields
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
          <div className="nudge-option" onClick={() => handleNudgeType('cc')}>
            <span>CALL CONTEXT</span>
          </div>
          <div className="nudge-option" onClick={() => handleNudgeType('ai')}>
            <span>AI GUIDANCE</span>
          </div>
          <div className="nudge-option" onClick={() => handleNudgeType('ss')}>
            <span>SPEECH SUGGESTION</span>
          </div>
          <div className="nudge-option" onClick={() => handleNudgeType('ka')}>
            <span>KNOWLEDGE ARTICLE</span>
          </div>
        </div>
      </div>

      <div className="main-content">
        <div className="left-section">
          {/* Render fields based on selected nudge */}
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
        </div>

        <div className="right-section">
          {/* Other components or information can be displayed here */}
          <div className="row">
            <button className="styled-button" onClick={sendTranscriptData}>
              Save Changes
            </button>
            <button className="styled-button" onClick={handleBack}>
              Back
            </button>
          </div>
        </div>
      </div>
    </div>
  );
}
