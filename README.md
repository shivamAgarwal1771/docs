import React, { useState } from 'react';
import AgentDetails from '../../components/demoCreationComponents/upload-demo/AgentDetails';
import MediaSelection from '../../components/demoCreationComponents/upload-demo/MediaSelection';
import Customize from "../../components/demoCreationComponents/customise/customisejson";
import CallSummaryDetails from "../../components/demoCreationComponents/upload-demo/CallSummaryDetails";
import CallSummaryTabs from "../../components/demoCreationComponents/upload-demo/callSummaryinfo";
import CustomizeWiki from "../../components/demoCreationComponents/customise/Aiwiki";
import CustomizeAutoAudit from "../../components/demoCreationComponents/customise/AutoAuditjson";
import CustomiseActionWorkflow from "../../components/demoCreationComponents/customise/ActionWorkflowjson";

const steps = [
  { number: 1, label: "Demo Details", component: <AgentDetails handleInput={() => { }} metadata={"text"} /> },
  { number: 2, label: "Demo Resources", component: <MediaSelection /> },
  { number: 3, label: "Nudges & Utterances", component: <Customize audio={""} setAudio={""} /> },
  { number: 4, label: "Agent Wiki", component: <CustomizeWiki /> },
  { number: 5, label: "Auto-Audit", component: <CustomizeAutoAudit /> },
  { number: 6, label: "Action Workflow", component: <CustomiseActionWorkflow /> },
  { number: 7, label: "Customer Info", component: <CallSummaryDetails /> },
  { number: 8, label: "Call Summary", component: <CallSummaryTabs /> }
];

const ProgressBar = () => {
  const [activeStep, setActiveStep] = useState(1);
  const [completedSteps, setCompletedSteps] = useState([]);

  const handlePrevious = () => {
    if (activeStep > 1) setActiveStep(prev => prev - 1);
  };

  const handleNext = () => {
    if (activeStep < steps.length) {
      setActiveStep(prev => prev + 1);
      // Mark the current step as completed if not already
      if (!completedSteps.includes(activeStep)) {
        setCompletedSteps(prev => [...prev, activeStep]);
      }
    }
  };

  return (
    <div className="progress-bar-container">
      <div style={{ marginLeft: "110px", display: "flex" }}>
        {steps.map((step, index) => {
          const isCompleted = completedSteps.includes(step.number);
          const isActive = step.number === activeStep;

          return (
            <div key={index} onClick={() => setActiveStep(step.number)}>
              <div
                className="step-shape"
                style={{
                  backgroundColor: isActive ? "#FF0000" : isCompleted ? "#4CC11D" : "#D3D3D3"
                }}
              >
                <div className="circle-number">
                  <span className="step-number">{step.number}</span>
                </div>
                <div className="step-label">{step.label}</div>
              </div>
            </div>
          );
        })}
      </div>
      <div className="step-content">
        {steps.find(step => step.number === activeStep)?.component}
      </div>

      <div className="navigation-buttons">
        <button onClick={handlePrevious} className="enabled" style={{ backgroundColor: activeStep === 1 ? "#ccc" : "" }} disabled={activeStep === 1}>
          Previous
        </button>
        <button onClick={handleNext} className="enabled" style={{ backgroundColor: activeStep === steps.length ? "#ccc" : "" }} disabled={activeStep === steps.length}>
          Next
        </button>
      </div>
    </div>
  );
};

export default ProgressBar;
