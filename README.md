import { useState, useEffect } from "react";

export default function AgentDetails({ metadata, handleInput }) {
  const [localMetadata, setLocalMetadata] = useState(metadata);

  // Generate a random 5-digit code
  const generateRandomCode = () => {
    return Math.floor(10000 + Math.random() * 90000); // Random 5-digit number
  };

  // Update the local state when any input field changes
  const handleFieldChange = (field, value) => {
    setLocalMetadata(prevState => {
      const updatedMetadata = { ...prevState, [field]: value };

      // If the field is not 'code', always generate a random code and set it
      if (field !== "code") {
        updatedMetadata.code = updatedMetadata.code || generateRandomCode(); // Only generate if it's not set
      }

      // Always update 'channel' to "Voice"
      updatedMetadata.channel = "Voice";

      // Call the parent handler to update metadata
      handleInput(updatedMetadata);

      return updatedMetadata;
    });
  };

  useEffect(() => {
    // Ensure 'code' is always set when metadata is first received as a prop
    if (!metadata.code) {
      metadata.code = generateRandomCode(); // Assign random code if not already present
    }
    setLocalMetadata(metadata);
  }, [metadata]);

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
              value={localMetadata?.agent || ''}
              onChange={e => handleFieldChange("agent", e.target.value)}
            />
          </div>
          <div className="agent-details-field">
            <label className="agent-details-label">Use Case</label>
            <input
              id="useCase"
              className="agent-details-input"
              placeholder="Use Case"
              value={localMetadata?.useCase || ''}
              onChange={e => handleFieldChange("useCase", e.target.value)}
            />
          </div>
          <div className="agent-details-field">
            <label className="agent-details-label">AHT</label>
            <input
              id="aht"
              className="agent-details-input"
              placeholder="AHT"
              value={localMetadata?.aht || ''}
              onChange={e => handleFieldChange("aht", e.target.value)}
            />
          </div>
          <div className="agent-details-field">
            <label className="agent-details-label">Industry</label>
            <select
              id="industry"
              className="agent-details-input"
              value={localMetadata?.industry || ''}
              onChange={e => handleFieldChange("industry", e.target.value)}
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
              value={localMetadata?.header || ''}
              onChange={e => handleFieldChange("header", e.target.value)}
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
              value={localMetadata?.interactionDate || ''}
              onChange={e => handleFieldChange("interactionDate", e.target.value)}
            />
          </div>
          {/* Hidden Code Field */}
          <div className="agent-details-field" style={{ display: "none" }}>
            <label className="agent-details-label">Code</label>
            <input
              id="code"
              className="agent-details-input"
              type="text"
              value={localMetadata?.code || ''}
              readOnly
            />
          </div>
          <div className="agent-details-field">
            <label className="agent-details-label">Language</label>
            <select
              id="selectedLanguage"
              className="agent-details-input"
              value={localMetadata?.selectedLanguage || ''}
              onChange={e => handleFieldChange("selectedLanguage", e.target.value)}
            >
              <option value="Hindi">Hindi</option>
              <option value="English">English</option>
              <option value="Mandarin">Mandarin</option>
              <option value="Spanish">Spanish</option>
            </select>
          </div>
        </div>
      </div>

      {/* Hidden Channel Field */}
      <div className="agent-details-field" style={{ display: "none" }}>
        <label className="agent-details-label">Channel</label>
        <select
          id="channel"
          className="agent-details-input"
          value={localMetadata?.channel || ''}
          onChange={e => handleFieldChange("channel", e.target.value)}
        >
          <option value="Voice">Voice</option>
          <option value="Mail">Mail</option>
        </select>
      </div>
    </div>
  );
}
