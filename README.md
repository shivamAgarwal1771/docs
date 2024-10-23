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

  // Function to handle selecting a nudge type
  const handleNudgeType = (type) => {
    setSelectedNudge(type);
    setUtteranceMode(false); // Reset utterance mode when switching nudges
  };

  // Function to add fields dynamically based on nudge type
  const addField = (type) => {
    setFields((prev) => ({
      ...prev,
      [type]: [
        ...prev[type],
        { id: prev[type].length + 1, title: '', subtitle: '', startTime: '', endTime: '', buttonFields: [] },
      ],
    }));
  };

  // Function to add button fields dynamically for AI Guidance
  const addButtonField = (aiFieldIndex) => {
    setFields((prev) => {
      const newAiFields = [...prev.ai];
      newAiFields[aiFieldIndex].buttonFields.push({ buttonTitle: '', buttonText: '' });
      return { ...prev, ai: newAiFields };
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
      <div className="nudge-types">
        <div className="nudge-types-header">NUDGE TYPE:</div>
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
        {/* Left Column (Details Section) */}
        <div className="left-section">
          {!isUtteranceMode && selectedNudge === 'cc' && (
            <div>
              <h3 className="section-title">Call Context Fields:</h3>
              {fields.cc.map((id, index) => (
                <div key={`cc-${id}`} className="field-section">
                  <div className="row">
                    <div className="label">CC: Call Context Message {id}</div>
                    <input type="text" placeholder="Title" />
                    <input type="text" placeholder="Call Context Message" />
                    <button className="styled-button remove-button" onClick={() => removeField('cc', index)}>
                      <FaMinus /> Remove
                    </button>
                  </div>
                </div>
              ))}
              <button className="styled-button" onClick={() => addField('cc')}>
                <FaPlus /> Add More Call Context Fields
              </button>
            </div>
          )}

          {!isUtteranceMode && selectedNudge === 'ai' && (
            <div>
              <h3 className="section-title">AI Guidance Fields:</h3>
              {fields.ai.map((field, index) => (
                <div key={`ai-${field.id}`} className="field-section">
                  <div className="row">
                    <div className="label">Title:</div>
                    <input
                      type="text"
                      value={field.title}
                      onChange={(e) => {
                        const updatedFields = [...fields.ai];
                        updatedFields[index].title = e.target.value;
                        setFields({ ...fields, ai: updatedFields });
                      }}
                    />
                  </div>
                  <div className="row">
                    <div className="label">Subtitle:</div>
                    <input
                      type="text"
                      value={field.subtitle}
                      onChange={(e) => {
                        const updatedFields = [...fields.ai];
                        updatedFields[index].subtitle = e.target.value;
                        setFields({ ...fields, ai: updatedFields });
                      }}
                    />
                  </div>
                  <button className="styled-button" onClick={() => addButtonField(index)}>
                    Add Button
                  </button>
                  {field.buttonFields.map((buttonField, buttonIndex) => (
                    <div key={`ai-button-${buttonIndex}`} className="row">
                      <div className="label">Button Title:</div>
                      <input
                        type="text"
                        value={buttonField.buttonTitle}
                        onChange={(e) => {
                          const updatedFields = [...fields.ai];
                          updatedFields[index].buttonFields[buttonIndex].buttonTitle = e.target.value;
                          setFields({ ...fields, ai: updatedFields });
                        }}
                      />
                      <div className="label">Button Text:</div>
                      <input
                        type="text"
                        value={buttonField.buttonText}
                        onChange={(e) => {
                          const updatedFields = [...fields.ai];
                          updatedFields[index].buttonFields[buttonIndex].buttonText = e.target.value;
                          setFields({ ...fields, ai: updatedFields });
                        }}
                      />
                    </div>
                  ))}
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
              <h3 className="section-title">Speech Suggestion Fields:</h3>
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
              <h3 className="section-title">Knowledge Article Fields:</h3>
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
              <h3 className="section-title">Utterance Fields</h3>
              <div className="row">
                <div className="label">Start Time:</div>
                <input type="text" placeholder="Start Time" />
              </div>
              <div className="row">
                <div className="label">End Time:</div>
                <input type="text" placeholder="End Time" />
              </div>
              <button className="styled-button" onClick={handleAddNudge}>
                <FaPlus /> Add Nudge
              </button>
            </div>
          )}

          {/* Button to switch to Utterance mode */}
          {!isUtteranceMode && (
            <button className="styled-button" onClick={handleAddUtterance}>
              Add Utterance
            </button>
          )}
        </div>

        {/* Right Column (Basic Info Section) */}
        <div className="right-section">
          {/* Other fields or additional content can go here */}
        </div>
      </div>
    </div>
  );
};

export default NudgeForm;
