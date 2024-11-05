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

function Body({ transcript, setTranscript, setIndex, index ,selectNudge}) {
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
    const newTranscript = [...transcript]
    let context = newTranscript[index].eventData?.Context ?
      newTranscript[index].eventData.Context :
      []
    context.push(newContext)
    newTranscript[index].eventData.Context = context
    setTranscript(newTranscript)
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
    const newTranscript = [...transcript]
    let guidance = {
      "type": AI_ASSISTANT_TYPE.SPEECH_SUGGESTION,
      "name": "Speech Suggestion",
      "value": value
    }
    let Gui = newTranscript[index].eventData.Guidance ? newTranscript[index].eventData.Guidance : []
    Gui.push(guidance)
    newTranscript[index].eventData.Guidance = Gui
    setTranscript(newTranscript)
  }

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

  const handleAddKnowledgeArticle = (index) => {
    const newTranscript = [...transcript]
    let guidance = {
      "type": AI_ASSISTANT_TYPE.KNOWLEDGE_ARTICLE,
      "name": '',
      "value": []
    }
    let Gui = newTranscript[index].eventData.Guidance ? newTranscript[index].eventData.Guidance : []
    Gui.push(guidance)
    newTranscript[index].eventData.Guidance = Gui
    setTranscript(newTranscript)
  }

  return (
    <Slider
      dots={false}
      infinite={false}
      speed={250}
      slidesToShow={1}
      slidesToScroll={1}
      draggable = {false}
      prevArrow={<GrCaretPrevious color='black' />}
      nextArrow={<GrCaretNext color='black' />}
      afterChange={(current) => { setIndex(current) }}
      ref={slider => { sliderRef = slider }}
    >
      {transcript.map((obj, index) => {
        let text = obj.eventData.Transcript.Transcript
        let channelId = obj.eventData.Transcript.ChannelId

        let isRecommendation = text && text.includes('"Title"') && channelId === 'AGENT_ASSISTANT' ? JSON.parse(text) : null

        return (
          <div key={index} className='transcript-div'>

            <div className='transcript-div-child'>
              {isRecommendation ? 
                <div className='transcript-div-recommendation' >
                  <input
                    className='transcript-div-recommendation-input'
                    type="text"
                    placeholder='Title'
                    value={isRecommendation.Title}
                    onChange={(e) => {handleEditRecomendation(index, 'Title', e.target.value)}}
                    required
                  />
                  <input
                    className='transcript-div-recommendation-input'
                    type="text"
                    placeholder='subTitle'
                    value={isRecommendation.subTitle}
                    onChange={(e) => {handleEditRecomendation(index, 'subTitle', e.target.value)}}
                    required
                  />
                  <textarea
                    className='transcript-div-textarea'
                    type="text"
                    placeholder='message'
                    value={isRecommendation.message}
                    onChange={(e) => {handleEditRecomendation(index, 'message', e.target.value)}}
                    required
                  />
                </div> :
                <textarea
                  className='transcript-div-textarea'
                  placeholder='Add recommendation / transcript here'
                  value={text}
                  onChange={(e) => handleEdit(index, 'Transcript', e.target.value)}
                  required
                />}
            </div>

            <div className='transcript-div-child'>
              <div className="customize-btns">
                {/* <button
                  className='transcript-div-button'
                  onClick={() => handleAddAudit(index, { section: "", auditStatus: "FAIL", questions: [] })}
                >
                  Add Audit
                </button> */}

              {selectNudge === 'cc' && <button
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

                  {selectNudge === 'ss' &&<button
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

                {selectNudge === 'ka' &&<button
                  className='transcript-div-button'
                  onClick={() => handleAddKnowledgeArticle(index)}
                >
                  Add Knowledge Article
                </button>}
              </div>
              <div className='customize-btns'>
                {/* <button
                  className="transcript-div-button"
                  onClick={() => { handleAddRecommendation(index, (index !== 0 ? transcript[index - 1].EndTime : 0), obj.StartTime, transcript, setTranscript, setIndex, 'before') }}
                >
                  Add Guidance Before
                </button> */}

                {selectNudge === 'ai' && <button
                  className="transcript-div-button"
                  onClick={() => { handleAddRecommendation(index, obj.EndTime, (index !== transcript.length - 1 ? transcript[index + 1].StartTime : obj.EndTime), transcript, setTranscript, setIndex) }}
                >
                  Add Guidance
                </button>}

              <button className='transcript-div-button' onClick={() => handleRemove(index, transcript, setTranscript)}>
                Remove Event
              </button>

              <button
                className='transcript-div-button'
                onClick={() => handleAddButton(index, { intialText: '', finalText: '' })}
              >
                Add Button
              </button>

            </div>

            {selectNudge === 'utterance' && <DisplayChannelTime obj={obj} index={index} handleEdit={handleEdit} handleEditTime={handleEditTime} />

            /*<DisplayAudits objIndex={index} audits={obj.eventData.Audits ? obj.eventData.Audits : []} handleRemoveAudit={handleRemoveAudit} handleEditInsightsAuditsWiki={handleEditInsightsAuditsWiki} />
            
            <DisplayCallContext objIndex={index} callContext={obj.eventData.Context ? obj.eventData.Context : []} handleEditInsightsAuditsWiki={handleEditInsightsAuditsWiki} handleRemoveCallContext={handleRemoveCallContext} />

            <DisplayButton objIndex={index} button={obj.eventData.Button ? obj.eventData.Button : []} handleEditInsightsAuditsWiki={handleEditInsightsAuditsWiki} handleRemoveButton={handleRemoveButton} />

            <DisplaySentiment objIndex={index} sentimentScore={obj.eventData.Transcript.SentimentScore} handleEdit={handleEdit} handleAdjustSentiment={handleAdjustSentiment} />

            <DisplayAIWiki objIndex={index} wikiChats={obj.eventData.AIWikiChat ? obj.eventData.AIWikiChat : []} handleEditInsightsAuditsWiki={handleEditInsightsAuditsWiki} handleRemoveWiki={handleRemoveWiki} />

            <DisplayWorkflow objIndex={index} guidance={obj.eventData.Guidance ? obj.eventData.Guidance : []} handleEditWorkflow={handleEditWorkflow} handleEditWorkflowStep={handleEditWorkflowStep} handleAddWorkflowStep={handleAddWorkflowStep} handleEditKnowledgeArticle={handleEditKnowledgeArticle} handleRemoveKnowledgeArticle={handleRemoveKnowledgeArticle} handleRemoveGuidance={handleRemoveGuidance} handleRemoveWorkflowStep={handleRemoveWorkflowStep} handleEditSpeech={handleEditSpeech} /> */}

          </div>
        )
      })}
    </Slider>
  )
}

export default Body;
