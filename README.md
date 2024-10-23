import React, { useState } from 'react';
import { FaPlus } from 'react-icons/fa'; // React Icon for the plus symbol
import './NudgeForm.css'; // Importing the updated CSS

const NudgeForm = () => {
  // State to track which nudge type is selected and dynamically added fields
  const [selectedNudge, setSelectedNudge] = useState('');
  const [fields, setFields] = useState({
    cc: [],
    ai: [],
    ss: [],
    ka: [],
  });

  // Function to handle selecting a nudge type
  const handleNudgeType = (type) => {
    setSelectedNudge(type);
  };

  // Function to add fields dynamically based on nudge type
  const addField = (type) => {
    setFields((prev) => ({
      ...prev,
      [type]: [...prev[type], prev[type].length + 1], // Add new field for the selected nudge type
    }));
  };

  return (
    <div className="nudge-layout">
      {/* Nudge Types Section at the Top */}
      <div className="nudge-types">
        <div>Nudge Type:</div>
        <div className="nudge-options">
          <div className="nudge-option" onClick={() => handleNudgeType('cc')}>
            <FaPlus /> <span>Call Context</span>
          </div>
          <div className="nudge-option" onClick={() => handleNudgeType('ai')}>
            <FaPlus /> <span>AI Guidance</span>
          </div>
          <div className="nudge-option" onClick={() => handleNudgeType('ss')}>
            <FaPlus /> <span>Speech Suggestion</span>
          </div>
          <div className="nudge-option" onClick={() => handleNudgeType('ka')}>
            <FaPlus /> <span>Knowledge Article</span>
          </div>
        </div>
      </div>

      <div className="main-content">
        {/* Left Column (Details Section) */}
        <div className="left-section">
          {/* Right Section will dynamically show the selected nudge fields */}
          {selectedNudge === 'cc' && (
            <div>
              <h3>Call Context Fields:</h3>
              {fields.cc.map((id) => (
                <div key={`cc-${id}`} className="row">
                  <div className="label">CC: Call Context Message {id}</div>
                  <input type="text" placeholder="Title" />
                </div>
              ))}
              <button className="styled-button" onClick={() => addField('cc')}>
                <FaPlus /> Add More Call Context Fields
              </button>
            </div>
          )}

          {selectedNudge === 'ai' && (
            <div>
              <h3>AI Guidance Fields:</h3>
              {fields.ai.map((id) => (
                <div key={`ai-${id}`} className="row">
                  <div className="label">AI: Message {id}</div>
                  <input type="text" placeholder="Message" />
                </div>
              ))}
              <button className="styled-button" onClick={() => addField('ai')}>
                <FaPlus /> Add More AI Guidance Fields
              </button>
            </div>
          )}

          {selectedNudge === 'ss' && (
            <div>
              <h3>Speech Suggestion Fields:</h3>
              {fields.ss.map((id) => (
                <div key={`ss-${id}`} className="row">
                  <div className="label">SS: Speech Suggestion Message {id}</div>
                  <input type="text" placeholder="Title" />
                </div>
              ))}
              <button className="styled-button" onClick={() => addField('ss')}>
                <FaPlus /> Add More Speech Suggestion Fields
              </button>
            </div>
          )}

          {selectedNudge === 'ka' && (
            <div>
              <h3>Knowledge Article Fields:</h3>
              {fields.ka.map((id) => (
                <div key={`ka-${id}`} className="row">
                  <div className="label">KA: Knowledge Article {id}</div>
                  <input type="text" placeholder="Title" />
                </div>
              ))}
              <button className="styled-button" onClick={() => addField('ka')}>
                <FaPlus /> Add More Knowledge Article Fields
              </button>
            </div>
          )}
        </div>

        {/* Right Column (Basic Info Section) */}
        <div className="right-section">
          <div className="row">
            <div className="label">Start Time</div>
            <input type="text" placeholder="Start Time" />
          </div>

          <div className="row">
            <div className="label">Customer or Agent</div>
            <input type="text" placeholder="Customer or Agent" />
          </div>

          <div className="row">
            <button className="styled-button">Add Nudge</button>
            <button className="styled-button">Add Utterance</button>
          </div>

          <div className="row">
            <div className="label">End Time</div>
            <input type="text" placeholder="End Time" />
          </div>
        </div>
      </div>
    </div>
  );
};

export default NudgeForm;
