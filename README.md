import React, { useState } from 'react';

const CallSummaryDetailsForm = ({ onFieldChange }) => {
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

  const handleFieldChange = (tab, index, fieldName, value) => {
    // Update the state directly when a field value changes
    if (tab === 'Contact Card') {
      const updatedFields = [...contactFields];
      updatedFields[index][fieldName] = value;
      setContactFields(updatedFields);
    }
    if (tab === 'Cases') {
      const updatedCases = [...cases];
      updatedCases[index][fieldName] = value;
      setCases(updatedCases);
    }
    if (tab === 'Interaction History') {
      const updatedInteractions = [...interactions];
      updatedInteractions[index][fieldName] = value;
      setInteractions(updatedInteractions);
    }
    // Call the parent method to pass the changes up
    onFieldChange(tab, index, { field: fieldName, value });
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
                value={field.field} // Bind value to state
                onChange={(e) => handleFieldChange('Contact Card', index, 'field', e.target.value)} // Handle field change
              />
              <input
                className="CallSummary-input-field"
                type="text"
                placeholder="Value"
                value={field.value} // Bind value to state
                onChange={(e) => handleFieldChange('Contact Card', index, 'value', e.target.value)} // Handle value change
              />
              <button className="CallSummary-remove-button" onClick={() => handleRemoveContactField(index)}>Remove</button>
            </div>
          ))}
        </div>
      )}

      {/* Cases Tab */}
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
                value={caseItem.caseId}
                onChange={(e) => handleFieldChange('Cases', index, 'caseId', e.target.value)}
              />
              <input
                className="CallSummary-input-field"
                type="date"
                placeholder="Creation Date"
                value={caseItem.creationDate}
                onChange={(e) => handleFieldChange('Cases', index, 'creationDate', e.target.value)}
              />
              <input
                className="CallSummary-input-field"
                type="text"
                placeholder="Subject"
                value={caseItem.subject}
                onChange={(e) => handleFieldChange('Cases', index, 'subject', e.target.value)}
              />
              <input
                className="CallSummary-input-field"
                type="text"
                placeholder="Priority"
                value={caseItem.priority}
                onChange={(e) => handleFieldChange('Cases', index, 'priority', e.target.value)}
              />
              <textarea
                className="CallSummary-input-field"
                placeholder="Description"
                value={caseItem.description}
                onChange={(e) => handleFieldChange('Cases', index, 'description', e.target.value)}
              ></textarea>
              <button className="CallSummary-remove-button" onClick={() => handleRemoveCase(index)}>Remove Case</button>
            </div>
          ))}
        </div>
      )}

      {/* Interaction History Tab */}
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
                value={interaction.title}
                onChange={(e) => handleFieldChange('Interaction History', index, 'title', e.target.value)}
              />
              <input
                className="CallSummary-input-field"
                type="date"
                placeholder="Date"
                value={interaction.date}
                onChange={(e) => handleFieldChange('Interaction History', index, 'date', e.target.value)}
              />
              <input
                className="CallSummary-input-field"
                type="time"
                placeholder="Time"
                value={interaction.time}
                onChange={(e) => handleFieldChange('Interaction History', index, 'time', e.target.value)}
              />
              <textarea
                className="CallSummary-input-field"
                placeholder="Description"
                value={interaction.description}
                onChange={(e) => handleFieldChange('Interaction History', index, 'description', e.target.value)}
              ></textarea>
              <button className="CallSummary-remove-button" onClick={() => handleRemoveInteraction(index)}>Remove Interaction</button>
            </div>
          ))}
        </div>
      )}

      {/* Customer 360 Tab */}
      {selectedTab === 'Customer 360' && (
        <div className="customer-360-app">
          <h1>C360 View</h1>
          {/* Custom Panels go here */}
        </div>
      )}
    </div>
  );
};

export default CallSummaryDetailsForm;
