export default function AgentDetails({ metadata, handleInput }) {
    return (
        <div style={{display:"flex" , flexDirection:"column"}}>
            <div className="details">
            <label htmlFor='agent'>Agent Name</label>
               <input
                id='agent'
                className="upload-demo-input"
                placeholder='Agent Name'
                value={metadata?.agent}
                onChange={e => handleInput('agent', e.target.value)}
            />
            </div>
            <div className="details"> 
            <label htmlFor='UseCase'>Use Case</label>             
            <input
                id='UseCase'
                className="upload-demo-input"
                placeholder='Use Case'
                value={metadata?.agent}
                onChange={e => handleInput('agent', e.target.value)}
            />
            </div>
            <div className="details"> 
            <label htmlFor='AHT'>AHT</label>             
            <input
                id='AHT'
                className="upload-demo-input"
                placeholder='AHT'
                value={metadata?.agent}
                onChange={e => handleInput('agent', e.target.value)}
            />
            </div>
            <div className="details"> 
            <label htmlFor='industry'>Industry</label>
                <select
                    id='industry'
                    value={metadata?.industry}
                    className="upload-demo-input"
                    onChange={e => handleInput('industry', e.target.value)}
                >
                    <option value='Insurance'>Insurance</option>
                    <option value='Healthcare'>Healthcare</option>
                    <option value='Finance'>Finance</option>
                    <option value='E-commerce'>E-commerce</option>
                    <option value='Utilities'>Utilites</option>
                </select>
            </div>
            <div className="details"> 
            <label htmlFor='header'>Heading</label>
                <select
                    id='header'
                    className="upload-demo-input"
                    value={metadata?.header}
                    onChange={e => handleInput('header', e.target.value)}
                >
                    <option value="Agent Assist">Agent Assist</option>
                    <option value="Dynamic 365">Dynamic 365</option>
                </select>
            </div>
            <div className="details"> 
            <label htmlFor='interaction-date'>Interaction Date</label>
                <input
                    id='interaction-date'
                    className="upload-demo-input"
                    type='date'
                    value={metadata?.interactionDate}
                    onChange={e => handleInput('interactionDate', e.target.value)}
                />
            </div>
            </div>
    )
}
