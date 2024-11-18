import React, { useEffect, useState } from 'react';

const CallSummaryDetailsForm = ({handleInput}) => {
  const [contactFields, setContactFields] = useState([]);
  const [cases, setCases] = useState([]);
  const [interactions, setInteractions] = useState([]);
  const [selectedTab, setSelectedTab] = useState('Contact Card');


  useEffect(()=>{
    handleInput("contact",contactFields)
  },[contactFields])

  useEffect(()=>{
    handleInput("cases",cases)
  },[cases])

  useEffect(()=>{
    handleInput("interactions",interactions)
  },[interactions])

  const handleAddContactField = () => {
    setContactFields([...contactFields, { field: '', value: '' }]);
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
      caseId: '',
      creationDate: '',
      subject: '',
      priority: '',
      description: '',
      attachments: ['', ''],
      linked: ['', '', ''],
      caseComments: [{ date: '', message: '' }]
    }]);
  };

  const handleRemoveCase = (index) => {
    const updatedCases = cases.filter((_, i) => i !== index);
    setCases(updatedCases);
  };

  const handleAddInteraction = () => {
    setInteractions([...interactions, { title: '', date: '', time: '', description: '' }]);
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
                name="field"
                value={field.field}
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
              <input className="CallSummary-input-field" type="text" placeholder="Case ID" />
              <input className="CallSummary-input-field" type="date" placeholder="Creation Date" />
              <input className="CallSummary-input-field" type="text" placeholder="Subject" />
              <input className="CallSummary-input-field" type="text" placeholder="Priority" />
              <textarea className="CallSummary-input-field" placeholder="Description"></textarea>

              <div className="CallSummary-attachments">
                <h4>Attachments</h4>
                {caseItem?.attachments?.map((_, i) => (
                  <input key={i} className="CallSummary-input-field" type="text" placeholder={`Attachment ${i + 1}`} />
                ))}
              </div>

              <div className="CallSummary-linked">
                <h4>Linked</h4>
                {caseItem?.linked?.map((_, i) => (
                  <input key={i} className="CallSummary-input-field" type="text" placeholder={`Linked ${i + 1}`} />
                ))}
              </div>

              <div className="case-comments">
                <h4>Case Comments</h4>
                {caseItem?.caseComments?.map((comment, i) => (
                  <div key={i} className="comment-row">
                    <input className="CallSummary-input-field" type="date" placeholder="Date" />
                    <textarea className="CallSummary-input-field" placeholder="Message"></textarea>
                  </div>
                ))}
              </div>

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
              <input className="CallSummary-input-field" type="text" placeholder="Title" />
              <input className="CallSummary-input-field" type="date" placeholder="Date" />
              <input className="CallSummary-input-field" type="time" placeholder="Time" />
              <textarea className="CallSummary-input-field" placeholder="Description"></textarea>
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
