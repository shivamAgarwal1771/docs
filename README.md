import React, { useEffect, useState } from 'react';

const CallSummaryTabs = ({ handleSubmit, onDataChange }) => {
  const [selectedTab, setSelectedTab] = useState('Call Info');
  const [callInfos, setCallInfos] = useState([]);
  const [callNotes, setCallNotes] = useState([]);
  const [resolutions, setResolutions] = useState([]);

  // Update parent with new data
  useEffect(() => {
    onDataChange({ callInfos, callNotes, resolutions });
  }, [callInfos, callNotes, resolutions, onDataChange]);

  const handleAddCallInfo = () => {
    setCallInfos([...callInfos, { intent: '', repeatCall: '', incomingTime: '', duration: '' }]);
  };

  const handleUpdateCallInfo = (index, key, value) => {
    const updatedInfos = [...callInfos];
    updatedInfos[index][key] = value;
    setCallInfos(updatedInfos);
  };

  const handleRemoveCallInfo = (index) => {
    const updatedInfos = callInfos.filter((_, i) => i !== index);
    setCallInfos(updatedInfos);
  };

  const handleAddCallNote = () => {
    setCallNotes([...callNotes, { note: '' }]);
  };

  const handleUpdateCallNote = (index, value) => {
    const updatedNotes = [...callNotes];
    updatedNotes[index].note = value;
    setCallNotes(updatedNotes);
  };

  const handleRemoveCallNote = (index) => {
    const updatedNotes = callNotes.filter((_, i) => i !== index);
    setCallNotes(updatedNotes);
  };

  const handleAddResolution = () => {
    setResolutions([...resolutions, { question: '', options: [] }]);
  };

  const handleUpdateResolution = (index, key, value) => {
    const updatedResolutions = [...resolutions];
    updatedResolutions[index][key] = value;
    setResolutions(updatedResolutions);
  };

  const handleAddOption = (index) => {
    const updatedResolutions = [...resolutions];
    updatedResolutions[index].options.push('');
    setResolutions(updatedResolutions);
  };

  const handleUpdateOption = (resIndex, optIndex, value) => {
    const updatedResolutions = [...resolutions];
    updatedResolutions[resIndex].options[optIndex] = value;
    setResolutions(updatedResolutions);
  };

  const handleRemoveResolution = (index) => {
    const updatedResolutions = resolutions.filter((_, i) => i !== index);
    setResolutions(updatedResolutions);
  };

  const handleRemoveOption = (resIndex, optIndex) => {
    const updatedResolutions = [...resolutions];
    updatedResolutions[resIndex].options = updatedResolutions[resIndex].options.filter((_, i) => i !== optIndex);
    setResolutions(updatedResolutions);
  };

  return (
    <>
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
                  onChange={(e) => handleUpdateCallInfo(index, 'intent', e.target.value)}
                />
                <select
                  className="input-field"
                  value={info.repeatCall}
                  onChange={(e) => handleUpdateCallInfo(index, 'repeatCall', e.target.value)}
                >
                  <option value="">Repeat Call</option>
                  <option value="Yes">Yes</option>
                  <option value="No">No</option>
                </select>
                <input
                  className="input-field"
                  type="time"
                  value={info.incomingTime}
                  onChange={(e) => handleUpdateCallInfo(index, 'incomingTime', e.target.value)}
                />
                <input
                  className="input-field"
                  type="text"
                  placeholder="Call Duration"
                  value={info.duration}
                  onChange={(e) => handleUpdateCallInfo(index, 'duration', e.target.value)}
                />
              </div>
              <button className="summary-remove-button" onClick={() => handleRemoveCallInfo(index)}>Remove</button>
            </div>
          ))}
        </div>
      </div>

      <div className="call-summary-tabs">
        <div className="tabs">
          <span className="tab-button">Call Note</span>
          <button className="btn" onClick={handleAddCallNote}>+</button>
        </div>
        <div className="call-notes">
          {callNotes.map((note, index) => (
            <div className="call-summary-subcontainer" key={index}>
              <textarea
                className="input-field"
                placeholder="Call Note"
                value={note.note}
                onChange={(e) => handleUpdateCallNote(index, e.target.value)}
              />
              <button className="summary-remove-button" onClick={() => handleRemoveCallNote(index)}>Remove</button>
            </div>
          ))}
        </div>
      </div>

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
                onChange={(e) => handleUpdateResolution(resIndex, 'question', e.target.value)}
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
                      onChange={(e) => handleUpdateOption(resIndex, optIndex, e.target.value)}
                    />
                    <button className="cancel-option" onClick={() => handleRemoveOption(resIndex, optIndex)}>X</button>
                  </div>
                ))}
              </div>
              <button className="summary-remove-button" onClick={() => handleRemoveResolution(resIndex)}>Remove</button>
            </div>
          ))}
        </div>
      </div>

      <button type="submit" onClick={handleSubmit}>Submit</button>
    </>
  );
};

export default CallSummaryTabs;
