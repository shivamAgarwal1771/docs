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
        <button onClick={() => setSelectedTab('Contact Card')}>Contact Card</button>
        <button onClick={() => setSelectedTab('Cases')}>Cases</button>
        <button onClick={() => setSelectedTab('Interaction History')}>Interaction History</button>
      </div>

      {selectedTab === 'Contact Card' && (
        <div className="contact-card">
          <h2>Contact Card</h2>
          <button onClick={handleAddContactField}>Add Customer Field</button>
          {contactFields.map((_, index) => (
            <div key={index} className="field-row">
              <input type="text" placeholder="Field" />
              <input type="text" placeholder="Value" />
              <button onClick={() => handleRemoveContactField(index)}>Remove</button>
            </div>
          ))}
        </div>
      )}

      {selectedTab === 'Cases' && (
        <div className="cases">
          <h2>Cases</h2>
          <button onClick={handleAddCase}>Add Case</button>
          {cases.map((caseItem, index) => (
            <div key={index} className="case-section">
              <input type="text" placeholder="Case ID" />
              <input type="date" placeholder="Creation Date" />
              <input type="text" placeholder="Subject" />
              <input type="text" placeholder="Priority" />
              <textarea placeholder="Description"></textarea>

              <div className="attachments">
                <h4>Attachments</h4>
                {caseItem.attachments.map((_, i) => (
                  <input key={i} type="text" placeholder={`Attachment ${i + 1}`} />
                ))}
              </div>

              <div className="linked">
                <h4>Linked</h4>
                {caseItem.linked.map((_, i) => (
                  <input key={i} type="text" placeholder={`Linked ${i + 1}`} />
                ))}
              </div>

              <div className="case-comments">
                <h4>Case Comments</h4>
                {caseItem.caseComments.map((comment, i) => (
                  <div key={i} className="comment-row">
                    <input type="date" placeholder="Date" />
                    <textarea placeholder="Message"></textarea>
                  </div>
                ))}
              </div>

              <button onClick={() => handleRemoveCase(index)}>Remove Case</button>
            </div>
          ))}
        </div>
      )}

      {selectedTab === 'Interaction History' && (
        <div className="interaction-history">
          <h2>Interaction History</h2>
          <button onClick={handleAddInteraction}>Add Interaction</button>
          {interactions.map((interaction, index) => (
            <div key={index} className="interaction-section">
              <input type="text" placeholder="Title" />
              <input type="date" placeholder="Date" />
              <input type="time" placeholder="Time" />
              <textarea placeholder="Description"></textarea>
              <button onClick={() => handleRemoveInteraction(index)}>Remove Interaction</button>
            </div>
          ))}
        </div>
      )}
    </div>
  );
};

export default DynamicForm;




/* Styles for the tabs */
.tabs {
  display: flex;
  gap: 10px;
  margin-bottom: 20px;
}

.tabs button {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  background-color: #007bff;
  color: #fff;
  cursor: pointer;
  transition: background-color 0.3s;
}

.tabs button:hover {
  background-color: #0056b3;
}

.tabs button:focus {
  outline: none;
}

/* General styles for the form sections */
.contact-card, .cases, .interaction-history {
  background-color: #f9f9f9;
  padding: 20px;
  border-radius: 8px;
  box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
  margin-bottom: 20px;
}

h2 {
  margin-bottom: 16px;
  font-size: 24px;
}

/* Styles for the fields */
.field-row, .case-section, .interaction-section {
  display: flex;
  flex-direction: column;
  gap: 10px;
  margin-bottom: 20px;
}

input[type="text"],
input[type="date"],
input[type="time"],
textarea {
  padding: 10px;
  border: 1px solid #ccc;
  border-radius: 4px;
  width: 100%;
  box-sizing: border-box;
}

textarea {
  resize: vertical;
}

button {
  padding: 8px 16px;
  border: none;
  border-radius: 4px;
  background-color: #28a745;
  color: #fff;
  cursor: pointer;
  transition: background-color 0.3s;
}

button:hover {
  background-color: #218838;
}

button:focus {
  outline: none;
}

/* Specific styles for case sections */
.attachments, .linked, .case-comments {
  margin-top: 16px;
}

h4 {
  margin-bottom: 10px;
  font-size: 18px;
}

/* Styles for individual input boxes */
.attachments input,
.linked input,
.case-comments .comment-row input,
.case-comments .comment-row textarea {
  margin-bottom: 10px;
}

.case-comments .comment-row {
  display: flex;
  flex-direction: column;
  gap: 8px;
}

.remove-button {
  background-color: #dc3545;
}

.remove-button:hover {
  background-color: #c82333;
}
