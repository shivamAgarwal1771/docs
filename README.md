import React, { useState } from 'react';
import { FaPlus, FaMinus } from 'react-icons/fa'; // Importing Plus and Minus icons

const NudgeForm = () => {
  const [selectedNudge, setSelectedNudge] = useState('cc'); // Default to Call Context
  const [isUtteranceMode, setUtteranceMode] = useState(false); // To track whether we are in Utterance mode
  const [fields, setFields] = useState({
    cc: [],
    ai: [],
    ss: [],
    ka: [],
  });
  const [buttonFields, setButtonFields] = useState([]); // To track dynamic button fields for AI guidance

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

  // Function to add a dynamic button field in AI guidance
  const addButtonField = () => {
    setButtonFields((prev) => [...prev, { title: '', text: '' }]);
  };

  // Function to handle input changes for button fields
  const handleButtonFieldChange = (index, field, value) => {
    setButtonFields((prev) => {
      const newFields = [...prev];
      newFields[index][field] = value;
      return newFields;
    });
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
      <div className="nudge-types" style={{ textAlign: 'center', fontWeight: 'bold' }}>
        <div style={{ textTransform: 'uppercase', fontSize: '20px' }}>Nudge Type:</div>
        <div className="nudge-options">
          <div className="nudge-option" onClick={() => handleNudgeType('cc')}>
            <span>Call Context</span>
          </div>
          <div className="nudge-option" onClick={() => handleNudgeType('ai')}>
            <span>AI Guidance</span>
          </div>
          <div className="nudge-option" onClick={() => handleNudgeType('ss')}>
            <span>Speech Suggestion</span>
          </div>
          <div className="nudge-option" onClick={() => handleNudgeType('ka')}>
            <span>Knowledge Article</span>
          </div>
        </div>
      </div>

      <div className="main-content">
        {/* Left Column (Details Section) */}
        <div className="left-section">
          {/* Call Context Fields */}
          {!isUtteranceMode && selectedNudge === 'cc' && (
            <div>
              <h3 style={{ fontSize: '24px', fontWeight: 'bold' }}>Call Context Fields:</h3>
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
          {!isUtteranceMode && selectedNudge === 'ai' && (
            <div>
              <h3 style={{ fontSize: '24px', fontWeight: 'bold' }}>AI Guidance Fields:</h3>
              <div className="row">
                <div className="label">Title:</div>
                <input type="text" placeholder="Title" />
              </div>
              <div className="row">
                <div className="label">Subtitle:</div>
                <input type="text" placeholder="Subtitle" />
              </div>
              <div className="row">
                <div className="label">Start Time:</div>
                <input type="text" placeholder="Start Time" />
              </div>
              <div className="row">
                <div className="label">End Time:</div>
                <input type="text" placeholder="End Time" />
              </div>
              {buttonFields.map((field, index) => (
                <div key={index} className="row">
                  <div className="label">Button {index + 1}:</div>
                  <input
                    type="text"
                    placeholder="Button Title"
                    value={field.title}
                    onChange={(e) => handleButtonFieldChange(index, 'title', e.target.value)}
                  />
                  <input
                    type="text"
                    placeholder="Button Text"
                    value={field.text}
                    onChange={(e) => handleButtonFieldChange(index, 'text', e.target.value)}
                  />
                  <button className="styled-button remove-button" onClick={() => handleButtonFieldChange(index, 'remove', null)}>
                    <FaMinus /> Remove
                  </button>
                </div>
              ))}
              <button className="styled-button" onClick={addButtonField}>
                <FaPlus /> Add More Button Fields
              </button>
            </div>
          )}

          {/* Speech Suggestion Fields */}
          {!isUtteranceMode && selectedNudge === 'ss' && (
            <div>
              <h3 style={{ fontSize: '24px', fontWeight: 'bold' }}>Speech Suggestion Fields:</h3>
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
          {!isUtteranceMode && selectedNudge === 'ka' && (
            <div>
              <h3 style={{ fontSize: '24px', fontWeight: 'bold' }}>Knowledge Article Fields:</h3>
              {fields.ka.map((id, index) => (
                <div key={`ka-${id}`} className="row">
                  <div className="label">KA: Knowledge Article {id}</div>
                  <input type="text" placeholder="Title" />
                  <div className="label">Pointer:</div>
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

          {/* Utterance Mode */}
          {isUtteranceMode && (
            <div>
              <h3 style={{ fontSize: '24px', fontWeight: 'bold' }}>Utterance Fields</h3>
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
