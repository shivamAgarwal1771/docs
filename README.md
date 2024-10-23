import React, { useState } from 'react';
import { FaPlus, FaMinus } from 'react-icons/fa'; // Importing Plus and Minus icons
import './NudgeForm.css'; // Importing the updated CSS

const NudgeForm = () => {
  const [selectedNudge, setSelectedNudge] = useState('cc'); // Default to Call Context
  const [isUtteranceMode, setUtteranceMode] = useState(false); // To track whether we are in Utterance mode
  const [fields, setFields] = useState({
    cc: [],
    ai: [],
    ss: [],
    ka: [],
  });

  // Function to handle selecting a nudge type
  const handleNudgeType = (type) => {
    setSelectedNudge(type);
    setUtteranceMode(false); // Reset utterance mode when switching nudges
  };

  // Function to add fields dynamically based on nudge type
  const addField = (type) => {
    setFields((prev) => ({
      ...prev,
      [type]: [...prev[type], prev[type].length + 1], // Add new field for the selected nudge type
    }));
  };

  // Function to remove a field
  const removeField = (type, index) => {
    setFields((prev) => ({
      ...prev,
      [type]: prev[type].filter((_, i) => i !== index), // Remove field at the specified index
    }));
  };

  // Function to switch to utterance mode
  const handleAddUtterance = () => {
    setUtteranceMode(true);
    setSelectedNudge(''); // Reset nudge type when switching to utterance
  };

  // Handle switching back to Call Context if "Add Nudge" is clicked
  const handleAddNudge = () => {
    setUtteranceMode(false);
    setSelectedNudge('cc'); // Reset to Call Context
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
          {!isUtteranceMode && selectedNudge === 'cc' && (
            <div>
              <h3>Call Context Fields:</h3>
              {fields.cc.map((id, index) => (
                <div key={`cc-${id}`} className="row">
                  <div className="label">CC: Call Context Message {id}</div>
                  <input type="text" placeholder="Title" />
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

          {!isUtteranceMode && selectedNudge === 'ai' && (
            <div>
              <h3>AI Guidance Fields:</h3>
              {fields.ai.map((id, index) => (
                <div key={`ai-${id}`} className="row">
                  <div className="label">AI: Message {id}</div>
                  <input type="text" placeholder="Message" />
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

          {!isUtteranceMode && selectedNudge === 'ss' && (
            <div>
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

          {!isUtteranceMode && selectedNudge === 'ka' && (
            <div>
              <h3>Knowledge Article Fields:</h3>
              {fields.ka.map((id, index) => (
                <div key={`ka-${id}`} className="row">
                  <div className="label">KA: Knowledge Article {id}</div>
                  <input type="text" placeholder="Title" />
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

          {/* Utterance Mode */}
          {isUtteranceMode && (
            <div>
              <h3>Utterance Fields</h3>
              <div className="row">
                <div className="label">Start Time:</div>
                <input type="text" placeholder="Start Time" />
              </div>
              <div className="row">
                <div className="label">End Time:</div>
                <input type="text" placeholder="End Time" />
              </div>
              <div className="row">
                <div className="label">Speaker:</div>
                <input type="text" placeholder="Agent or Customer" />
              </div>
              <div className="row">
                <div className="label">Sentiment:</div>
                <div>
                  <input type="radio" name="sentiment" value="positive" /> Positive
                  <input type="radio" name="sentiment" value="neutral" /> Neutral
                  <input type="radio" name="sentiment" value="negative" /> Negative
                </div>
              </div>
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
            <button className="styled-button" onClick={handleAddNudge}>Add Nudge</button>
            <button className="styled-button" onClick={handleAddUtterance}>Add Utterance</button>
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
