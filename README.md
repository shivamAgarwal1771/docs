import React, { useEffect, useState } from 'react';
import Customer360 from './Customer360';
import { useDispatch } from "react-redux";
import { IoCloseSharp } from "react-icons/io5";
import { CUSTOMERDETAILS, INPUT_TYPES, ADD_BUTTON } from "../../../utility/constants"
import { setProgressBarResponse } from "../../../store/callSlice";

const CallSummaryDetailsForm = ({ metadata, handleInput }) => {
  const dispatch = useDispatch()
  const [contactFields, setContactFields] = useState([]);
  const [cases, setCases] = useState([]);
  const [interactions, setInteractions] = useState([]);
  const [selectedTab, setSelectedTab] = useState('Contact Card');
  const [activeCaseTab, setActiveCaseTab] = useState(null);
  const [activeInteractionTab, setActiveInteractionTab] = useState(null);

  useEffect(() => {
    dispatch(setProgressBarResponse(true))
  }, [])

  useEffect(() => {
    if (!contactFields?.length && metadata?.personalInformation) {
      setContactFields(metadata?.personalInformation);
    }
  }, [metadata?.personalInformation, contactFields]);

  useEffect(() => {
    if (!cases?.length && metadata?.cases) {
      setCases(metadata?.cases);
    }
  }, [metadata?.cases, cases]);

  useEffect(() => {
    if (!interactions?.length && metadata?.interactionHistory) {
      setInteractions(metadata?.interactionHistory);
    }
  }, [metadata?.interactionHistory, interactions]);

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
    const updatedFields = contactFields?.filter((_, i) => i !== index);
    setContactFields(updatedFields);
  };

  const handleFieldChange = (index, e) => {
    const { name, value } = e.target;
    const updatedFields = contactFields?.map((field, i) =>
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
    const updatedCases = cases?.map((caseItem, i) =>
      i === index ? { ...caseItem, [name]: value } : caseItem
    );
    setCases(updatedCases);
  };

  const handleRemoveCase = (index) => {
    const updatedCases = cases?.filter((_, i) => i !== index);
    setCases(updatedCases);
  };

  const handleAddInteraction = () => {
    setInteractions([...interactions, { title: '', date: '', time: '', description: '' }]);
  };

  const handleInteractionChange = (index, e) => {
    const { name, value } = e.target;
    const updatedInteractions = interactions?.map((interaction, i) =>
      i === index ? { ...interaction, [name]: value } : interaction
    );
    setInteractions(updatedInteractions);
  };

  const handleRemoveInteraction = (index) => {
    const updatedInteractions = interactions?.filter((_, i) => i !== index);
    setInteractions(updatedInteractions);
  };

  const handleCaseTabClick = (index) => {
    setActiveCaseTab(index);
  };

  const handleInteractionTabClick = (index) => {
    setActiveInteractionTab(index);
  };

  return (
    <div>
      <div className="CallSummary-tabs">
        <button
          className={`CallSummary-tab-button ${selectedTab === CUSTOMERDETAILS.CONTACT ? 'active-tab' : ''}`}
          onClick={() => setSelectedTab(CUSTOMERDETAILS.CONTACT)}
        >
          {CUSTOMERDETAILS.CONTACT}
        </button>
        <button
          className={`CallSummary-tab-button ${selectedTab === CUSTOMERDETAILS.CASES ? 'active-tab' : ''}`}
          onClick={() => setSelectedTab(CUSTOMERDETAILS.CASES)}
        >
          {CUSTOMERDETAILS.CASES}
        </button>
        <button
          className={`CallSummary-tab-button ${selectedTab === CUSTOMERDETAILS.INTERACTION_HISTORY ? 'active-tab' : ''}`}
          onClick={() => setSelectedTab(CUSTOMERDETAILS.INTERACTION_HISTORY)}
        >
          {CUSTOMERDETAILS.INTERACTION_HISTORY}
        </button>
        <button
          className={`CallSummary-tab-button ${selectedTab === CUSTOMERDETAILS.CUSTOMER_360 ? 'active-tab' : ''}`}
          onClick={() => setSelectedTab(CUSTOMERDETAILS.CUSTOMER_360)}
        >
          {CUSTOMERDETAILS.CUSTOMER_360}
        </button>
      </div>


      {selectedTab === CUSTOMERDETAILS.CONTACT && (
        <div className="contact-card">
          <div className="contact-card-header">
            <h2><b>{CUSTOMERDETAILS.CONTACT}</b></h2>
            <button className="add-fields-btn" onClick={handleAddContactField}>{ADD_BUTTON.ADD}</button>
          </div>
          <div className="overflow-y-scroll">
            {contactFields?.map((field, index) => (
              <div key={index} className="field-row margin-tb-20">
                <input
                  className="CallSummary-input-field left-curved-input call-info-input-width"
                  type={INPUT_TYPES.TEXT}
                  placeholder="Field"
                  name="key"
                  value={field?.key}
                  onChange={(e) => handleFieldChange(index, e)}
                />
                <input
                  className="CallSummary-input-field call-info-input-width"
                  type={INPUT_TYPES.TEXT}
                  placeholder="Value"
                  name="value"
                  value={field?.value}
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

      {selectedTab === CUSTOMERDETAILS.CASES && (
        <div className="CallSummary-cases">
          <div className="contact-card-header">
            <h2><b>{CUSTOMERDETAILS.CASES}</b></h2>
            <button className="add-fields-btn" onClick={handleAddCase}>{ADD_BUTTON.ADD}</button>
          </div>
          <div className="tabs">
            {cases?.map((_, index) => (
              <button
                key={index}
                className={`tab-button ${activeCaseTab === index ? 'active' : ''}`}
                onClick={() => handleCaseTabClick(index)}
              >
                Case {index + 1}
              </button>
            ))}
          </div>

          <div className="overflow-y-scroll">
            {cases?.map((caseItem, index) => (
              <div
                key={index}
                className="case-tab-content"
              >
                {activeCaseTab === index && (
                  <div className="margin-tb-10">
                    <button className="resolution-remove-btn" onClick={() => handleRemoveCase(index)}><IoCloseSharp className="close-btn-icon" /></button>
                    <div key={index} className="field-column border-box padding-10 rounded-border gap-10">
                      <div className="field-row gap-10">
                        <input
                          className="CallSummary-input-field call-info-input-width rounded-border"
                          type={INPUT_TYPES.TEXT}
                          placeholder="CaseNumber"
                          name="caseNumber"
                          value={caseItem.caseNumber}
                          onChange={(e) => handleCaseChange(index, e)}
                        />
                        <input
                          className="CallSummary-input-field call-info-input-width rounded-border"
                          type={INPUT_TYPES.DATE}
                          placeholder="Creation Date"
                          name="creationDate"
                          value={caseItem.creationDate}
                          onChange={(e) => handleCaseChange(index, e)}
                        />
                      </div>
                      <div className="field-row gap-10">
                        <input
                          className="CallSummary-input-field call-info-input-width rounded-border"
                          type={INPUT_TYPES.TEXT}
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
                          <option value="High">High</option>
                          <option value="Medium">Medium</option>
                          <option value="Low">Low</option>
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
                        type={INPUT_TYPES.TEXT}
                        placeholder="quickAction"
                        name="quickAction"
                        value={caseItem.quickAction}
                        onChange={(e) => handleCaseChange(index, e)}
                      />
                    </div>
                  </div>
                )}
              </div>
            ))}
          </div>
        </div>
      )}

      {selectedTab === CUSTOMERDETAILS.INTERACTION_HISTORY && (
        <div className="CallSummary-interaction-history">
          <div className="contact-card-header">
            <h2><b>{CUSTOMERDETAILS.INTERACTION_HISTORY}</b></h2>
            <button className="add-fields-btn" onClick={handleAddInteraction}>+ Add</button>
          </div>
          <div className="tabs">
            {interactions?.map((_, index) => (
              <button
                key={index}
                className={`tab-button ${activeInteractionTab === index ? 'active' : ''}`}
                onClick={() => handleInteractionTabClick(index)}
              >
                Interaction {index + 1}
              </button>
            ))}
          </div>

          <div className="overflow-y-scroll">
            {interactions?.map((interaction, index) => (
              <div
                key={index}
                className="interaction-tab-content"
              // className={`interaction-tab-content ${activeInteractionTab === index ? 'active' : ''}`}
              >
                {activeInteractionTab === index && (
                  <div className="margin-tb-10">
                    <button className="resolution-remove-btn" onClick={() => handleRemoveInteraction(index)}><IoCloseSharp className="close-btn-icon" /></button>
                    <div key={index} className="field-column border-box padding-10 rounded-border gap-10">
                      <div className="field-row gap-10">
                        <input
                          className="CallSummary-input-field call-info-input-width rounded-border"
                          type={INPUT_TYPES.TEXT}
                          placeholder="Title"
                          name="title"
                          value={interaction.title}
                          onChange={(e) => handleInteractionChange(index, e)}
                        />
                        {/* <input
                          className="CallSummary-input-field call-info-input-width rounded-border"
                          type={INPUT_TYPES.DATE}
                          placeholder="Date"
                          name="date"
                          value={interaction.date}
                          onChange={(e) => handleInteractionChange(index, e)}
                        /> */}
                        <input
                          className="CallSummary-input-field call-info-input-width rounded-border"
                          type="datetime-local"
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
                  </div>
                )}
              </div>
            ))}
          </div>
        </div>
      )}

      {selectedTab === CUSTOMERDETAILS.CUSTOMER_360 && (
        <Customer360 metadata={metadata} handleInput={handleInput} />
      )}
    </div>
  );
};

export default CallSummaryDetailsForm;
