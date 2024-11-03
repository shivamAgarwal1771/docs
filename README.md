import React, { useEffect, useState } from 'react';
import Body from './Body';
import { useDispatch, useSelector } from 'react-redux'
import { MdDownload } from "react-icons/md";
import TranscriptPreview from './Preview';
import { handleAddRecommendation, handleRemove } from './Body';
import { FaArrowRightFromBracket } from "react-icons/fa6";
import { checkJson } from '../../../utility/customise/checkJson';
import { setDemoTranscriptSubmitted, setEditDemoTranscript, updateDemoData } from '../../../store/demo-slice';

function FileUpload({ setTranscript, message, setMessage }) {
  const onUpload = (e) => {
    const filereader = new FileReader()
    filereader.readAsText(e.target.files[0], 'UTF-8')
    filereader.onload = (e) => {
      let content = e.target.result

      try {
        let jsonData = JSON.parse(content)
        let reviewJson = checkJson(jsonData)
        if (!reviewJson.value) {
          setMessage(reviewJson.msg)
          setTimeout(() => {
            setMessage('')
          }, [8000])
        } else {
          if (jsonData) {
            jsonData?.forEach((obj) => {
              if (obj.eventData.Context && !Array.isArray(obj.eventData.Context)) {
                obj.eventData.Context = Array(obj.eventData.Context);
              }
            })
          }
          setTranscript(jsonData)
        }
      } catch (err) {
        setMessage('Not a valid JSON or format')
        setTimeout(() => {
          setMessage('')
        }, [8000])
        console.error(err)
      }
    }
  }

  return (
    <form className='body-container'>
      <label htmlFor='json-file' className='upload-btn tab btn-active'>+ Upload</label>
      <input style={{ visibility: 'hidden' }} id='json-file' type='file' accept='application/JSON' onChange={e => onUpload(e)} />
      {message && <p style={{ color: 'red' }}>{message}</p>}
    </form>
  )
}

function JsonPreview({ transcript, index }) {
  const [expand, setExpand] = useState(false)

  // return (
  //   <div className={`transcript-json-div ${expand ? '' : 'json-collapse'}`} onClick={() => { setExpand(!expand) }}>
  //     <div className={`expand-btn ${expand ? '' : 'rotate'}`}>
  //       <FaArrowRightFromBracket />
  //     </div>
  //     <div className={`transcript-json ${expand ? '' : 'scrollbar-none'}`} >
  //       <pre>{JSON.stringify(transcript[index], null, 2)}</pre>
  //     </div>
  //   </div>
  // )
}

export default function Customize({ audio, setAudio }) {
  const demoState = useSelector((state) => state?.demostate);
  const isEditDemo = demoState?.editDemo;
  const [transcript, setTranscript] = useState(demoState?.getDemoData?.transcript);
  const [message, setMessage] = useState(undefined)
  const [index, setIndex] = useState(0);
  const dispatch = useDispatch();

  audio && setAudio(audio);

  const downloadTranscript = () => {
    if (transcript) {
      const transcriptURL = URL.createObjectURL(new Blob([JSON.stringify(transcript)], { type: 'application/json' }))

      const a = document.createElement('a')
      a.href = transcriptURL

      a.download = "transcription.json"
      a.style.display = "none"
      document.body.appendChild(a)

      a.click()

      document.body.removeChild(a)
      URL.revokeObjectURL(transcriptURL)
    }
  }

  const sendTranscriptData = () => {
    if (isEditDemo && transcript?.length) {
      dispatch(updateDemoData({ type: "transcript", data: transcript }))
    }
    dispatch(setEditDemoTranscript(false));
    dispatch(setDemoTranscriptSubmitted(true));
  };

  const handleBack = () => {
    dispatch(setEditDemoTranscript(false));
    dispatch(setDemoTranscriptSubmitted(false));
  };

  return (
    <div className='transcript-container'>
      {!transcript?.length && <FileUpload setTranscript={setTranscript} message={message} setMessage={setMessage} />}
      {transcript?.length &&
        <div style={{ display: 'flex' }}>
          <div className='transcript-body'>
            <Body transcript={JSON.parse(JSON.stringify(transcript))} setTranscript={setTranscript} setIndex={setIndex} index={index} />
          </div>
          <div className='transcript-preview'>

            <TranscriptPreview
              transcript={transcript}
              setTranscript={setTranscript}
              setIndex={setIndex}
              handleAdd={handleAddRecommendation}
              handleRemove={handleRemove}
              index={index}
            />
            <JsonPreview transcript={transcript} index={index} />
            <div style={{ display: 'flex' }}>
              {/* {!isEditDemo &&
                <button
                  className='transcript-download-btn btn-width'
                  onClick={() => downloadTranscript()}
                >
                  Download <MdDownload color='white' />
                </button>} */}
              {isEditDemo &&
                <>
                  <button
                    className='transcript-download-btn'
                    onClick={() => sendTranscriptData()}
                  >
                    Save Changes
                  </button>
                  <button
                    className='transcript-download-btn back-btn-right-align'
                    onClick={() => handleBack()}
                  >
                    Back
                  </button>
                </>}
            </div>
          </div>
        </div>}
    </div>
  )
}

import React, { useState } from 'react';
import { FaPlus, FaMinus } from 'react-icons/fa'; // Importing Plus and Minus icons

const NudgeForm = () => {
  const [selectedNudge, setSelectedNudge] = useState('cc'); // Default to Call Context
  const [isUtteranceMode, setUtteranceMode] = useState(false); // To track whether we are in Utterance mode
  const [fields, setFields] = useState({
    cc: [],
    ai: [],
    ss: [],
    ka: [],
  });

  // Function to handle selecting a nudge type
  const handleNudgeType = (type) => {
    setSelectedNudge(type);
    setUtteranceMode(false); // Reset utterance mode when switching nudges
  };

  // Function to add fields dynamically based on nudge type
  const addField = (type) => {
    setFields((prev) => ({
      ...prev,
      [type]: [...prev[type], prev[type].length + 1], // Add new field for the selected nudge type
    }));
  };

  // Function to remove a field
  const removeField = (type, index) => {
    setFields((prev) => ({
      ...prev,
      [type]: prev[type].filter((_, i) => i !== index), // Remove field at the specified index
    }));
  };

  // Function to switch to utterance mode
  const handleAddUtterance = () => {
    setUtteranceMode(true);
    setSelectedNudge(''); // Reset nudge type when switching to utterance
  };

  // Handle switching back to Call Context if "Add Nudge" is clicked
  const handleAddNudge = () => {
    setUtteranceMode(false);
    setSelectedNudge('cc'); // Reset to Call Context
  };

  return (
    <div className="nudge-layout">
      {/* Nudge Types Section at the Top */}
      <div className="nudge-types">
        <div className='options-types'>NUDGE TYPE</div>
        <div className="nudge-options">
          <div className="nudge-option" onClick={() => handleNudgeType('cc')}>
            <span>CALL CONTEXT</span>
          </div>
          <div className="nudge-option" onClick={() => handleNudgeType('ai')}>
            <span>AI GUIDANCE</span>
          </div>
          <div className="nudge-option" onClick={() => handleNudgeType('ss')}>
            <span>SPEECH SUGGESTION</span>
          </div>
          <div className="nudge-option" onClick={() => handleNudgeType('ka')}>
            <span>KNOWLEDGE ARTICLE</span>
          </div>
        </div>
      </div>

      <div className="main-content">
        {/* Left Column (Details Section) */}
        <div className="left-section">
          {!isUtteranceMode && selectedNudge === 'cc' && (
            <div className="field-section">
              <h3>Call Context Fields:</h3>
              {fields.cc.map((id, index) => (
                <div key={`cc-${id}`} className="row">
                  <div className="label">CC: Call Context Message {id}</div>
                  <input type="text" placeholder="Call Context Message" />
                  <button className="styled-button remove-button" onClick={() => removeField('cc', index)}>
                    <FaMinus /> Remove
                  </button>
                </div>
              ))}
              <button className="styled-button" onClick={() => addField('cc')}>
                <FaPlus /> Add More Call Context Fields
              </button>
            </div>
          )}

          {!isUtteranceMode && selectedNudge === 'ai' && (
            <div className="field-section">
              <h3>AI Guidance Fields:</h3>
              {fields.ai.map((id, index) => (
                <div key={`ai-${id}`} className="row">
                  <div className="label">AI: Message {id}</div>
                  <input type="text" placeholder="Title" />
                  <input type="text" placeholder="Subtitle" />
                  <input type="text" placeholder="Start Time" />
                  <input type="text" placeholder="End Time" />
                  <button className="styled-button remove-button" onClick={() => removeField('ai', index)}>
                    <FaMinus /> Remove
                  </button>
                </div>
              ))}
              <button className="styled-button" onClick={() => addField('ai')}>
                <FaPlus /> Add More AI Guidance Fields
              </button>
              <button className="styled-button" onClick={() => addField('ai-button')}>
                <FaPlus /> Add More AI Buttons
              </button>
              {fields['ai-button'] && fields['ai-button'].map((id, index) => (
                <div key={`ai-button-${id}`} className="row">
                  <input type="text" placeholder="Button Title" />
                  <input type="text" placeholder="Button Text" />
                  <button className="styled-button remove-button" onClick={() => removeField('ai-button', index)}>
                    <FaMinus /> Remove
                  </button>
                </div>
              ))}
            </div>
          )}

          {!isUtteranceMode && selectedNudge === 'ss' && (
            <div className="field-section">
              <h3>Speech Suggestion Fields:</h3>
              {fields.ss.map((id, index) => (
                <div key={`ss-${id}`} className="row">
                  <div className="label">SS: Speech Suggestion Message {id}</div>
                  <input type="text" placeholder="Title" />
                  <button className="styled-button remove-button" onClick={() => removeField('ss', index)}>
                    <FaMinus /> Remove
                  </button>
                </div>
              ))}
              <button className="styled-button" onClick={() => addField('ss')}>
                <FaPlus /> Add More Speech Suggestion Fields
              </button>
            </div>
          )}

          {!isUtteranceMode && selectedNudge === 'ka' && (
            <div className="field-section">
              <h3>Knowledge Article Fields:</h3>
              {fields.ka.map((id, index) => (
                <div key={`ka-${id}`} className="row">
                  <div className="label">KA: Knowledge Article {id}</div>
                  <input type="text" placeholder="Pointer" />
                  <button className="styled-button remove-button" onClick={() => removeField('ka', index)}>
                    <FaMinus /> Remove
                  </button>
                </div>
              ))}
              <button className="styled-button" onClick={() => addField('ka')}>
                <FaPlus /> Add More Knowledge Article Fields
              </button>
            </div>
          )}

          {/* Utterance Mode */}
          {isUtteranceMode && (
            <div>
              <h3>Utterance Fields</h3>
              <div className="row">
                <div className="label">Start Time:</div>
                <input type="text" placeholder="Start Time" />
              </div>
              <div className="row">
                <div className="label">End Time:</div>
                <input type="text" placeholder="End Time" />
              </div>
              <div className="row">
                <div className="label">Speaker:</div>
                <input type="text" placeholder="Agent or Customer" />
              </div>
              <div className="row">
                <div className="label">Sentiment:</div>
                <div>
                  <input type="radio" name="sentiment" value="positive" /> Positive
                  <input type="radio" name="sentiment" value="neutral" /> Neutral
                  <input type="radio" name="sentiment" value="negative" /> Negative
                </div>
              </div>
            </div>
          )}
        </div>

        {/* Right Column (Basic Info Section) */}
        <div className="right-section">
          <div className="row">
            <div className="label">Start Time</div>
            <input type="text" placeholder="Start Time" />
          </div>

          <div className="row">
            <div className="label">Customer or Agent</div>
            <input type="text" placeholder="Customer or Agent" />
          </div>

          <div className="row">
            <button className="styled-button" onClick={handleAddNudge}>Add Nudge</button>
            <button className="styled-button" onClick={handleAddUtterance}>Add Utterance</button>
          </div>

          <div className="row">
            <div className="label">End Time</div>
            <input type="text" placeholder="End Time" />
          </div>
            <div className="row">
            <div className="label">Notes</div>
            <input type="text" placeholder="Additional Notes" />
          </div>
        </div>
      </div>
    </div>
  );
};

export default NudgeForm;





