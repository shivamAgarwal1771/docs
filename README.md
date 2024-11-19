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





import React, { useEffect, useState } from 'react';

const CallSummaryDetailsForm = ({metadata, handleInput }) => {
  const [contactFields, setContactFields] = useState([]);
  const [cases, setCases] = useState([]);
  const [interactions, setInteractions] = useState([]);
  const [selectedTab, setSelectedTab] = useState('Contact Card');


  useEffect(() => {
    handleInput("personalInformation", contactFields)
  }, [contactFields])

  useEffect(() => {
    handleInput("cases", cases)
  }, [cases])

  useEffect(() => {
    handleInput("interactionHistory", interactions)
  }, [interactions])

  const handleAddContactField = () => {
    setContactFields([...contactFields, { key: '', value: '' }]);
  };

  const handleRemoveContactField = (index) => {
    const updatedFields = contactFields.filter((_, i) => i !== index);
    setContactFields(updatedFields);
  };

  const handleFieldChange = (index, e) => {
    const { name, value } = e.target;
    const updatedFields = contactFields.map((field, i) =>
      i === index ? { ...field, [name]: value } : field
    );
    setContactFields(updatedFields);
  };

  const handleAddCase = () => {
    setCases([...cases, {
      caseNumber: '',
      creationDate: '',
      subject: '',
      priority: '',
      description: '',
      attachments: ['', ''],
      linkedCases: ['', '', ''],
      comments: [{ date: '', message: '' }],
      contactName:'',
      quickAction:'...'
    }]);
  };


  const handleCaseChange = (index, e) => {
    const { name, value } = e.target;
    const updatedCases = cases.map((caseItem, i) =>
      i === index ? { ...caseItem, [name]: value } : caseItem
    );
    setCases(updatedCases);
  };


  const handleRemoveCase = (index) => {
    const updatedCases = cases.filter((_, i) => i !== index);
    setCases(updatedCases);
  };

  const handleAddInteraction = () => {
    setInteractions([...interactions, { title: '', date: '', time: '', description: '' }]);
  };

  const handleInteractionChange = (index, e) => {
    const { name, value } = e.target;
    const updatedInteractions = interactions.map((interaction, i) =>
      i === index ? { ...interaction, [name]: value } : interaction
    );
    setInteractions(updatedInteractions);
  };

  const handleRemoveInteraction = (index) => {
    const updatedInteractions = interactions.filter((_, i) => i !== index);
    setInteractions(updatedInteractions);
  };

  return (
    <div>
      <div className="CallSummary-tabs">
        <button className="CallSummary-tab-button" onClick={() => setSelectedTab('Contact Card')}>Contact Card</button>
        <button className="CallSummary-tab-button" onClick={() => setSelectedTab('Cases')}>Cases</button>
        <button className="CallSummary-tab-button" onClick={() => setSelectedTab('Interaction History')}>Interaction History</button>
        <button className="CallSummary-tab-button" onClick={() => setSelectedTab('Customer 360')}>Customer 360</button>
      </div>

      {selectedTab === 'Contact Card' && (
        <div className="contact-card">
          <h2>Contact Card</h2>
          <button className="CallSummary-btn" onClick={handleAddContactField}>Add Customer Field</button>
          {contactFields.map((field, index) => (
            <div key={index} className="field-row">
              <input
                className="CallSummary-input-field"
                type="text"
                placeholder="Field"
                name="key"
                value={field.key}
                onChange={(e) => handleFieldChange(index, e)}
              />
              <input
                className="CallSummary-input-field"
                type="text"
                placeholder="Value"
                name="value"
                value={field.value}
                onChange={(e) => handleFieldChange(index, e)}
              />
              <button className="CallSummary-remove-button" onClick={() => handleRemoveContactField(index)}>Remove</button>
            </div>
          ))}
        </div>
      )}

      {/* Code for Cases and Interaction History can be updated similarly */}

      {selectedTab === 'Cases' && (
        <div className="CallSummary-cases">
          <h2>Cases</h2>
          <button className="CallSummary-btn" onClick={handleAddCase}>Add Case</button>
          {cases.map((caseItem, index) => (
            <div key={index} className="case-section">
              <input
                className="CallSummary-input-field"
                type="text"
                placeholder="Case ID"
                name="caseId"
                value={caseItem.caseId}
                onChange={(e) => handleCaseChange(index, e)}
              />
              <input
                className="CallSummary-input-field"
                type="date"
                placeholder="Creation Date"
                name="creationDate"
                value={caseItem.creationDate}
                onChange={(e) => handleCaseChange(index, e)}
              />
              <input
                className="CallSummary-input-field"
                type="text"
                placeholder="Subject"
                name="subject"
                value={caseItem.subject}
                onChange={(e) => handleCaseChange(index, e)}
              />
              <input
                className="CallSummary-input-field"
                type="text"
                placeholder="Priority"
                name="priority"
                value={caseItem.priority}
                onChange={(e) => handleCaseChange(index, e)}
              />
              <textarea
                className="CallSummary-input-field"
                placeholder="Description"
                name="description"
                value={caseItem.description}
                onChange={(e) => handleCaseChange(index, e)}
              />
              <button className="CallSummary-remove-button" onClick={() => handleRemoveCase(index)}>Remove Case</button>
            </div>
          ))}

        </div>
      )}

      {selectedTab === 'Interaction History' && (
        <div className="CallSummary-interaction-history">
          <h2>Interaction History</h2>
          <button className="CallSummary-btn" onClick={handleAddInteraction}>Add Interaction</button>
          {interactions.map((interaction, index) => (
            <div key={index} className="CallSummary-interaction-section">
              <input
                className="CallSummary-input-field"
                type="text"
                placeholder="Title"
                name="title"
                value={interaction.title}
                onChange={(e) => handleInteractionChange(index, e)}
              />
              <input
                className="CallSummary-input-field"
                type="date"
                placeholder="Date"
                name="date"
                value={interaction.date}
                onChange={(e) => handleInteractionChange(index, e)}
              />
              <input
                className="CallSummary-input-field"
                type="time"
                placeholder="Time"
                name="time"
                value={interaction.time}
                onChange={(e) => handleInteractionChange(index, e)}
              />
              <textarea
                className="CallSummary-input-field"
                placeholder="Description"
                name="description"
                value={interaction.description}
                onChange={(e) => handleInteractionChange(index, e)}
              />
              <button className="CallSummary-remove-button" onClick={() => handleRemoveInteraction(index)}>Remove Interaction</button>
            </div>
          ))}
        </div>
      )}
      {selectedTab === 'Customer 360' && (
        <div className="customer-360-app">
          {/* Customer 360 Panel content here */}
        </div>
      )}

    </div>
  );
};

export default CallSummaryDetailsForm;
