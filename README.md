import React, { useState } from "react";

const CallSummaryDetailsForm = ({ onFieldChange }) => {
  const [contactFields, setContactFields] = useState([]);
  const [cases, setCases] = useState([]);
  const [interactions, setInteractions] = useState([]);
  const [selectedTab, setSelectedTab] = useState("Contact Card");

  const handleAddContactField = () => {
    setContactFields([...contactFields, { field: "", value: "" }]);
  };

  const handleRemoveContactField = (index) => {
    const updatedFields = contactFields.filter((_, i) => i !== index);
    setContactFields(updatedFields);
  };

  const handleAddCase = () => {
    setCases([
      ...cases,
      {
        caseId: "",
        creationDate: "",
        subject: "",
        priority: "",
        description: "",
      },
    ]);
  };

  const handleRemoveCase = (index) => {
    const updatedCases = cases.filter((_, i) => i !== index);
    setCases(updatedCases);
  };

  const handleAddInteraction = () => {
    setInteractions([
      ...interactions,
      { title: "", date: "", time: "", description: "" },
    ]);
  };

  const handleRemoveInteraction = (index) => {
    const updatedInteractions = interactions.filter((_, i) => i !== index);
    setInteractions(updatedInteractions);
  };

  const handleFieldChange = (tab, fieldName, value) => {
    onFieldChange(tab, fieldName, value);
  };

  return (
    <div>
      <div className="CallSummary-tabs">
        <button
          className="CallSummary-tab-button"
          onClick={() => setSelectedTab("Contact Card")}
        >
          Contact Card
        </button>
        <button
          className="CallSummary-tab-button"
          onClick={() => setSelectedTab("Cases")}
        >
          Cases
        </button>
        <button
          className="CallSummary-tab-button"
          onClick={() => setSelectedTab("Interaction History")}
        >
          Interaction History
        </button>
      </div>

      {selectedTab === "Contact Card" && (
        <div className="contact-card">
          <h2>Contact Card</h2>
          <button className="CallSummary-btn" onClick={handleAddContactField}>
            Add Customer Field
          </button>
          {contactFields.map((field, index) => (
            <div key={index} className="field-row">
              <input
                className="CallSummary-input-field"
                type="text"
                placeholder="Field"
                value={field.field}
                onChange={(e) =>
                  handleFieldChange("Contact Card", `field-${index}`, e.target.value)
                }
              />
              <input
                className="CallSummary-input-field"
                type="text"
                placeholder="Value"
                value={field.value}
                onChange={(e) =>
                  handleFieldChange("Contact Card", `value-${index}`, e.target.value)
                }
              />
              <button
                className="CallSummary-remove-button"
                onClick={() => handleRemoveContactField(index)}
              >
                Remove
              </button>
            </div>
          ))}
        </div>
      )}

      {selectedTab === "Cases" && (
        <div className="CallSummary-cases">
          <h2>Cases</h2>
          <button className="CallSummary-btn" onClick={handleAddCase}>
            Add Case
          </button>
          {cases.map((caseItem, index) => (
            <div key={index} className="case-section">
              <input
                className="CallSummary-input-field"
                type="text"
                placeholder="Case ID"
                value={caseItem.caseId}
                onChange={(e) =>
                  handleFieldChange("Cases", `caseId-${index}`, e.target.value)
                }
              />
              <input
                className="CallSummary-input-field"
                type="date"
                placeholder="Creation Date"
                value={caseItem.creationDate}
                onChange={(e) =>
                  handleFieldChange("Cases", `creationDate-${index}`, e.target.value)
                }
              />
              <input
                className="CallSummary-input-field"
                type="text"
                placeholder="Subject"
                value={caseItem.subject}
                onChange={(e) =>
                  handleFieldChange("Cases", `subject-${index}`, e.target.value)
                }
              />
              <textarea
                className="CallSummary-input-field"
                placeholder="Description"
                value={caseItem.description}
                onChange={(e) =>
                  handleFieldChange("Cases", `description-${index}`, e.target.value)
                }
              />
              <button
                className="CallSummary-remove-button"
                onClick={() => handleRemoveCase(index)}
              >
                Remove Case
              </button>
            </div>
          ))}
        </div>
      )}

      {selectedTab === "Interaction History" && (
        <div className="CallSummary-interaction-history">
          <h2>Interaction History</h2>
          <button className="CallSummary-btn" onClick={handleAddInteraction}>
            Add Interaction
          </button>
          {interactions.map((interaction, index) => (
            <div key={index} className="CallSummary-interaction-section">
              <input
                className="CallSummary-input-field"
                type="text"
                placeholder="Title"
                value={interaction.title}
                onChange={(e) =>
                  handleFieldChange("Interaction History", `title-${index}`, e.target.value)
                }
              />
              <input
                className="CallSummary-input-field"
                type="date"
                placeholder="Date"
                value={interaction.date}
                onChange={(e) =>
                  handleFieldChange("Interaction History", `date-${index}`, e.target.value)
                }
              />
              <textarea
                className="CallSummary-input-field"
                placeholder="Description"
                value={interaction.description}
                onChange={(e) =>
                  handleFieldChange(
                    "Interaction History",
                    `description-${index}`,
                    e.target.value
                  )
                }
              />
              <button
                className="CallSummary-remove-button"
                onClick={() => handleRemoveInteraction(index)}
              >
                Remove Interaction
              </button>
            </div>
          ))}
        </div>
      )}
    </div>
  );
};

export default CallSummaryDetailsForm;
