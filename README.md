import React, { useEffect, useState } from 'react';
import Customer360 from './Customer360';
import { IoCloseSharp } from "react-icons/io5";

const CallSummaryDetailsForm = ({ metadata, handleInput }) => {
  const [contactFields, setContactFields] = useState([]);
  const [cases, setCases] = useState([]);
  const [interactions, setInteractions] = useState([]);
  const [selectedTab, setSelectedTab] = useState('Contact Card'); // Default tab
  const [activeCaseTab, setActiveCaseTab] = useState(null); // Track the active case tab

  // Initialize fields from metadata only if no user changes are made
  useEffect(() => {
    if (!contactFields.length && metadata?.personalInformation) {
      setContactFields(metadata.personalInformation);
    }
  }, [metadata?.personalInformation, contactFields]);

  useEffect(() => {
    if (!cases.length && metadata?.cases) {
      setCases(metadata.cases);
    }
  }, [metadata?.cases, cases]);

  useEffect(() => {
    if (!interactions.length && metadata?.interactionHistory) {
      setInteractions(metadata.interactionHistory);
    }
  }, [metadata?.interactionHistory, interactions]);

  // Sync updated fields with the parent component
  useEffect(() => {
    handleInput("personalInformation", contactFields);
  }, [contactFields, handleInput]);

  useEffect(() => {
    handleInput("cases", cases);
  }, [cases, handleInput]);

  useEffect(() => {
    handleInput("interactionHistory", interactions);
  }, [interactions, handleInput]);

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
    setCases([
      ...cases,
      {
        caseNumber: '',
        creationDate: '',
        subject: '',
        priority: '',
        description: '',
        attachments: ['', ''],
        linkedCases: ['', '', ''],
        comments: [{ date: '', message: '' }],
        contactName: '',
        quickAction: '...',
      },
    ]);
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

  const handleCaseTabClick = (index) => {
    // Set the active case tab without resetting the selectedTab
    setActiveCaseTab(index);
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
          <div className="contact-card-header">
            <h2><b>Contact Card</b></h2>
            <button className="add-fields-btn" onClick={handleAddContactField}>+ Add</button>
          </div>
          <div className="overflow-y-scroll">
            {contactFields.map((field, index) => (
              <div key={index} className="field-row margin-tb-20">
                <input
                  className="CallSummary-input-field left-curved-input call-info-input-width"
                  type="text"
                  placeholder="Field"
                  name="key"
                  value={field.key}
                  onChange={(e) => handleFieldChange(index, e)}
                />
                <input
                  className="CallSummary-input-field call-info-input-width"
                  type="text"
                  placeholder="Value"
                  name="value"
                  value={field.value}
                  onChange={(e) => handleFieldChange(index, e)}
                />
                <button className="section-remove-btn" onClick={() => handleRemoveContactField(index)}>
                  <IoCloseSharp className="close-btn-icon" />
                </button>
              </div>
            ))}
          </div>
        </div>
      )}

      {selectedTab === 'Cases' && (
        <div className="CallSummary-cases">
          <div className="contact-card-header">
            <h2><b>Cases</b></h2>
            <button className="add-fields-btn" onClick={handleAddCase}>+ Add</button>
          </div>
          <div className="tabs">
            {cases.map((_, index) => (
              <button
                key={index}
                className={`tab-button ${activeCaseTab === index ? 'active' : ''}`}
                onClick={() => handleCaseTabClick(index)} // This now only updates the active case tab
              >
                Case {index + 1}
              </button>
            ))}
          </div>

          <div className="overflow-y-scroll">
            {cases.map((caseItem, index) => (
              <div
                key={index}
                className={`case-tab-content ${activeCaseTab === index ? 'active' : ''}`}
              >
                {activeCaseTab === index && (
                  <div className="field-column border-box padding-10 rounded-border gap-10">
                    <div className="field-row gap-10">
                      <input
                        className="CallSummary-input-field call-info-input-width rounded-border"
                        type="text"
                        placeholder="CaseNumber"
                        name="caseNumber"
                        value={caseItem.caseNumber}
                        onChange={(e) => handleCaseChange(index, e)}
                      />
                      <input
                        className="CallSummary-input-field call-info-input-width rounded-border"
                        type="date"
                        placeholder="Creation Date"
                        name="creationDate"
                        value={caseItem.creationDate}
                        onChange={(e) => handleCaseChange(index, e)}
                      />
                    </div>
                    <div className="field-row gap-10">
                      <input
                        className="CallSummary-input-field call-info-input-width rounded-border"
                        type="text"
                        placeholder="Subject"
                        name="subject"
                        value={caseItem.subject}
                        onChange={(e) => handleCaseChange(index, e)}
                      />
                      <select
                        className="CallSummary-input-field call-info-input-width rounded-border"
                        name="priority"
                        value={caseItem.priority}
                        onChange={(e) => handleCaseChange(index, e)}
                      >
                        <option value="Low">Low</option>
                        <option value="Mid">Mid</option>
                        <option value="High">High</option>
                      </select>
                    </div>
                    <textarea
                      className="CallSummary-input-field rounded-border"
                      placeholder="Description"
                      name="description"
                      value={caseItem.description}
                      onChange={(e) => handleCaseChange(index, e)}
                    />
                    <input
                      className="CallSummary-input-field rounded-border"
                      type="text"
                      placeholder="Quick Action"
                      name="quickAction"
                      value={caseItem.quickAction}
                      onChange={(e) => handleCaseChange(index, e)}
                    />
                  </div>
                )}
              </div>
            ))}
          </div>
        </div>
      )}

      {selectedTab === 'Interaction History' && (
        <div className="CallSummary-interaction-history">
          <div className="contact-card-header">
            <h2><b>Interaction History</b></h2>
            <button className="add-fields-btn" onClick={handleAddInteraction}>+ Add</button>
          </div>
          <div className="overflow-y-scroll">
            {interactions.map((interaction, index) => (
              <div className="margin-tb-20" key={index}>
                <button className="resolution-remove-btn" onClick={() => handleRemoveInteraction(index)}>
                  <IoCloseSharp className="close-btn-icon" />
                </button>
                <div className="field-row gap-10">
                  <input
                    className="CallSummary-input-field call-info-input-width rounded-border"
                    type="text"
                    placeholder="Title"
                    name="title"
                    value={interaction.title}
                    onChange={(e) => handleInteractionChange(index, e)}
                  />
                  <input
                    className="CallSummary-input-field call-info-input-width rounded-border"
                    type="date"
                    placeholder="Date"
                    name="date"
                    value={interaction.date}
                    onChange={(e) => handleInteractionChange(index, e)}
                  />
                  <input
                    className="CallSummary-input-field call-info-input-width rounded-border"
                    type="time"
                    placeholder="Time"
                    name="time"
                    value={interaction.time}
                    onChange={(e) => handleInteractionChange(index, e)}
                  />
                </div>
                <textarea
                  className="CallSummary-input-field rounded-border"
                  placeholder="Description"
                  name="description"
                  value={interaction.description}
                  onChange={(e) => handleInteractionChange(index, e)}
                />
              </div>
            ))}
          </div>
        </div>
      )}

      {selectedTab === 'Customer 360' && (
        <Customer360 metadata={metadata} handleInput={handleInput} />
      )}
    </div>
  );
};

export default CallSummaryDetailsForm;
