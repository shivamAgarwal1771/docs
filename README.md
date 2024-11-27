import React, { useEffect, useState } from 'react';
import Customer360 from './Customer360';
import { IoCloseSharp } from "react-icons/io5";

const CallSummaryDetailsForm = ({ metadata, handleInput }) => {
  const [contactFields, setContactFields] = useState([]);
  const [cases, setCases] = useState([]);
  const [interactions, setInteractions] = useState([]);
  const [selectedTab, setSelectedTab] = useState('Contact Card');
  const [caseTabs, setCaseTabs] = useState([]); // Track dynamic case tabs
  const [interactionTabs, setInteractionTabs] = useState([]); // Track dynamic interaction history tabs

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

  const handleAddCase = () => {
    const newCaseIndex = caseTabs.length;
    const newCase = {
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
    };
    
    setCases([...cases, newCase]);
    setCaseTabs([...caseTabs, `Case ${newCaseIndex + 1}`]); // Add new tab name
    setSelectedTab(`Case ${newCaseIndex + 1}`); // Set this new case tab as selected
  };

  const handleAddInteraction = () => {
    const newInteractionIndex = interactionTabs.length;
    const newInteraction = { title: '', date: '', time: '', description: '' };
    
    setInteractions([...interactions, newInteraction]);
    setInteractionTabs([...interactionTabs, `Interaction ${newInteractionIndex + 1}`]); // Add new tab name
    setSelectedTab(`Interaction ${newInteractionIndex + 1}`); // Set this new interaction tab as selected
  };

  const handleRemoveTab = (tabType, index) => {
    if (tabType === 'case') {
      const updatedCases = cases.filter((_, i) => i !== index);
      setCases(updatedCases);
      const updatedCaseTabs = caseTabs.filter((_, i) => i !== index);
      setCaseTabs(updatedCaseTabs);
    } else if (tabType === 'interaction') {
      const updatedInteractions = interactions.filter((_, i) => i !== index);
      setInteractions(updatedInteractions);
      const updatedInteractionTabs = interactionTabs.filter((_, i) => i !== index);
      setInteractionTabs(updatedInteractionTabs);
    }
    setSelectedTab(tabType === 'case' ? (caseTabs[0] || '') : (interactionTabs[0] || ''));
  };

  const handleCaseChange = (index, e) => {
    const { name, value } = e.target;
    const updatedCases = cases.map((caseItem, i) =>
      i === index ? { ...caseItem, [name]: value } : caseItem
    );
    setCases(updatedCases);
  };

  const handleInteractionChange = (index, e) => {
    const { name, value } = e.target;
    const updatedInteractions = interactions.map((interaction, i) =>
      i === index ? { ...interaction, [name]: value } : interaction
    );
    setInteractions(updatedInteractions);
  };

  return (
    <div>
      <div className="CallSummary-tabs">
        <button className="CallSummary-tab-button" onClick={() => setSelectedTab('Contact Card')}>Contact Card</button>
        {caseTabs.map((tabName, index) => (
          <button key={index} className="CallSummary-tab-button" onClick={() => setSelectedTab(tabName)}>
            {tabName}
            <IoCloseSharp
              className="close-btn-icon"
              onClick={(e) => {
                e.stopPropagation();
                handleRemoveTab('case', index);
              }}
            />
          </button>
        ))}
        {interactionTabs.map((tabName, index) => (
          <button key={index} className="CallSummary-tab-button" onClick={() => setSelectedTab(tabName)}>
            {tabName}
            <IoCloseSharp
              className="close-btn-icon"
              onClick={(e) => {
                e.stopPropagation();
                handleRemoveTab('interaction', index);
              }}
            />
          </button>
        ))}
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

      {caseTabs.includes(selectedTab) && (
        <div className="CallSummary-cases">
          {caseTabs.map((tabName, index) => (
            selectedTab === tabName && (
              <div className="margin-tb-20" key={index}>
                <button className="resolution-remove-btn" onClick={() => handleRemoveCase(index)}>
                  <IoCloseSharp className="close-btn-icon" />
                </button>
                <div className="field-column border-box padding-10 rounded-border gap-10">
                  <div className="field-row gap-10">
                    <input
                      className="CallSummary-input-field call-info-input-width rounded-border"
                      type="text"
                      placeholder="CaseNumber"
                      name="caseNumber"
                      value={cases[index]?.caseNumber}
                      onChange={(e) => handleCaseChange(index, e)}
                    />
                    <input
                      className="CallSummary-input-field call-info-input-width rounded-border"
                      type="date"
                      placeholder="Creation Date"
                      name="creationDate"
                      value={cases[index]?.creationDate}
                      onChange={(e) => handleCaseChange(index, e)}
                    />
                  </div>
                  <div className="field-row gap-10">
                    <input
                      className="CallSummary-input-field call-info-input-width rounded-border"
                      type="text"
                      placeholder="Subject"
                      name="subject"
                      value={cases[index]?.subject}
                      onChange={(e) => handleCaseChange(index, e)}
                    />
                    <select
                      className="CallSummary-input-field call-info-input-width rounded-border"
                      name="priority"
                      value={cases[index]?.priority}  
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
                    value={cases[index]?.description}
                    onChange={(e) => handleCaseChange(index, e)}
                  />
                  <input
                    className="CallSummary-input-field rounded-border"
                    type="text"
                    placeholder="quickAction"
                    name="quickAction"
                    value={cases[index]?.quickAction}
                    onChange={(e) => handleCaseChange(index, e)}
                  />
                </div>
              </div>
            )
          ))}
        </div>
      )}

      {interactionTabs.includes(selectedTab) && (
        <div className="CallSummary-interaction-history">
          {interactionTabs.map((tabName, index) => (
            selectedTab === tabName && (
              <div key={index}>
                <button className="resolution-remove-btn" onClick={() => handleRemoveInteraction(index)}>
                  <IoCloseSharp className="close-btn-icon" />
                </button>
                <div className="field-row gap-10">
                  <input
                    className="CallSummary-input-field call-info-input-width rounded-border"
                    type="text"
                    placeholder="Title"
                    name="title"
                    value={interactions[index]?.title}
                    onChange={(e) => handleInteractionChange(index, e)}
                  />
                  <input
                    className="CallSummary-input-field call-info-input-width rounded-border"
                    type="date"
                    placeholder="Date"
                    name="date"
                    value={interactions[index]?.date}
                    onChange={(e) => handleInteractionChange(index, e)}
                  />
                  <input
                    className="CallSummary-input-field call-info-input-width rounded-border"
                    type="time"
                    placeholder="Time"
                    name="time"
                    value={interactions[index]?.time}
                    onChange={(e) => handleInteractionChange(index, e)}
                  />
                </div>
                <textarea
                  className="CallSummary-input-field rounded-border"
                  placeholder="Description"
                  name="description"
                  value={interactions[index]?.description}
                  onChange={(e) => handleInteractionChange(index, e)}
                />
              </div>
            )
          ))}
        </div>
      )}

      {selectedTab === 'Customer 360' && (
        <Customer360 metadata={metadata} handleInput={handleInput} />
      )}
    </div>
  );
};

export default CallSummaryDetailsForm;
