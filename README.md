import React, { useEffect, useState } from 'react';

const CallSummaryTabs = ({handleSubmit}) => {
  const [selectedTab, setSelectedTab] = useState('Call Info');
  const [callInfos, setCallInfos] = useState([]);
  const [callNotes, setCallNotes] = useState([]);
  const [resolutions, setResolutions] = useState([]);

  const handleAddCallInfo = () => {
    setCallInfos([...callInfos, { intent: '', repeatCall: '', incomingTime: '', duration: '' }]);
  };
 useEffect(()=>{
console.log(callInfos,"test666")
 },[callInfos])
  const handleRemoveCallInfo = (index) => {
    const updatedInfos = callInfos.filter((_, i) => i !== index);
    setCallInfos(updatedInfos);
  };

  const handleAddCallNote = () => {
    setCallNotes([...callNotes, { note: '' }]);
  };

  const handleRemoveCallNote = (index) => {
    const updatedNotes = callNotes.filter((_, i) => i !== index);
    setCallNotes(updatedNotes);
  };

  const handleAddResolution = () => {
    setResolutions([...resolutions, { question: '', options: [] }]);
  };

  const handleAddOption = (index) => {
    const updatedResolutions = [...resolutions];
    updatedResolutions[index].options.push('');
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
          {/* <button className="tab-button" onClick={() => setSelectedTab('Call Notes')}>Call Notes</button>
        <button className="tab-button" onClick={() => setSelectedTab('Resolution')}>Resolution</button> */}
        </div>
        <div className="call-info">
          {/* <button className="btn" onClick={handleAddCallInfo}>+</button> */}
          {callInfos.map((info, index) => (
            <div className="call-summary-subcontainer">
              <div key={index} className="call-info-section">
                <input className="input-field" type="text" placeholder="Intent Captured" />
                <select className="input-field" value={info.repeatCall} onChange={(e) => {
                  const updatedInfos = [...callInfos];
                  updatedInfos[index].repeatCall = e.target.value;
                  setCallInfos(updatedInfos);
                }}>
                  <option value="">Repeat Call</option>
                  <option value="Yes">Yes</option>
                  <option value="No">No</option>
                </select>
                <input className="input-field" type="time" placeholder="Incoming Time" />
                <input className="input-field" type="text" placeholder="Call Duration" />
              </div>
              <div>
                <button className="summary-remove-button" onClick={() => handleRemoveCallInfo(index)}>
                  <span>R<br />E<br />M<br />O<br />V<br />E</span>
                </button>
              </div>
            </div>
          ))}
        </div>

        {/* {selectedTab === 'Call Info' && (
          <div className="call-info">
            <button className="btn" onClick={handleAddCallInfo}>Add Call Info</button>
            {callInfos.map((info, index) => (
              <div key={index} className="call-info-section">
                <input className="input-field" type="text" placeholder="Intent Captured" />
                <select className="input-field" value={info.repeatCall} onChange={(e) => {
                  const updatedInfos = [...callInfos];
                  updatedInfos[index].repeatCall = e.target.value;
                  setCallInfos(updatedInfos);
                }}>
                  <option value="">Repeat Call</option>
                  <option value="Yes">Yes</option>
                  <option value="No">No</option>
                </select>
                <input className="input-field" type="time" placeholder="Incoming Time" />
                <input className="input-field" type="text" placeholder="Call Duration" />
                <button className="remove-button" onClick={() => handleRemoveCallInfo(index)}>Remove</button>
              </div>
            ))}
          </div>
        )}

        {selectedTab === 'Call Notes' && (
          <div className="call-notes">
            <button className="btn" onClick={handleAddCallNote}>Add Call Note</button>
            {callNotes.map((note, index) => (
              <div key={index} className="call-note-section">
                <textarea className="input-field" placeholder="Call Note"></textarea>
                <button className="remove-button" onClick={() => handleRemoveCallNote(index)}>Remove</button>
              </div>
            ))}
          </div>
        )}

        {selectedTab === 'Resolution' && (
          <div className="resolution">
            <button className="btn" onClick={handleAddResolution}>Add Resolution</button>
            {resolutions.map((resolution, resIndex) => (
              <div key={resIndex} className="resolution-section">
                <input className="input-field" type="text" placeholder="Question" />
                <button className="btn" onClick={() => handleAddOption(resIndex)}>Add Option</button>
                {resolution.options.map((option, optIndex) => (
                  <div key={optIndex} className="option-section">
                    <input className="input-field" type="text" placeholder="Option" />
                    <button className="remove-button" onClick={() => handleRemoveOption(resIndex, optIndex)}>Remove Option</button>
                  </div>
                ))}
                <button className="remove-button" onClick={() => handleRemoveResolution(resIndex)}>Remove Resolution</button>
              </div>
            ))}
          </div>
        )} */}
      </div>
      <div className="call-summary-tabs">
        <div className="tabs">
          <span className="tab-button">Call Note</span>
          <button className="btn" onClick={handleAddCallNote}>+</button>
        </div>
        <div className="call-notes">
          {callNotes.map((note, index) => (
            <div className="call-summary-subcontainer">
              <div key={index} className="call-note-section">
                <textarea className="input-field" placeholder="Call Note"></textarea>
              </div>
              <div>
                <button className="summary-remove-button" onClick={() => handleRemoveCallNote(index)}>
                  <span>R<br />E<br />M<br />O<br />V<br />E</span>
                </button>
              </div>
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
            <div className="call-summary-subcontainer">
              <div key={resIndex} className="resolution-section">
                <input className="input-field" type="text" placeholder="Question" />
                <button className="btn" onClick={() => handleAddOption(resIndex)}>Add Option</button>
                <div className="option-container">
                  {resolution.options.map((option, optIndex) => (
                    <div key={optIndex} className="option-section">
                      <input className="input-field" type="text" placeholder="Option" />
                      <button className="cancel-option" onClick={() => handleRemoveOption(resIndex, optIndex)}>X</button>
                    </div>
                  ))}
                </div>
              </div>
              <div>
                <button className="summary-remove-button" onClick={() => handleRemoveResolution(resIndex)}>
                  <span>R<br />E<br />M<br />O<br />V<br />E</span>
                </button>
              </div>
            </div>
          ))}
        </div>
        <button type='submit' onClick={()=>handleSubmit()}>Submit</button>
      </div>
    </>
  );
};

export default CallSummaryTabs;
