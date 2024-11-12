import React, { useState } from 'react';

const CallSummaryDetailsForm = () => {
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
      <div className="CallSummary-tabs">
        <button className="CallSummary-tab-button" onClick={() => setSelectedTab('Contact Card')}>Contact Card</button>
        <button className="CallSummary-tab-button" onClick={() => setSelectedTab('Cases')}>Cases</button>
        <button className="CallSummary-tab-button" onClick={() => setSelectedTab('Interaction History')}>Interaction History</button>
      </div>

      {selectedTab === 'Contact Card' && (
        <div className="contact-card">
          <h2>Contact Card</h2>
          <button className="CallSummary-btn" onClick={handleAddContactField}>Add Customer Field</button>
          {contactFields.map((_, index) => (
            <div key={index} className="field-row">
              <input className="CallSummary-input-field" type="text" placeholder="Field" />
              <input className="CallSummary-input-field" type="text" placeholder="Value" />
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
              <input className="CallSummary-input-field" type="text" placeholder="Case ID" />
              <input className="CallSummary-input-field" type="date" placeholder="Creation Date" />
              <input className="CallSummary-input-field" type="text" placeholder="Subject" />
              <input className="CallSummary-input-field" type="text" placeholder="Priority" />
              <textarea className="CallSummary-input-field" placeholder="Description"></textarea>

              <div className="CallSummary-attachments">
                <h4>Attachments</h4>
                {caseItem.attachments.map((_, i) => (
                  <input key={i} className="CallSummary-input-field" type="text" placeholder={`Attachment ${i + 1}`} />
                ))}
              </div>

              <div className="CallSummary-linked">
                <h4>Linked</h4>
                {caseItem.linked.map((_, i) => (
                  <input key={i} className="CallSummary-input-field" type="text" placeholder={`Linked ${i + 1}`} />
                ))}
              </div>

              <div className="case-comments">
                <h4>Case Comments</h4>
                {caseItem.caseComments.map((comment, i) => (
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
    </div>
  );
};

export default CallSummaryDetailsForm;


/* Styles for the tabs */
.CallSummary-tabs {
    display: flex;
    align-items: center;
    justify-content: space-between;
    gap: 10px;
    margin-bottom: 10px;
}

.CallSummary-tab-button {
    /* padding: 10px 20px; */
    /* border: none; */
    /* border-radius: 5px; */
    /* background-color: #773D87; */
    color: #000;
    font-weight: bold;
    /* cursor: pointer; */
    transition: background-color 0.3s;
    font-size: 16px;
}

/* .tab-button:hover {
    background-color: #773D87;
    color: white;
} */

.CallSummary-tab-button:focus {
    outline: none;
}

/* General styles for the form sections */
.contact-card, .CallSummary-cases, .CallSummary-interaction-history {
    background-color: #f9f9f9;
    padding: 20px;
    border-radius: 8px;
    box-shadow: 0 2px 8px rgba(0, 0, 0, 0.1);
    margin-bottom: 20px;
}

/* h2 {
    margin-bottom: 20px;
    font-size: 24px;
} */

/* Styles for the fields */
.field-row, .case-section, .interaction-section {
    display: flex;
    flex-direction: column;
    gap: 12px;
    margin-bottom: 20px;
}

.CallSummary-input-field {
    padding: 12px;
    border: 1px solid #ccc;
    border-radius: 4px;
    width: 100%;
    box-sizing: border-box;
    font-size: 14px;
}

textarea {
    resize: vertical;
}

/* Button styles */
.CallSummary-btn {
    padding: 5px 15px;
    border: none;
    border-radius: 5px;
    /* background-color: rgb(6, 224, 6); */
    border: 2px solid rgb(6, 224, 6) !important;
    color: #000;
    font-weight: bold;
    cursor: pointer;
    transition: background-color 0.3s;
    font-size: 24px;
}

.CallSummary-btn:hover {
    background-color: rgb(6, 224, 6);
    border: none;
}

.CallSummary-btn:focus {
    outline: none;
}

.CallSummary-remove-button {
    /* background-color: #000000; */
    color: white;
    padding: 10px 20px;
    width: 30rem;
    justify-content: center;
    justify-items: center;
}

/* .remove-button:hover {
    background-color: #cfa2a7;
    color: black;
} */

/* Specific styles for case sections */
.CallSummary-attachments, .CallSummary-linked, .case-comments {
    margin-top: 16px;
}

h4 {
    margin-bottom: 10px;
    font-size: 18px;
}

/* Styles for individual input boxes */
.CallSummary-attachments input,
.CallSummary-linked input,
.case-comments .comment-row input,
.case-comments .comment-row textarea {
    margin-bottom: 10px;
}

.case-comments .comment-row {
    display: flex;
    flex-direction: column;
    gap: 8px;
}
