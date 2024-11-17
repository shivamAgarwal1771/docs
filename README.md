import React, { useState, useEffect } from 'react';

const CallSummaryTabs = ({ onDataChange }) => {
  const [callInfos, setCallInfos] = useState([]);
  const [callNotes, setCallNotes] = useState([]);
  const [resolutions, setResolutions] = useState([]);

  useEffect(() => {
    onDataChange({ callInfos, callNotes, resolutions });
  }, [callInfos, callNotes, resolutions, onDataChange]);

  const handleFieldChange = (setter, index, field, value) => {
    setter((prev) =>
      prev.map((item, i) => (i === index ? { ...item, [field]: value } : item))
    );
  };

  const handleAddCallInfo = () => {
    setCallInfos([...callInfos, { intent: '', repeatCall: '', incomingTime: '', duration: '' }]);
  };

  const handleRemoveCallInfo = (index) => {
    setCallInfos(callInfos.filter((_, i) => i !== index));
  };

  const handleAddCallNote = () => {
    setCallNotes([...callNotes, { note: '' }]);
  };

  const handleRemoveCallNote = (index) => {
    setCallNotes(callNotes.filter((_, i) => i !== index));
  };

  const handleAddResolution = () => {
    setResolutions([...resolutions, { question: '', options: [] }]);
  };

  const handleAddOption = (resIndex) => {
    setResolutions((prev) => {
      const updated = [...prev];
      updated[resIndex].options.push('');
      return updated;
    });
  };

  const handleRemoveResolution = (index) => {
    setResolutions(resolutions.filter((_, i) => i !== index));
  };

  const handleRemoveOption = (resIndex, optIndex) => {
    setResolutions((prev) => {
      const updated = [...prev];
      updated[resIndex].options = updated[resIndex].options.filter((_, i) => i !== optIndex);
      return updated;
    });
  };

  return (
    <>
      {/* Call Info Section */}
      <div className="call-summary-tabs">
        <div className="tabs">
          <span className="tab-button">Call Info</span>
          <button className="btn" onClick={handleAddCallInfo}>+</button>
        </div>
        <div className="call-info">
          {callInfos.map((info, index) => (
            <div className="call-summary-subcontainer" key={index}>
              <div className="call-info-section">
                <input
                  className="input-field"
                  type="text"
                  placeholder="Intent Captured"
                  value={info.intent}
                  onChange={(e) => handleFieldChange(setCallInfos, index, 'intent', e.target.value)}
                />
                <select
                  className="input-field"
                  value={info.repeatCall}
                  onChange={(e) => handleFieldChange(setCallInfos, index, 'repeatCall', e.target.value)}
                >
                  <option value="">Repeat Call</option>
                  <option value="Yes">Yes</option>
                  <option value="No">No</option>
                </select>
                <input
                  className="input-field"
                  type="time"
                  value={info.incomingTime}
                  onChange={(e) => handleFieldChange(setCallInfos, index, 'incomingTime', e.target.value)}
                />
                <input
                  className="input-field"
                  type="text"
                  placeholder="Call Duration"
                  value={info.duration}
                  onChange={(e) => handleFieldChange(setCallInfos, index, 'duration', e.target.value)}
                />
              </div>
              <button className="summary-remove-button" onClick={() => handleRemoveCallInfo(index)}>
                Remove
              </button>
            </div>
          ))}
        </div>
      </div>

      {/* Call Notes Section */}
      <div className="call-summary-tabs">
        <div className="tabs">
          <span className="tab-button">Call Notes</span>
          <button className="btn" onClick={handleAddCallNote}>+</button>
        </div>
        <div className="call-notes">
          {callNotes.map((note, index) => (
            <div className="call-summary-subcontainer" key={index}>
              <textarea
                className="input-field"
                placeholder="Call Note"
                value={note.note}
                onChange={(e) => handleFieldChange(setCallNotes, index, 'note', e.target.value)}
              ></textarea>
              <button className="summary-remove-button" onClick={() => handleRemoveCallNote(index)}>
                Remove
              </button>
            </div>
          ))}
        </div>
      </div>

      {/* Resolution Section */}
      <div className="call-summary-tabs">
        <div className="tabs">
          <span className="tab-button">Resolution</span>
          <button className="btn" onClick={handleAddResolution}>+</button>
        </div>
        <div className="resolution">
          {resolutions.map((resolution, resIndex) => (
            <div className="call-summary-subcontainer" key={resIndex}>
              <input
                className="input-field"
                type="text"
                placeholder="Question"
                value={resolution.question}
                onChange={(e) => handleFieldChange(setResolutions, resIndex, 'question', e.target.value)}
              />
              <button className="btn" onClick={() => handleAddOption(resIndex)}>Add Option</button>
              <div className="option-container">
                {resolution.options.map((option, optIndex) => (
                  <div className="option-section" key={optIndex}>
                    <input
                      className="input-field"
                      type="text"
                      placeholder="Option"
                      value={option}
                      onChange={(e) => {
                        const updatedResolutions = [...resolutions];
                        updatedResolutions[resIndex].options[optIndex] = e.target.value;
                        setResolutions(updatedResolutions);
                      }}
                    />
                    <button className="cancel-option" onClick={() => handleRemoveOption(resIndex, optIndex)}>X</button>
                  </div>
                ))}
              </div>
              <button className="summary-remove-button" onClick={() => handleRemoveResolution(resIndex)}>
                Remove
              </button>
            </div>
          ))}
        </div>
      </div>
    </>
  );
};

export default CallSummaryTabs;
