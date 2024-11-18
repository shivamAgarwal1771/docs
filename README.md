import React, { useState } from 'react';

const CallSummaryDetailsForm = ({ onUpdate }) => {
  const [contactFields, setContactFields] = useState([]);
  const [cases, setCases] = useState([]);
  const [interactions, setInteractions] = useState([]);
  const [customer360, setCustomer360] = useState([]);
  const [selectedTab, setSelectedTab] = useState('Contact Card');

  const handleUpdate = (key, value) => {
    switch (key) {
      case 'contactFields':
        setContactFields(value);
        break;
      case 'cases':
        setCases(value);
        break;
      case 'interactions':
        setInteractions(value);
        break;
      case 'customer360':
        setCustomer360(value);
        break;
      default:
        break;
    }
    onUpdate(key, value);
  };

  const handleAddField = (type) => {
    const newItem =
      type === 'contactFields'
        ? { field: '', value: '' }
        : type === 'cases'
        ? {
            caseId: '',
            creationDate: '',
            subject: '',
            priority: '',
            description: '',
            attachments: [''],
            linked: [''],
            caseComments: [{ date: '', message: '' }],
          }
        : type === 'interactions'
        ? { title: '', date: '', time: '', description: '' }
        : null;

    if (newItem) {
      handleUpdate(type, [...(type === 'contactFields' ? contactFields : type === 'cases' ? cases : interactions), newItem]);
    }
  };

  const handleRemoveField = (type, index) => {
    const updatedItems = (type === 'contactFields'
      ? contactFields
      : type === 'cases'
      ? cases
      : interactions
    ).filter((_, i) => i !== index);

    handleUpdate(type, updatedItems);
  };

  return (
    <div>
      <div className="CallSummary-tabs">
        <button onClick={() => setSelectedTab('Contact Card')}>Contact Card</button>
        <button onClick={() => setSelectedTab('Cases')}>Cases</button>
        <button onClick={() => setSelectedTab('Interaction History')}>Interaction History</button>
        <button onClick={() => setSelectedTab('Customer 360')}>Customer 360</button>
      </div>

      {selectedTab === 'Contact Card' && (
        <div>
          <h2>Contact Card</h2>
          <button onClick={() => handleAddField('contactFields')}>Add Customer Field</button>
          {contactFields.map((field, index) => (
            <div key={index}>
              <input
                type="text"
                placeholder="Field"
                value={field.field}
                onChange={(e) => {
                  const updated = [...contactFields];
                  updated[index].field = e.target.value;
                  handleUpdate('contactFields', updated);
                }}
              />
              <input
                type="text"
                placeholder="Value"
                value={field.value}
                onChange={(e) => {
                  const updated = [...contactFields];
                  updated[index].value = e.target.value;
                  handleUpdate('contactFields', updated);
                }}
              />
              <button onClick={() => handleRemoveField('contactFields', index)}>Remove</button>
            </div>
          ))}
        </div>
      )}

      {selectedTab === 'Cases' && (
        <div>
          <h2>Cases</h2>
          <button onClick={() => handleAddField('cases')}>Add Case</button>
          {cases.map((caseItem, index) => (
            <div key={index}>
              <input
                type="text"
                placeholder="Case ID"
                value={caseItem.caseId}
                onChange={(e) => {
                  const updated = [...cases];
                  updated[index].caseId = e.target.value;
                  handleUpdate('cases', updated);
                }}
              />
              <input
                type="date"
                placeholder="Creation Date"
                value={caseItem.creationDate}
                onChange={(e) => {
                  const updated = [...cases];
                  updated[index].creationDate = e.target.value;
                  handleUpdate('cases', updated);
                }}
              />
              <input
                type="text"
                placeholder="Subject"
                value={caseItem.subject}
                onChange={(e) => {
                  const updated = [...cases];
                  updated[index].subject = e.target.value;
                  handleUpdate('cases', updated);
                }}
              />
              <input
                type="text"
                placeholder="Priority"
                value={caseItem.priority}
                onChange={(e) => {
                  const updated = [...cases];
                  updated[index].priority = e.target.value;
                  handleUpdate('cases', updated);
                }}
              />
              <textarea
                placeholder="Description"
                value={caseItem.description}
                onChange={(e) => {
                  const updated = [...cases];
                  updated[index].description = e.target.value;
                  handleUpdate('cases', updated);
                }}
              />
              <button onClick={() => handleRemoveField('cases', index)}>Remove Case</button>
            </div>
          ))}
        </div>
      )}

      {selectedTab === 'Interaction History' && (
        <div>
          <h2>Interaction History</h2>
          <button onClick={() => handleAddField('interactions')}>Add Interaction</button>
          {interactions.map((interaction, index) => (
            <div key={index}>
              <input
                type="text"
                placeholder="Title"
                value={interaction.title}
                onChange={(e) => {
                  const updated = [...interactions];
                  updated[index].title = e.target.value;
                  handleUpdate('interactions', updated);
                }}
              />
              <input
                type="date"
                placeholder="Date"
                value={interaction.date}
                onChange={(e) => {
                  const updated = [...interactions];
                  updated[index].date = e.target.value;
                  handleUpdate('interactions', updated);
                }}
              />
              <input
                type="time"
                placeholder="Time"
                value={interaction.time}
                onChange={(e) => {
                  const updated = [...interactions];
                  updated[index].time = e.target.value;
                  handleUpdate('interactions', updated);
                }}
              />
              <textarea
                placeholder="Description"
                value={interaction.description}
                onChange={(e) => {
                  const updated = [...interactions];
                  updated[index].description = e.target.value;
                  handleUpdate('interactions', updated);
                }}
              />
              <button onClick={() => handleRemoveField('interactions', index)}>Remove Interaction</button>
            </div>
          ))}
        </div>
      )}

      {selectedTab === 'Customer 360' && (
        <div>
          <h2>Customer 360</h2>
          <Panel
            title="Customer 360 Panel"
            fields={customer360}
            onUpdate={(updatedFields) => handleUpdate('customer360', updatedFields)}
            addFieldLabel="Add Field"
          />
        </div>
      )}
    </div>
  );
};

const Panel = ({ title, fields, onUpdate, addFieldLabel }) => {
  const handleUpdateField = (id, key, value) => {
    const updatedFields = fields.map((field) =>
      field.id === id ? { ...field, [key]: value } : field
    );
    onUpdate(updatedFields);
  };

  const addField = () => {
    const newField = { id: Date.now(), name: '', value: '' };
    onUpdate([...fields, newField]);
  };

  const removeField = (id) => {
    const updatedFields = fields.filter((field) => field.id !== id);
    onUpdate(updatedFields);
  };

  return (
    <div>
      <h3>{title}</h3>
      {fields.map((field) => (
        <div key={field.id}>
          <input
            type="text"
            placeholder="Field"
            value={field.name}
            onChange={(e) => handleUpdateField(field.id, 'name', e.target.value)}
          />
          <input
            type="text"
            placeholder="Value"
            value={field.value}
            onChange={(e) => handleUpdateField(field.id, 'value', e.target.value)}
          />
          <button onClick={() => removeField(field.id)}>Remove</button>
        </div>
      ))}
      <button onClick={addField}>{addFieldLabel}</button>
    </div>
  );
};

const ParentComponent = () => {
  const [formData, setFormData] = useState({
    contactFields: [],
    cases: [],
    interactions: [],
    customer360: [],
  });

  const handleUpdate = (key, value) => {
    setFormData((prev) => ({ ...prev, [key]: value }));
  };

  return (
    <div>
      <h1>Parent Component</h1>
      <CallSummaryDetailsForm onUpdate={handleUpdate} />
      <pre>{JSON.stringify(formData, null, 2)}</pre>
    </div>
  );
};

export default ParentComponent;
