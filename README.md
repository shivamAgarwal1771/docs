import React, { useState } from 'react';

const DynamicForm = () => {
  const [contactFields, setContactFields] = useState([]);
  const [cases, setCases] = useState([]);
  const [interactions, setInteractions] = useState([]);
  const [selectedTab, setSelectedTab] = useState('Contact Card');

  const handleAddContactField = () => {
    setContactFields([...contactFields, { field: '', value: '' }]);
  };

  const handleRemoveContactField = (index) => {
    const updatedFields = contactFields.filter((_, i) => i !== index);
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
      <div className="tabs">
        <button className="tab-button" onClick={() => setSelectedTab('Contact Card')}>Contact Card</button>
        <button className="tab-button" onClick={() => setSelectedTab('Cases')}>Cases</button>
        <button className="tab-button" onClick={() => setSelectedTab('Interaction History')}>Interaction History</button>
      </div>

      {selectedTab === 'Contact Card' && (
        <div className="contact-card">
          <h2>Contact Card</h2>
          <button className="btn" onClick={handleAddContactField}>Add Customer Field</button>
          {contactFields.map((_, index) => (
            <div key={index} className="field-row">
              <input className="input-field" type="text" placeholder="Field" />
              <input className="input-field" type="text" placeholder="Value" />
              <button className="remove-button" onClick={() => handleRemoveContactField(index)}>Remove</button>
            </div>
          ))}
        </div>
      )}

      {selectedTab === 'Cases' && (
        <div className="cases">
          <h2>Cases</h2>
          <button className="btn" onClick={handleAddCase}>Add Case</button>
          {cases.map((caseItem, index) => (
            <div key={index} className="case-section">
              <input className="input-field" type="text" placeholder="Case ID" />
              <input className="input-field" type="date" placeholder="Creation Date" />
              <input className="input-field" type="text" placeholder="Subject" />
              <input className="input-field" type="text" placeholder="Priority" />
              <textarea className="input-field" placeholder="Description"></textarea>

              <div className="attachments">
                <h4>Attachments</h4>
                {caseItem.attachments.map((_, i) => (
                  <input key={i} className="input-field" type="text" placeholder={`Attachment ${i + 1}`} />
                ))}
              </div>

              <div className="linked">
                <h4>Linked</h4>
                {caseItem.linked.map((_, i) => (
                  <input key={i} className="input-field" type="text" placeholder={`Linked ${i + 1}`} />
                ))}
              </div>

              <div className="case-comments">
                <h4>Case Comments</h4>
                {caseItem.caseComments.map((comment, i) => (
                  <div key={i} className="comment-row">
                    <input className="input-field" type="date" placeholder="Date" />
                    <textarea className="input-field" placeholder="Message"></textarea>
                  </div>
                ))}
              </div>

              <button className="remove-button" onClick={() => handleRemoveCase(index)}>Remove Case</button>
            </div>
          ))}
        </div>
      )}

      {selectedTab === 'Interaction History' && (
        <div className="interaction-history">
          <h2>Interaction History</h2>
          <button className="btn" onClick={handleAddInteraction}>Add Interaction</button>
          {interactions.map((interaction, index) => (
            <div key={index} className="interaction-section">
              <input className="input-field" type="text" placeholder="Title" />
              <input className="input-field" type="date" placeholder="Date" />
              <input className="input-field" type="time" placeholder="Time" />
              <textarea className="input-field" placeholder="Description"></textarea>
              <button className="remove-button" onClick={() => handleRemoveInteraction(index)}>Remove Interaction</button>
            </div>
          ))}
        </div>
      )}
    </div>
  );
};

export default DynamicForm;
