import React, { useEffect, useState } from 'react';
import { FaChevronDown, FaChevronUp } from 'react-icons/fa6';
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
      status: 'Open',
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

  const renderCaseDetails = (index) => (
    <div className="case-details">
      <div>
        <input
          className="upload-demo-input"
          placeholder="Case Number"
          value={cases[index]?.caseNumber}
          onChange={(e) => handleCaseChange(index, e)}
        />
        <select
          className="upload-demo-input"
          value={cases[index]?.priority}
          onChange={(e) => handleCaseChange(index, e)}
        >
          <option hidden>Select Priority</option>
          <option value="Low">Low</option>
          <option value="Medium">Medium</option>
          <option value="High">High</option>
        </select>
        <label htmlFor={`creation-date-${index}`}>Creation Date</label>
        <input
          className="upload-demo-input"
          type="date"
          id={`creation-date-${index}`}
          placeholder="Creation Date"
          value={cases[index]?.creationDate}
          onChange={(e) => handleCaseChange(index, e)}
        />
        <div>
          <input
            className="upload-demo-subject-description"
            placeholder="Subject"
            value={cases[index]?.subject}
            onChange={(e) => handleCaseChange(index, e)}
          />
        </div>
        <div>
          <textarea
            className="upload-demo-subject-description"
            placeholder="Description"
            value={cases[index]?.description}
            onChange={(e) => handleCaseChange(index, e)}
          ></textarea>
        </div>
        <button className="demo-btn remove-btn" onClick={(e) => handleRemoveTab('case', index)}>
          Remove Case
        </button>
      </div>
    </div>
  );

  const renderInteractionDetails = (index) => (
    <div className="interaction-details">
      <div>
        <input
          className="upload-demo-input"
          placeholder="Title"
          value={interactions[index]?.title}
          onChange={(e) => handleInteractionChange(index, e)}
        />
        <input
          className="upload-demo-input"
          type="date"
          placeholder="Date"
          value={interactions[index]?.date}
          onChange={(e) => handleInteractionChange(index, e)}
        />
        <input
          className="upload-demo-input"
          type="time"
          placeholder="Time"
          value={interactions[index]?.time}
          onChange={(e) => handleInteractionChange(index, e)}
        />
        <textarea
          className="upload-demo-subject-description"
          placeholder="Description"
          value={interactions[index]?.description}
          onChange={(e) => handleInteractionChange(index, e)}
        />
      </div>
      <button className="demo-btn remove-btn" onClick={(e) => handleRemoveTab('interaction', index)}>
        Remove Interaction
      </button>
    </div>
  );

  return (
    <div>
      <div className="tabs">
        <button className="tab-button" onClick={() => setSelectedTab('Contact Card')}>Contact Card</button>

        {caseTabs.map((tabName, index) => (
          <div className="tab-item" key={index}>
            <button className={`tab-button ${selectedTab === tabName ? 'active' : ''}`} onClick={() => setSelectedTab(tabName)}>
              {tabName}
              <IoCloseSharp
                className="close-tab-icon"
                onClick={(e) => {
                  e.stopPropagation();
                  handleRemoveTab('case', index);
                }}
              />
            </button>
          </div>
        ))}
        {interactionTabs.map((tabName, index) => (
          <div className="tab-item" key={index}>
            <button className={`tab-button ${selectedTab === tabName ? 'active' : ''}`} onClick={() => setSelectedTab(tabName)}>
              {tabName}
              <IoCloseSharp
                className="close-tab-icon"
                onClick={(e) => {
                  e.stopPropagation();
                  handleRemoveTab('interaction', index);
                }}
              />
            </button>
          </div>
        ))}
      </div>

      <div className="content">
        {selectedTab === 'Contact Card' && <Customer360 metadata={metadata} handleInput={handleInput} />}
        {caseTabs.includes(selectedTab) && renderCaseDetails(caseTabs.indexOf(selectedTab))}
        {interactionTabs.includes(selectedTab) && renderInteractionDetails(interactionTabs.indexOf(selectedTab))}
      </div>
    </div>
  );
};

export default CallSummaryDetailsForm;
