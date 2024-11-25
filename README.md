import React, { useEffect, useState } from 'react';
import Body from './Body';
import { useDispatch, useSelector } from 'react-redux';
import json from "../../../assets/resource/andydemo.json"
import { MdDownload } from "react-icons/md";
import TranscriptPreview from './Preview';
import { FaPlus, FaMinus } from "react-icons/fa6";
import { checkJson } from '../../../utility/customise/checkJson';
import AgentWikiBody from "./agentWikiBody"
import AutoAuditBody from "./AutoAuditBody"
import ActionWorkflow from "./actionWorkflow"
import {setProgressBarTranscript} from "../../../store/callSlice"
import { setDemoTranscriptSubmitted, setEditDemoTranscript, updateDemoData } from '../../../store/demo-slice';


function FileUpload({ setTranscript, message, setMessage }) {
  const dispatch = useDispatch()
  const onUpload = (e) => {
    const file = e.target.files[0];
    if (!file) return;

    const filereader = new FileReader();
    filereader.readAsText(file, 'UTF-8');

    filereader.onload = (e) => {
      let content = e.target.result;

      try {
        let jsonData = JSON.parse(content);
        let reviewJson = checkJson(jsonData);
        if (!reviewJson.value) {
          setMessage(reviewJson.msg);
          setTimeout(() => {
            setMessage('');
          }, 8000);
        } else {
          jsonData.forEach((obj) => {
            if (obj.eventData.Context && !Array.isArray(obj.eventData.Context)) {
              obj.eventData.Context = Array(obj.eventData.Context);
            }
          });
          setTranscript(jsonData);
        }
      } catch (err) {
        setMessage('Not a valid JSON or format');
        setTimeout(() => {
          setMessage('');
        }, 8000);
        console.error(err);
      }
    };
  }

  return (
    <form className='body-container'>
      <label htmlFor='json-file' className='upload-btn tab btn-active'>+ Upload</label>
      <input style={{ visibility: 'hidden' }} id='json-file' type='file' acontextNudgeTabept='application/JSON' onChange={onUpload} />
      {message && <p style={{ color: 'red' }}>{message}</p>}
    </form>
  );
}

export default function Customize({finalTranscript,setFinalTranscript,transcripts, setTranscripts,handleInput, audio, setAudio, tab }) {
  const demoState = useSelector((state) => state?.demostate);
  const appCallData = useSelector((state) => state?.call);
  const isEditDemo = demoState?.editDemo;
  const [transcript, setTranscript] = useState(demoState?.getDemoData?.transcript || []);
  const [message, setMessage] = useState(undefined);
  const [selectedNudge, setSelectedNudge] = useState('contextNudgeTab'); // Default to Call Context
  const [index, setIndex] = useState(0);
  const [progessScript,setProgessScript] = useState(true)
  const [fields, setFields] = useState({
    contextNudgeTab: [],
    aiNudgeTab: [],
    speechSuggestionNudge: [],
    knowledgeTab: [],
    utterance: [],
  });
  const dispatch = useDispatch();

  audio && setAudio(audio);

  const sendTranscriptData = () => {
    if (isEditDemo && transcript?.length) {
      dispatch(updateDemoData({ type: "transcript", data: transcript }));
    }
    dispatch(setEditDemoTranscript(false));
    dispatch(setDemoTranscriptSubmitted(true));
  };

  const downloadTranscript = () => {
    if (transcript) {
        // dispatch(setProgressBarTranscript(transcript))
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

  const handleNudgeType = (type) => {
    setSelectedNudge(type);
  };

  const addField = (type) => {
    setFields((prev) => ({
      ...prev,
      [type]: [...prev[type], prev[type].length + 1],
    }));
  };

  const removeField = (type, index) => {
    setFields((prev) => ({
      ...prev,
      [type]: prev[type].filter((_, i) => i !== index),
    }));
  };

  useEffect(()=>{
    transcript?.length && setFinalTranscript(transcript)
    progessScript && appCallData.progressBarTranscript && setFinalTranscript(appCallData.progressBarTranscript)
    progessScript && appCallData.progressBarTranscript && setTranscript(appCallData.progressBarTranscript);
    setProgessScript(false)
  },[transcript,appCallData.progressBarTranscript])

  return (
    <div className='transcript-container'>
      {tab==1&&<div className="nudge-types">
        <div className="options-types">NUDGE TYPE AND UTTERANCES</div>
        <div className="nudge-options">
          {['contextNudgeTab', 'aiNudgeTab', 'speechSuggestionNudge', 'knowledgeTab', 'utterance'].map(type => (
            <div key={type} 
              className={`nudge-option ${type === 'contextNudgeTab' ? "first-nudge" : (type === 'utterance' ? "last-nudge" : "mid-nudge")}`} 
              onClick={() => handleNudgeType(type)}
            >
              <span>{type === 'contextNudgeTab' ? '+ CALL CONTEXT' : type === 'aiNudgeTab' ? '+ AI GUIDANCE' : type === 'speechSuggestionNudge' ? '+ SPEECH SUGGESTION' : type === 'knowledgeTab' ? '+ KNOWLEDGE ARTICLE' : '+ UTTERANCES'}</span>
            </div>
          ))}
        </div>
      </div>}
       
      {!transcript?.length && <FileUpload setTranscript={setTranscript} message={message} setMessage={setMessage} />}
      {transcript?.length &&
        <div style={{ display: 'flex', justifyContent: 'space-between' }}>
          <div className='transcript-body'>
            {tab==1&&<Body tabs={1} transcript={transcript} setTranscript={setTranscript} selectNudge= {selectedNudge} setIndex={setIndex} index={index}/>}
            {tab==2&&<AgentWikiBody tabs={2} transcript={transcript} setTranscript={setTranscript} selectNudge= {selectedNudge} setIndex={setIndex} index={index}/>}
            {tab==3&&<AutoAuditBody tabs={3} transcript={transcript} setTranscript={setTranscript} selectNudge= {selectedNudge} setIndex={setIndex} index={index}/>}
            {tab==4&&<ActionWorkflow tabs={4} transcript={transcript} setTranscript={setTranscript} selectNudge= {selectedNudge} setIndex={setIndex} index={index}/>}
          </div>
          <div className={`transcript-preview ${tab == 1 ? "transcript-preview" : "transcript-preview-enlarged"}`}>
            <TranscriptPreview
              transcript={transcript}
              setTranscript={setTranscript}
              setIndex={setIndex}
              index={index}
            />
          </div>
        </div>}
    </div>
  );
}


import React, { useEffect, useRef, useState } from 'react'
import Slider from 'react-slick'
import 'slick-carousel/slick/slick.css'
import 'slick-carousel/slick/slick-theme.css'
import { GrCaretPrevious, GrCaretNext } from "react-icons/gr"
import { DisplayAudits, DisplaySentiment, DisplayAIWiki, DisplayCallContext, DisplayButton, DisplayChannelTime, DisplayWorkflow } from './Display'
import { AI_ASSISTANT_TYPE } from '../../../utility/constants'
import { v4 as uuidv4 } from 'uuid';

export const handleRemove = (index, transcript, setTranscript) => {
  const newTranscript = [...transcript]
  newTranscript.splice(index, 1)
  setTranscript(newTranscript)
}

export const handleAddRecommendation = (index, prevEndTime, nextStartTime, transcript, setTranscript, setIndex, type) => {
  const newTranscript = [...transcript]
  let newStartTime = (Number(prevEndTime) + Number(nextStartTime)) / 2
  let recommendation = {
    "StartTime": newStartTime.toFixed(3),
    "EndTime": (newStartTime + 1).toFixed(3),
    "eventData": {
      "Transcript": {
        "Transcript": '{"Title":"","subTitle":"","message":""}',
        "ChannelId": "AGENT_ASSISTANT",
        "StartTime": newStartTime.toFixed(3),
        "EndTime": (newStartTime + 1).toFixed(3),
        "IsPartial": false,
        "ResultId": uuidv4(),
        "Sentiment": "Neutral",
        "SentimentScore": {
          "Positive": 0.1,
          "Negative": 0.1,
          "Neutral": 0.8,
          "Mixed": 0
        }
      },
      "Audits": []
    }
  }
  if (type === 'before') {
    newTranscript.splice(index, 0, recommendation)
    setIndex(index)
  } else {
    newTranscript.splice(index + 1, 0, recommendation)
    setIndex(index + 1)
  }
  setTranscript(newTranscript)
}

function Body({ tabs, transcript, setTranscript, setIndex, index, selectNudge }) {
  let sliderRef = useRef(null)
  useEffect(() => {
    sliderRef.slickGoTo(index)
  }, [index])

  const handleEdit = (index, field, newValue) => {
    const newTranscript = [...transcript]
    newTranscript[index].eventData.Transcript[field] = newValue
    setTranscript(newTranscript)
  }

  const handleEditTime = (index, type, newValue) => {
    const newTranscript = [...transcript]
    newTranscript[index][type] = newValue
    newTranscript[index].eventData.Transcript[type] = newValue
    setTranscript(newTranscript)
  }

  const handleEditInsightsAuditsWiki = (index, type, newValue) => {
    const newTranscript = [...transcript]
    newTranscript[index].eventData[type] = newValue
    setTranscript(newTranscript)
  }

  const handleEditKnowledgeArticle = (objIndex, type, value) => {
    const newTranscript = [...transcript]
    newTranscript[objIndex].eventData[type] = value
    setTranscript(newTranscript)
  }


  const handleEditRecomendation = (index, field, newValue) => {
    const newTranscript = [...transcript]
    const newRecommendation = JSON.parse(newTranscript[index].eventData.Transcript.Transcript)
    newRecommendation[field] = newValue
    newTranscript[index].eventData.Transcript.Transcript = JSON.stringify(newRecommendation)
    setTranscript(newTranscript)
  }

  

  const handleEditWorkflow = (objIndex, index, type, value) => {
    const newTranscript = [...transcript]
    newTranscript[objIndex].eventData.Guidance[index][type] = value
    if (value !== 'running') {
      newTranscript[objIndex].eventData.Guidance[index]["steps"] = []
    }
    setTranscript(newTranscript)
  }

  const handleEditSpeech = (objIndex, index, value) => {
    const newTranscript = [...transcript]
    newTranscript[objIndex].eventData.Guidance[index].value = value
    setTranscript(newTranscript)
  }

  const handleEditWorkflowStep = (objIndex, guidInd, index, type, value) => {
    const newTranscript = [...transcript]
    let step = newTranscript[objIndex].eventData.Guidance[guidInd].steps[index]
    if (type === 'stepName' || type === 'state') {
      step[type] = value
    }
    if (type === 'type') {
      step.card.type = value
    }
    if (type === 'text') {
      step.card.messages[0].text = value
    }
    newTranscript[objIndex].eventData.Guidance[guidInd].steps[index] = step
    setTranscript(newTranscript)
  }

  const handleRemoveAudit = (index, auditIndex) => {
    const newTranscript = [...transcript]
    newTranscript[index]?.eventData?.Audits.splice(auditIndex, 1)
    setTranscript(newTranscript)
  }

  const handleRemoveWiki = (index, wikiIndex) => {
    const newTranscript = [...transcript]
    newTranscript[index].eventData.AIWikiChat.splice(wikiIndex, 1)
    setTranscript(newTranscript)
  }

  const handleRemoveGuidance = (index, guidInd) => {
    const newTranscript = [...transcript]
    newTranscript[index].eventData.Guidance.splice(guidInd, 1)
    setTranscript(newTranscript)
  }

  const handleRemoveWorkflowStep = (index, guidInd, stepInd) => {
    const newTranscript = [...transcript]
    newTranscript[index].eventData.Guidance[guidInd].steps.splice(stepInd, 1)
    setTranscript(newTranscript)
  }

  const handleRemoveKnowledgeArticle = (index, guidInd) => {
    const newTranscript = [...transcript]
    newTranscript[index]?.eventData?.Guidance.splice(guidInd, 1)
    setTranscript(newTranscript)
  }

  const handleRemoveCallContext = (index, guidInd) => {
    const newTranscript = [...transcript]
    newTranscript[index]?.eventData?.Context?.splice(guidInd, 1)
    setTranscript(newTranscript)
  }

  const handleRemoveButton = (index, guidInd) => {
    const newTranscript = [...transcript]
    newTranscript[index]?.eventData?.Button?.splice(guidInd, 1)
    setTranscript(newTranscript)
  }

  const handleAddAudit = (index, newAudit) => {
    const newTranscript = [...transcript]

    let audit = newTranscript[index].eventData?.Audits ?
      newTranscript[index].eventData.Audits :
      []
    audit.push(newAudit)
    newTranscript[index].eventData.Audits = audit
    setTranscript(newTranscript)
  }

  const handleAddContext = (index, newContext) => {
    const newTranscript = [...transcript]; // Clone the transcript array

    // Clone the eventData object before modifying it
    const updatedEventData = { 
        ...newTranscript[index].eventData, 
        Context: newTranscript[index].eventData?.Context ? 
            [...newTranscript[index].eventData.Context, newContext] : 
            [newContext] 
    };

    // Replace the eventData with the updated version
    newTranscript[index] = {
        ...newTranscript[index],
        eventData: updatedEventData
    };

    setTranscript(newTranscript);
}


  const handleAddButton = (index, newButton) => {
    const newTranscript = [...transcript]
    let button = newTranscript[index].eventData?.Button ?
      newTranscript[index].eventData.Button :
      []
    button.push(newButton)
    newTranscript[index].eventData.Button = button
    setTranscript(newTranscript)
  }

  const handleAddWiki = (index, newWiki) => {
    const newTranscript = [...transcript]
    let chats = newTranscript[index].eventData?.AIWikiChat ?
      newTranscript[index].eventData.AIWikiChat :
      []
    chats.push(newWiki)
    newTranscript[index].eventData.AIWikiChat = chats
    setTranscript(newTranscript)
  }

  const handleAdjustSentiment = (index) => {
    const newTranscript = [...transcript]
    let pos = newTranscript[index].eventData.Transcript.SentimentScore.Positive
    let neg = newTranscript[index].eventData.Transcript.SentimentScore.Negative
    let neu = newTranscript[index].eventData.Transcript.SentimentScore.Neutral

    let sen = Math.max(pos, neg, neu) === pos ? "Positive" : Math.max(pos, neg, neu) === neg ? "Negative" : "Neutral"

    newTranscript[index].eventData.Transcript.Sentiment = sen
    setTranscript(newTranscript)
  }

  const handleAddSpeechSuggestion = (index, value) => {
    // Clone the transcript array immutably
    const newTranscript = [...transcript];
  
    // Create the new guidance object
    const guidance = {
      type: AI_ASSISTANT_TYPE.SPEECH_SUGGESTION,
      name: 'Speech Suggestion',
      value: value,
    };
  
    // Clone the eventData and Guidance properties immutably
    const updatedEventData = {
      ...newTranscript[index].eventData,
      Guidance: newTranscript[index].eventData.Guidance ? 
        [...newTranscript[index].eventData.Guidance, guidance] : 
        [guidance],
    };
  
    // Replace the eventData at the specified index with the updated one
    newTranscript[index] = {
      ...newTranscript[index],
      eventData: updatedEventData,
    };
  
    // Update the state immutably
    setTranscript(newTranscript);
  };
  
  
  
  const handleAddKnowledgeArticle = (index) => {
    // Clone the transcript array immutably
    const newTranscript = [...transcript];
  
    // Create the new guidance object
    const guidance = {
      type: AI_ASSISTANT_TYPE.KNOWLEDGE_ARTICLE,
      name: '',
      value: [],
    };
  
    // Clone the eventData and Guidance properties immutably
    const updatedEventData = {
      ...newTranscript[index].eventData,
      Guidance: newTranscript[index].eventData.Guidance ? 
        [...newTranscript[index].eventData.Guidance, guidance] : 
        [guidance],
    };
  
    // Replace the eventData at the specified index with the updated one
    newTranscript[index] = {
      ...newTranscript[index],
      eventData: updatedEventData,
    };
  
    // Update the state immutably
    setTranscript(newTranscript);
  };
  



  const handleAddActionWorkflow = (index, type, intent, name) => {
    const newTranscript = [...transcript]
    let guidance = {
      "type": AI_ASSISTANT_TYPE.ACTION_WORKFLOW,
      "cardShown": true,
      "workflowType": type,
      "intent": intent,
      "name": name,
      "steps": []
    }
    let Gui = newTranscript[index].eventData.Guidance ? newTranscript[index].eventData.Guidance : []
    Gui.push(guidance)
    newTranscript[index].eventData.Guidance = Gui
    setTranscript(newTranscript)
  }

  const handleAddWorkflowStep = (index, guidInd, name, state, type, text) => {
    const newTranscript = [...transcript]
    let runningFlow = newTranscript[index].eventData.Guidance[guidInd]
    if (runningFlow.workflowType !== 'running') {
      return
    }

    let step = {
      "stepName": name,
      "state": state,
      "card": {
        "type": type,
        "isEditable": false,
        "messages": [
          {
            "text": text
          }
        ]
      }
    }

    runningFlow.steps.push(step)

    newTranscript[index].eventData.Guidance[guidInd] = runningFlow
    setTranscript(newTranscript)
  }

  const handleStartActionWorkflow = (index, intent) => {
    const newTranscript = [...transcript]
    let guidance = {
      "type": AI_ASSISTANT_TYPE.ACTION_WORKFLOW,
      "cardShown": false,
      "intent": intent,
    }
    let Gui = newTranscript[index].eventData.Guidance ? newTranscript[index].eventData.Guidance : []
    Gui.push(guidance)
    newTranscript[index].eventData.Guidance = Gui
    setTranscript(newTranscript)
  }

  const handleClick = (index) => {
    let falseworkflow = true

    transcript.forEach(element => {
      element?.eventData?.Guidance?.forEach(el => {
        if (el?.cardShown === false) {
          falseworkflow = false
        }
      })
    })

    if (falseworkflow) {
      handleStartActionWorkflow(index, '')
    }
  }



  return (
    <Slider
      dots={false}
      infinite={false}
      speed={250}
      slidesToShow={1}
      slidesToScroll={1}
      draggable={false}
      prevArrow={<GrCaretPrevious color='black' className='backward-arrow'/>}
      nextArrow={<GrCaretNext color='black' className='forward-arrow'/>}
      afterChange={(current) => { setIndex(current) }}
      ref={slider => { sliderRef = slider }}
    >
      {transcript?.map((obj, index) => {
        let text = obj.eventData.Transcript.Transcript
        let channelId = obj.eventData.Transcript.ChannelId

        let isRecommendation = text && text.includes("Title") && channelId === 'AGENT_ASSISTANT' ? JSON.parse(text) : null

        return (
          <div key={index} className='transcript-div'>

            <button className='transcript-div-close-button' onClick={() => handleRemove(index, transcript, setTranscript)}>
              X
            </button>

            {<DisplayChannelTime nudge={selectNudge} obj={obj} index={index} handleEdit={handleEdit} handleEditTime={handleEditTime} />}

            <div className='transcript-div-child'>
              {isRecommendation ?
                <div className='transcript-div-recommendation' >
                  <input
                    className='transcript-div-recommendation-input'
                    type="text"
                    placeholder='Title'
                    value={isRecommendation.Title}
                    onChange={(e) => { handleEditRecomendation(index, 'Title', e.target.value) }}
                    required
                  />
                  {!(selectNudge=="utterance")&&<input
                    className='transcript-div-recommendation-input'
                    type="text"
                    placeholder='subTitle'
                    value={isRecommendation.subTitle}
                    onChange={(e) => { handleEditRecomendation(index, 'subTitle', e.target.value) }}
                    required
                  />}
                  {!(selectNudge=="utterance")&&<textarea
                    className='transcript-div-textarea'
                    type="text"
                    placeholder='message'
                    value={isRecommendation.message}
                    onChange={(e) => { handleEditRecomendation(index, 'message', e.target.value) }}
                    required
                  />}
                </div> :
                <textarea
                  className='transcript-div-textarea-only'
                  placeholder='Add recommendation / transcript here'
                  value={text}
                  onChange={(e) => handleEdit(index, 'Transcript', e.target.value)}
                  required
                />}
            </div>
            {<DisplaySentiment nudge={selectNudge} objIndex={index} sentimentScore={obj.eventData.Transcript.SentimentScore} handleEdit={handleEdit} handleAdjustSentiment={handleAdjustSentiment} />}

            <div className='transcript-div-child'>
              <div className="customize-btns">
                {/* <button
                  className='transcript-div-button'
                  onClick={() => handleAddAudit(index, { section: "", auditStatus: "FAIL", questions: [] })}
                >
                  Add Audit
                </button> */}

                {selectNudge === 'contextNudgeTab' && <button
                  className='transcript-div-button'
                  onClick={() => handleAddContext(index, { Title: '', Content: '' })}
                >
                  Add Call Context
                </button>}

                {/* <button
                  className='transcript-div-button'
                  onClick={() => handleAddWiki(index, { user: '', utterance: '' })}
                >
                  Add AI Wiki
                </button> */}

                {selectNudge === 'speechSuggestionNudge' && <button
                  className='transcript-div-button'
                  onClick={() => handleAddSpeechSuggestion(index, '')}
                >
                  Add Speech Suggestion
                </button>}
              </div>
            </div>
            <div className='customize-btns'>
              {/* <button
                  className='transcript-div-button'
                  onClick={() => handleAddActionWorkflow(index, 'detected', '', '')}
                >
                  Add Action Workflow
                </button> */}

              {/* <button
                  className='transcript-div-button'
                  onClick={() => handleClick(index, '')}
                >
                  Start Action Workflow
                </button> */}

              {selectNudge === 'knowledgeTab' && <button
                className='transcript-div-button'
                onClick={() => handleAddKnowledgeArticle(index)}
              >
                Add Knowledge Article
              </button>}
            </div>
            <div className='customize-btns'>
             {selectNudge === 'aiNudgeTab' && <button
                  className="transcript-div-button"
                  onClick={() => { handleAddRecommendation(index, (index !== 0 ? transcript[index - 1].EndTime : 0), obj.StartTime, transcript, setTranscript, setIndex, 'before') }}
                >
                  Add Guidance Before
                </button>}

              {selectNudge === 'aiNudgeTab' && <button
                className="transcript-div-button"
                onClick={() => { handleAddRecommendation(index, obj.EndTime, (index !== transcript.length - 1 ? transcript[index + 1].StartTime : obj.EndTime), transcript, setTranscript, setIndex) }}
              >
                Add Guidance
              </button>}

              {/* <button className='transcript-div-button' onClick={() => handleRemove(index, transcript, setTranscript)}>
                Remove Event
              </button> */}

              {selectNudge === 'utterance' &&<button
                className='transcript-div-add-button'
                onClick={() => { handleAddRecommendation(index, obj.EndTime, (index !== transcript.length - 1 ? transcript[index + 1].StartTime : obj.EndTime), transcript, setTranscript, setIndex) }}
              >
                Add Button
              </button>}

            </div>

            {/* {<DisplayAudits objIndex={index} audits={obj.eventData.Audits ? obj.eventData.Audits : []} handleRemoveAudit={handleRemoveAudit} handleEditInsightsAuditsWiki={handleEditInsightsAuditsWiki} />} */}
            
            {selectNudge ==='contextNudgeTab' &&<DisplayCallContext objIndex={index} callContext={obj.eventData.Context ? obj.eventData.Context : []} handleEditInsightsAuditsWiki={handleEditInsightsAuditsWiki} handleRemoveCallContext={handleRemoveCallContext} />}

            {/* {<DisplayButton objIndex={index} button={obj.eventData.Button ? obj.eventData.Button : []} handleEditInsightsAuditsWiki={handleEditInsightsAuditsWiki} handleRemoveButton={handleRemoveButton} />}

            {<DisplayAIWiki objIndex={index} wikiChats={obj.eventData.AIWikiChat ? obj.eventData.AIWikiChat : []} handleEditInsightsAuditsWiki={handleEditInsightsAuditsWiki} handleRemoveWiki={handleRemoveWiki} />*/}

            {<DisplayWorkflow selectedNudges ={selectNudge} objIndex={index} guidance={obj.eventData.Guidance ? obj.eventData.Guidance : []} handleEditWorkflow={handleEditWorkflow} handleEditWorkflowStep={handleEditWorkflowStep} handleAddWorkflowStep={handleAddWorkflowStep} handleEditKnowledgeArticle={handleEditKnowledgeArticle} handleRemoveKnowledgeArticle={handleRemoveKnowledgeArticle} handleRemoveGuidance={handleRemoveGuidance} handleRemoveWorkflowStep={handleRemoveWorkflowStep} handleEditSpeech={handleEditSpeech} />}
          </div>
        )
      })}
    </Slider>
  )
}

export default Body;
