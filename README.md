import React, { useState } from 'react';
import AgentDetails from '../../components/demoCreationComponents/upload-demo/AgentDetails';
import MediaSelection from '../../components/demoCreationComponents/upload-demo/MediaSelection';
import Customize from "../../components/demoCreationComponents/customise/customisejson"
import CallSummaryDetails from "../../components/demoCreationComponents/upload-demo/CallSummaryDetails"
import CallSummaryTabs from "../../components/demoCreationComponents/upload-demo/callSummaryinfo"
import CustomizeWiki from "../../components/demoCreationComponents/customise/Aiwiki"
import CustomizeAutoAudit from "../../components/demoCreationComponents/customise/AutoAuditjson"
import CustomiseActionWorkflow from "../../components/demoCreationComponents/customise/ActionWorkflowjson"

const DemoDetails = () => <div>Demo Details Content</div>;
const DemoResources = () => <div>Demo Resources Content</div>;
const NudgesUtterances = () => <div>Nudges & Utterances Content</div>;
const AgentWiki = () => <div>Agent Wiki Content</div>;
const AutoAudit = () => <div>Auto-Audit Content</div>;
const ActionWorkflow = () => <div>Action Workflow Content</div>;
const CustomerInfo = () => <div>Customer Info Content</div>;
const CallSummary = () => <div>Call Summary Content</div>;
 
const steps = [
  { number: 1, label: "Demo Details", component: <AgentDetails handleInput={()=>{}} metadata={"text"} /> },
  { number: 2, label: "Demo Resources", component: <MediaSelection /> },
  { number: 3, label: "Nudges & Utterances", component: <Customize audio={""} setAudio={""}/>},
  { number: 4, label: "Agent Wiki", component: <CustomizeWiki /> },
  { number: 5, label: "Auto-Audit", component: <CustomizeAutoAudit />},
  { number: 6, label: "Action Workflow", component: <CustomiseActionWorkflow />},
  { number: 7, label: "Customer Info", component: <CallSummaryDetails />},
  { number: 8, label: "Call Summary", component: <CallSummaryTabs /> }
];
 
const ProgressBar = () => {
  const [activeStep, setActiveStep] = useState(1); 
 
  const handlePrevious = () => {
    if (activeStep > 1) setActiveStep(prev => prev - 1);
  };
 
  const handleNext = () => {
    if (activeStep < steps.length) setActiveStep(prev => prev + 1);
  };
 
  return (
    <div className="progress-bar-container">
      <div style={{marginLeft:"110px", display:"flex"}}>
        {steps.map((step, index) => (
          <React.Fragment key={index}>
            <div
              onClick={() => setActiveStep(step.number)} // On click, change the active step
            >
              <div
                className="step-shape"
                style={{ backgroundColor: activeStep == step.number ? "#4CC11D" : ""}}
              >
                <div className="circle-number">
                  <span className="step-number">{step.number}</span>
                </div>
                <div className="step-label">{step.label}</div>
              </div>
            </div>
          </React.Fragment>
        ))}
      </div>
      <div className="step-content">
        {steps.find(step => step.number === activeStep)?.component}
      </div>
 
      <div className="navigation-buttons">
        <button onClick={handlePrevious} className='enabled '  style={{ backgroundColor: activeStep === 1 ? "#ccc" : ""}} disabled={activeStep === 1}>
          Previous
        </button>
        <button onClick={handleNext} className='enabled '  style={{ backgroundColor: activeStep === steps.length ? "#ccc" : ""}} disabled={activeStep === steps.length}>
          Next
        </button>
      </div>
    </div>
  );
};
 
export default ProgressBar;
 

.progress-bar-container {
    font-family: Arial, sans-serif;
    padding: 20px;
    margin-left: 14px;
  }
      
  .progress-bar {
    display: flex;
    align-items: center;
  }
   
  .progress-step {
    display: flex;
    flex-direction: column;
    align-items: center;
    text-align: center;
    margin-right: 10px;
    position: relative;
  }
   
  .step-shape {
    width: 125px;
    height: 57px;
    margin-left: 38px;
    background-color:#894D9A;
    clip-path: polygon(0% 0%, 85% 0%, 100% 50%, 85% 100%, 0% 100%);
    display: flex;
    justify-content: center;
    align-items: center;
    position: relative;
    margin-bottom: 5px;
    border: none;
  }
   
  .circle-number {
    width: 63px;
    height: 63px;
    background-color: white;
    color: black;
    border-radius: 50%;
    position: absolute;
    top: -3px;
    left: -19px;
    display: flex;
    justify-content: center;
    align-items: center;
    font-weight: bold;
    border: 3px solid white;
  }
   
  .step-number {
    font-size: 14px;
  }
   
  .step-label {
    padding: 2px;
    color: white;
    font-size: 14px;
    max-width: 80px;
    margin-left:42px;
  }
  
  .step-content{
    display: flex;
    flex-direction: row;
    gap: 10px;
    margin-top: 10px;
    padding: 20px;
    margin-left: 60px;
  }
   
  .navigation-buttons {
    position: fixed;
    margin-top: 20px;
    display: flex;
    align-items: end;
    justify-content: end;
    width: 100%;
    bottom: 1rem;
    right: 2rem;
  }
   
   .enabled {
    margin-right: 35px;
    padding: 8px 16px;
    font-size: 16px;
    background-color: #773D87;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    outline: none;
  }
