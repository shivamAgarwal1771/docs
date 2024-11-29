import { useEffect } from "react";
import { AGENTDEATAILS, SIMULATION_HEADER, LANGUAGES, INDUSTRY_DETAILS } from "../../../utility/constants";

export default function AgentDetails({ metadata, handleInput }) {
    // Function to generate a random 5-digit code
    const generateRandomCode = () => {
        return Math.floor(10000 + Math.random() * 90000); // Generates a 5-digit number
    };

    // Effect to set initial values when the component mounts
    useEffect(() => {
        // Check if 'code' is not set yet and assign a random 5-digit code
        if (!metadata?.code) {
            const randomCode = generateRandomCode().toString();
            handleInput('code', randomCode); // Update the 'code' field
        }

        // Check if 'channel' is not set yet and assign 'Voice'
        if (!metadata?.channel) {
            handleInput('channel', 'Voice'); // Set default channel to 'Voice'
        }
    }, [metadata, handleInput]); // Only run when metadata or handleInput changes

    return (
        <div className="agent-details-container align-column">
            <div className="align-row">
                <div className="align-column">
                    <div className="agent-details-field">
                        <label className="agent-details-label">{AGENTDEATAILS.AGENT_NAME}</label>
                        <input
                            id="agent"
                            className="agent-details-input"
                            placeholder="Agent Name"
                            value={metadata?.agent || ''}
                            onChange={e => handleInput('agent', e.target.value)}
                        />
                    </div>
                    <div className="agent-details-field">
                        <label className="agent-details-label">{AGENTDEATAILS.USE_CASE}</label>
                        <input
                            id="useCase"
                            className="agent-details-input"
                            placeholder="Use Case"
                            value={metadata?.useCase || ''}
                            onChange={e => handleInput('useCase', e.target.value)}
                        />
                    </div>
                    <div className="agent-details-field">
                        <label className="agent-details-label">{AGENTDEATAILS.AHT}</label>
                        <input
                            id="aht"
                            className="agent-details-input"
                            placeholder="AHT"
                            value={metadata?.aht || ''}
                            onChange={e => handleInput('aht', e.target.value)}
                        />
                    </div>
                    <div className="agent-details-field">
                        <label className="agent-details-label">{AGENTDEATAILS.INDUSTRY}</label>
                        <select
                            id="industry"
                            className="agent-details-input"
                            value={metadata?.industry || ''}
                            onChange={e => handleInput('industry', e.target.value)}
                        >
                            <option value="Insurance">{INDUSTRY_DETAILS.INSURANCE}</option>
                            <option value="Healthcare">{INDUSTRY_DETAILS.HEALTHCARE}</option>
                            <option value="Finance">{INDUSTRY_DETAILS.FINANCE}</option>
                            <option value="E-commerce">{INDUSTRY_DETAILS.COMMERCE}</option>
                            <option value="Utilities">{INDUSTRY_DETAILS.UTILITIES}</option>
                        </select>
                    </div>
                </div>
                <div className="align-column">
                    <div className="agent-details-field">
                        <label className="agent-details-label">{AGENTDEATAILS.HEADING}</label>
                        <select
                            id="header"
                            className="agent-details-input"
                            value={metadata?.header || ''}
                            onChange={e => handleInput('header', e.target.value)}
                        >
                            <option value="Agent Assist">{SIMULATION_HEADER.AGENT_ASSIST}</option>
                            <option value="Dynamic 365">{SIMULATION_HEADER.DYNAMIC_365}</option>
                        </select>
                    </div>
                    <div className="agent-details-field">
                        <label className="agent-details-label">{AGENTDEATAILS.INTERACTION_DATE}</label>
                        <input
                            id="interaction-date"
                            className="agent-details-input"
                            type="date"
                            value={metadata?.interactionDate || ''}
                            onChange={e => handleInput('interactionDate', e.target.value)}
                        />
                    </div>
                    <div className="agent-details-field">
                        <label className="agent-details-label">Code</label>
                        <input
                            id="code"
                            className="agent-details-input"
                            type="text"
                            value={metadata?.code || ''}
                            onChange={e => handleInput('code', e.target.value)}
                        />
                    </div>
                    <div className="agent-details-field">
                        <label className="agent-details-label">{AGENTDEATAILS.LANGUAGE}</label>
                        <select
                            id="selectedLanguage"
                            className="agent-details-input"
                            value={metadata?.selectedLanguage || ''}
                            onChange={e => handleInput('selectedLanguage', e.target.value)}
                        >
                            <option value="Hindi">{LANGUAGES.HINDI.name}</option>
                            <option value="English">{LANGUAGES.ENGLISH.name}</option>
                            <option value="Mandarin">{LANGUAGES.MANDARIN.name}</option>
                            <option value="Spanish">{LANGUAGES.SPANISH.name}</option>
                        </select>
                    </div>
                </div>
            </div>
            <div className="agent-details-field">
                <label className="agent-details-label">Channel</label>
                <select
                    id="channel"
                    className="agent-details-input"
                    value={metadata?.channel || ''}
                    onChange={e => handleInput('channel', e.target.value)}
                >
                    <option value="Voice">Voice</option>
                    <option value="Mail">Mail</option>
                </select>
            </div>
        </div>
    );
}
