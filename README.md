import { useState, useEffect } from "react";

// Function to generate random 5-digit code
const generateRandomCode = () => Math.floor(10000 + Math.random() * 90000);

export default function AgentDetails({ metadata, handleInput }) {
  const [localMetadata, setLocalMetadata] = useState(metadata);

  // Effect to ensure code and channel are initialized when the component loads
  useEffect(() => {
    if (!localMetadata.code) {
      setLocalMetadata((prev) => ({ ...prev, code: generateRandomCode() }));
    }

    if (!localMetadata.channel) {
      setLocalMetadata((prev) => ({ ...prev, channel: "Voice" }));
    }
  }, [metadata]); // This only runs on the initial render or when 'metadata' changes

  // Handle input change for any field except 'code' and 'channel'
  const handleFieldChange = (field, value) => {
    setLocalMetadata((prevState) => {
      const updatedMetadata = { ...prevState, [field]: value };

      // If the code is not already set, generate a random code
      if (!updatedMetadata.code) {
        updatedMetadata.code = generateRandomCode();
      }

      // Ensure 'channel' is always set to 'Voice'
      updatedMetadata.channel = "Voice";

      // Call handleInput function to pass the updated data to the parent component
      handleInput(updatedMetadata);

      return updatedMetadata;
    });
  };

  return (
    <div className="agent-details-container align-column">
      <div className="align-row">
        <div className="align-column">
          <div className="agent-details-field">
            <label className="agent-details-label">Agent Name</label>
            <input
              id="agent"
              className="agent-details-input"
              placeholder="Agent Name"
              value={localMetadata?.agent || ""}
              onChange={(e) => handleFieldChange("agent", e.target.value)}
            />
          </div>
          <div className="agent-details-field">
            <label className="agent-details-label">Use Case</label>
            <input
              id="useCase"
              className="agent-details-input"
              placeholder="Use Case"
              value={localMetadata?.useCase || ""}
              onChange={(e) => handleFieldChange("useCase", e.target.value)}
            />
          </div>
          <div className="agent-details-field">
            <label className="agent-details-label">AHT</label>
            <input
              id="aht"
              className="agent-details-input"
              placeholder="AHT"
              value={localMetadata?.aht || ""}
              onChange={(e) => handleFieldChange("aht", e.target.value)}
            />
          </div>
          <div className="agent-details-field">
            <label className="agent-details-label">Industry</label>
            <select
              id="industry"
              className="agent-details-input"
              value={localMetadata?.industry || ""}
              onChange={(e) => handleFieldChange("industry", e.target.value)}
            >
              <option value="Insurance">Insurance</option>
              <option value="Healthcare">Healthcare</option>
              <option value="Finance">Finance</option>
              <option value="E-commerce">E-commerce</option>
              <option value="Utilities">Utilities</option>
            </select>
          </div>
        </div>
        <div className="align-column">
          <div className="agent-details-field">
            <label className="agent-details-label">Heading</label>
            <select
              id="header"
              className="agent-details-input"
              value={localMetadata?.header || ""}
              onChange={(e) => handleFieldChange("header", e.target.value)}
            >
              <option value="Agent Assist">Agent Assist</option>
              <option value="Dynamic 365">Dynamic 365</option>
            </select>
          </div>
          <div className="agent-details-field">
            <label className="agent-details-label">Interaction Date</label>
            <input
              id="interaction-date"
              className="agent-details-input"
              type="date"
              value={localMetadata?.interactionDate || ""}
              onChange={(e) => handleFieldChange("interactionDate", e.target.value)}
            />
          </div>
          <div className="agent-details-field">
            <label className="agent-details-label">Language</label>
            <select
              id="selectedLanguage"
              className="agent-details-input"
              value={localMetadata?.selectedLanguage || ""}
              onChange={(e) => handleFieldChange("selectedLanguage", e.target.value)}
            >
              <option value="Hindi">Hindi</option>
              <option value="English">English</option>
              <option value="Mandarin">Mandarin</option>
              <option value="Spanish">Spanish</option>
            </select>
          </div>
        </div>
      </div>

      {/* Hidden field for channel, always set to 'Voice' */}
      <input
        type="hidden"
        id="channel"
        value={localMetadata?.channel || "Voice"}
        onChange={() => {}}
      />

      {/* Hidden field for the randomly generated code */}
      <input
        type="hidden"
        id="code"
        value={localMetadata?.code || generateRandomCode()}
        onChange={() => {}}
      />
    </div>
  );
}
