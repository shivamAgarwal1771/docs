import { useEffect } from "react";

export default function AgentDetails({ metadata, handleInput }) {
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
                            value={metadata?.agent}
                            onChange={e => handleInput('agent', e.target.value)}
                        />
                    </div>
                    <div className="agent-details-field">
                        <label className="agent-details-label">Use Case</label>
                        <input
                            id="useCase"
                            className="agent-details-input"
                            placeholder="Use Case"
                            value={metadata?.useCase}
                            onChange={e => handleInput('useCase', e.target.value)}
                        />
                    </div>
                    <div className="agent-details-field">
                        <label className="agent-details-label">AHT</label>
                        <input
                            id="aht"
                            className="agent-details-input"
                            placeholder="AHT"
                            value={metadata?.aht}
                            onChange={e => handleInput('aht', e.target.value)}
                        />
                    </div>
                    <div className="agent-details-field">
                        <label className="agent-details-label">Industry</label>
                        <select
                            id="industry"
                            className="agent-details-input"
                            value={metadata?.industry}
                            onChange={e => handleInput('industry', e.target.value)}
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
                            value={metadata?.header}
                            onChange={e => handleInput('header', e.target.value)}
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
                            value={metadata?.interactionDate}
                            onChange={e => handleInput('interactionDate', e.target.value)}
                        />
                    </div>
                    <div className="agent-details-field">
                        <label className="agent-details-label">code</label>
                        <input
                            id="code"
                            className="agent-details-input"
                            type="text"
                            value={metadata?.code}
                            onChange={e => handleInput('code', e.target.value)}
                        />
                    </div>
                    <div className="agent-details-field">
                        <label className="agent-details-label">Language</label>
                        <select
                            id="selectedLanguage"
                            className="agent-details-input"
                            value={metadata?.selectedLanguage}
                            onChange={e => handleInput('selectedLanguage', e.target.value)}
                        >
                            <option value="Hindi">Hindi</option>
                            <option value="English">English</option>
                        </select>
                    </div>
                </div>
            </div>
            <div className="agent-details-field">
                <label className="agent-details-label">Channel</label>
                <select
                    id="channel"
                    className="agent-details-input"
                    value={metadata?.channel}
                    onChange={e => handleInput('channel', e.target.value)}
                >
                    <option value="Voice">Voice</option>
                    <option value="Mail">Mail</option>
                </select>
            </div>
        </div>

    );
}

