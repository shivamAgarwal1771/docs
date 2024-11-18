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
    // Constructing the updated data object and passing it to the parent component
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
          {contactFields.map((_, index) => (
            <div key={index} className="field-row">
              <input
                className="CallSummary-input-field"
                type="text"
                placeholder="Field"
                value={_.field}
                onChange={(e) => handleFieldChange('Contact Card', index, 'field', e.target.value)}
              />
              <input
                className="CallSummary-input-field"
                type="text"
                placeholder="Value"
                value={_.value}
                onChange={(e) => handleFieldChange('Contact Card', index, 'value', e.target.value)}
              />
              <button className="CallSummary-remove-button" onClick={() => handleRemoveContactField(index)}>Remove</button>
            </div>
          ))}
        </div>
      )}

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
              <div className="CallSummary-attachments">
                <h4>Attachments</h4>
                {caseItem.attachments.map((_, i) => (
                  <input
                    key={i}
                    className="CallSummary-input-field"
                    type="text"
                    placeholder={`Attachment ${i + 1}`}
                    value={caseItem.attachments[i]}
                    onChange={(e) => handleFieldChange('Cases', index, `attachment_${i}`, e.target.value)}
                  />
                ))}
              </div>
              <div className="CallSummary-linked">
                <h4>Linked</h4>
                {caseItem.linked.map((_, i) => (
                  <input
                    key={i}
                    className="CallSummary-input-field"
                    type="text"
                    placeholder={`Linked ${i + 1}`}
                    value={caseItem.linked[i]}
                    onChange={(e) => handleFieldChange('Cases', index, `linked_${i}`, e.target.value)}
                  />
                ))}
              </div>
              <div className="case-comments">
                <h4>Case Comments</h4>
                {caseItem.caseComments.map((comment, i) => (
                  <div key={i} className="comment-row">
                    <input
                      className="CallSummary-input-field"
                      type="date"
                      placeholder="Date"
                      value={comment.date}
                      onChange={(e) => handleFieldChange('Cases', index, `comment_${i}_date`, e.target.value)}
                    />
                    <textarea
                      className="CallSummary-input-field"
                      placeholder="Message"
                      value={comment.message}
                      onChange={(e) => handleFieldChange('Cases', index, `comment_${i}_message`, e.target.value)}
                    />
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

      {selectedTab === 'Customer 360' && (
        <div className="customer-360-app">
          <h1>C360 View</h1>
          <Panel
            title="Risk Assessment"
            initialFields={[{ id: 1, name: "", value: "" }]}
            addFieldLabel="Add Field"
            fieldType="field"
            onFieldChange={onFieldChange}
          />
          <Panel
            title="Account Info"
            initialFields={[{ id: 2, name: "", value: "" }]}
            addFieldLabel="Add Sub Header"
            fieldType="field"
            onFieldChange={onFieldChange}
          />
          <Panel
            title="Next Best Action"
            initialFields={[{ id: 3, name: "Data Point 1", value: "" }]}
            addFieldLabel="Add Data Point"
            fieldType="data"
            onFieldChange={onFieldChange}
          />
          <Panel
            title="Life Events"
            initialFields={[{ id: 4, name: "Date", value: "" }]}
            addFieldLabel="Add Field"
            fieldType="field"
            onFieldChange={onFieldChange}
          />
          <Panel
            title="Marketing Analysis"
            initialFields={[{ id: 5, name: "", value: "" }]}
            addFieldLabel="Add Field"
            fieldType="field"
            onFieldChange={onFieldChange}
          />
        </div>
      )}
    </div>
  );
};

export default CallSummaryDetailsForm;

const Panel = ({ title, initialFields, addFieldLabel, fieldType, onFieldChange }) => {
  const [fields, setFields] = useState(initialFields);

  const addField = () => {
    const newField = { id: Date.now(), name: "", value: "" };
    setFields([...fields, newField]);
    onFieldChange(title, newField.id, newField);
  };

  const removeField = (id) => {
    const updatedFields = fields.filter((field) => field.id !== id);
    setFields(updatedFields);
    onFieldChange(title, id, null);
  };

  const handleFieldChange = (id, fieldName, value) => {
    const updatedFields = fields.map(field => 
      field.id === id ? { ...field, [fieldName]: value } : field
    );
    setFields(updatedFields);
    onFieldChange(title, id, { field: fieldName, value });
  };

  return (
    <div className="customer-360-panel">
      <h3>{title}</h3>
      {fields.map((field) => (
        <div key={field.id}>
          <input
            type="text"
            placeholder={fieldType === "data" ? "Data Point" : "Field"}
            className="customer-360-field-input"
            value={field.name}
            onChange={(e) => handleFieldChange(field.id, 'name', e.target.value)}
          />
          <input
            type="text"
            placeholder="Value"
            className="customer-360-value-input"
            value={field.value}
            onChange={(e) => handleFieldChange(field.id, 'value', e.target.value)}
          />
          <button
            className="customer-360-remove-btn"
            onClick={() => removeField(field.id)}
          >
            ×
          </button>
          <button className="customer-360-add-btn" onClick={addField}>
            {addFieldLabel}
          </button>
        </div>
      ))}
    </div>
  );
};
