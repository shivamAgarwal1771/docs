export default function AgentDetails({ metadata, handleInput }) {
    return (
        <div className="agent-details-container">
            <div className="agent-details-field">
                <label htmlFor="agent" className="agent-details-label">Agent Name</label>
                <input
                    id="agent"
                    className="agent-details-input"
                    placeholder="Agent Name"
                    value={metadata?.agent}
                    onChange={e => handleInput('agent', e.target.value)}
                />
            </div>
            <div className="agent-details-field">
                <label htmlFor="useCase" className="agent-details-label">Use Case</label>
                <input
                    id="useCase"
                    className="agent-details-input"
                    placeholder="Use Case"
                    value={metadata?.useCase}
                    onChange={e => handleInput('useCase', e.target.value)}
                />
            </div>
            <div className="agent-details-field">
                <label htmlFor="AHT" className="agent-details-label">AHT</label>
                <input
                    id="AHT"
                    className="agent-details-input"
                    placeholder="AHT"
                    value={metadata?.AHT}
                    onChange={e => handleInput('AHT', e.target.value)}
                />
            </div>
            <div className="agent-details-field">
                <label htmlFor="industry" className="agent-details-label">Industry</label>
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
            <div className="agent-details-field">
                <label htmlFor="header" className="agent-details-label">Heading</label>
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
                <label htmlFor="interaction-date" className="agent-details-label">Interaction Date</label>
                <input
                    id="interaction-date"
                    className="agent-details-input"
                    type="date"
                    value={metadata?.interactionDate}
                    onChange={e => handleInput('interactionDate', e.target.value)}
                />
            </div>
        </div>
    );
}


.agent-details-container {
    display: flex;
    flex-direction: column;
    gap: 1rem;
    max-width: 400px;
    margin: 0 auto;
    padding: 1.5rem;
    border: 1px solid #ddd;
    border-radius: 8px;
    background-color: #f9f9f9;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.agent-details-field {
    display: flex;
    flex-direction: column;
}

.agent-details-label {
    font-size: 0.9rem;
    font-weight: bold;
    margin-bottom: 0.3rem;
    color: #333;
}

.agent-details-input {
    padding: 0.5rem;
    font-size: 0.9rem;
    border: 1px solid #ccc;
    border-radius: 4px;
    background-color: #fff;
    outline: none;
    transition: border-color 0.3s ease;
}

.agent-details-input:focus {
    border-color: #007bff;
    box-shadow: 0 0 4px rgba(0, 123, 255, 0.2);
}

.agent-details-container select.agent-details-input {
    appearance: none;
    background-image: url("data:image/svg+xml,%3Csvg xmlns='http://www.w3.org/2000/svg' viewBox='0 0 4 5'%3E%3Cpath fill='%23000' d='M2 0L0 2h4z'/%3E%3C/svg%3E");
    background-repeat: no-repeat;
    background-position: right 0.5rem center;
    background-size: 0.65rem;
}

.agent-details-container input[type="date"].agent-details-input {
    padding-right: 2rem; /* Ensures text doesn’t overlap with the calendar icon */
}
