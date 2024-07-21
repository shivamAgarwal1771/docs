import React from 'react'
import { CiCircleRemove } from "react-icons/ci"

// export function DisplayInsights({objIndex, insights, handleRemoveInsight, handleEditInsightsAuditsWiki}) {
//   const handleInsightEdit = (index, field, newValue) => {
//     const newInsights = [...insights]
//     newInsights[index][field] = newValue
//     handleEditInsightsAuditsWiki(objIndex, 'Insights', newInsights)
//   }

//   return(
//     <>
//         {insights.map((insight, index) => (
//           <div key={index} className='transctpit-div-child' >
//             <label className='transcript-div-label'>Insight</label>

//             <input
//               className='transctpit-div-grandchild transcript-div-input'
//               type="text"
//               placeholder='Value'
//               value={insight.value}
//               onChange={(e) => handleInsightEdit(index, 'value', e.target.value)}
//               required
//             />

//             <input
//               className='transctpit-div-grandchild transcript-div-input'
//               type="text"
//               placeholder='Type'
//               value={insight.type}
//               onChange={(e) => handleInsightEdit(index, 'type', e.target.value)}
//               required
//             />

//             <input
//               className='transctpit-div-grandchild transcript-div-input'
//               type="text"
//               placeholder='Actor'
//               value={insight.actor}
//               onChange={(e) => handleInsightEdit(index, 'actor', e.target.value)}
//               required
//             />

//             <input
//               className='transctpit-div-grandchild transcript-div-input'
//               type="text"
//               placeholder='Entity'
//               value={insight.entity}
//               onChange={(e) => handleInsightEdit(index, 'entity', e.target.value)}
//               required
//             />

//             <button className='transcript-div-remove' onClick={() => handleRemoveInsight(objIndex, index)}>
//               <CiCircleRemove size={24} />
//             </button>
//           </div>
//         ))}
//     </>
//   )
// }



export function DisplayAudits({ objIndex, audits, handleRemoveAudit, handleEditInsightsAuditsWiki }) {

  const handleAuditEdit = (index, field, newValue) => {
    const newAudits = [...audits]
    newAudits[index][field] = newValue
    handleEditInsightsAuditsWiki(objIndex, 'Audits', newAudits)
  }

  const handleAddQuestion = (index) => {
    const newAudits = [...audits]
    newAudits[index].questions.push({ question: '', auditResponse: '', options: [] })
    handleEditInsightsAuditsWiki(objIndex, 'Audits', newAudits)
  }

  const handleAddOption = (index, quesInd) => {
    const newAudits = [...audits]
    newAudits[index].questions[quesInd].options.push('')
    handleEditInsightsAuditsWiki(objIndex, 'Audits', newAudits)
  }

  const handleEditQuestion = (index, quesInd, type, value) => {
    const newAudits = [...audits]
    newAudits[index].questions[quesInd][type] = value
    handleEditInsightsAuditsWiki(objIndex, 'Audits', newAudits)
  }

  const handleEditOption = (index, quesInd, optInd, value) => {
    const newAudits = [...audits]
    newAudits[index].questions[quesInd].options[optInd] = value
    handleEditInsightsAuditsWiki(objIndex, 'Audits', newAudits)
  }

  const handleRemoveQuestion = (index, quesInd) => {
    const newAudits = [...audits]
    newAudits[index].questions.splice(quesInd, 1)
    handleEditInsightsAuditsWiki(objIndex, 'Audits', newAudits)
  }

  const handleRemoveOption = (index, quesInd, optInd) => {
    const newAudits = [...audits]
    newAudits[index].questions[quesInd].options.splice(optInd, 1)
    handleEditInsightsAuditsWiki(objIndex, 'Audits', newAudits)
  }

  return (
    <>
      {audits.map((audit, index) => (
        <>
          <div key={index} className='transctpit-div-child' >
            <label className='transcript-div-label'>Audit</label>
            <input
              className='transctpit-div-grandchild transcript-div-input'
              type="text"
              placeholder='Section'
              value={audit.section}
              onChange={(e) => handleAuditEdit(index, 'section', e.target.value)}
            />

            <select
              className='transctpit-div-grandchild transcript-div-input'
              onChange={(e) => handleAuditEdit(index, 'auditStatus', e.target.value)}
              defaultValue={audit.auditStatus}
              required
            >
              <option value={'FAIL'}>Fail</option>
              <option value={'PASS'}>Pass</option>
            </select>

            <button
              className='transcript-div-button'
              onClick={() => handleAddQuestion(index)}
            >
              Add Question
            </button>

            <button className='transcript-div-remove' onClick={() => handleRemoveAudit(objIndex, index)}>
              <CiCircleRemove size={24} />
            </button>

          </div>
          <DisplayAuditQuestions auditInd={index} questions={audit.questions} handleEditQuestion={handleEditQuestion} handleEditOption={handleEditOption} handleRemoveQuestion={handleRemoveQuestion} handleRemoveOption={handleRemoveOption} handleAddOption={handleAddOption} />
        </>
      ))}
    </>
  )
}

export function DisplayAuditQuestions({ auditInd, questions, handleEditQuestion, handleEditOption, handleRemoveQuestion, handleRemoveOption, handleAddOption }) {
  return (
    <>
      {questions.map((question, quesIndex) => (
        <>
          <div key={quesIndex} className='transctpit-div-child' >
            <label className='transcript-div-label'>Question</label>

            <input
              className='transctpit-div-grandchild transcript-div-input'
              type="text"
              placeholder='Question'
              value={question.question}
              onChange={(e) => {handleEditQuestion(auditInd, quesIndex, 'question', e.target.value) }}
              required
            />

            <input
              className='transctpit-div-grandchild transcript-div-input'
              type="text"
              placeholder='Audit Response'
              value={question.auditResponse}
              onChange={(e) => {handleEditQuestion(auditInd, quesIndex, 'auditResponse', e.target.value) }}
              required
            />

            <button
              className='transcript-div-button'
              onClick={() => handleAddOption(auditInd, quesIndex)}
            >
              Add Option
            </button>

            <button className='transcript-div-remove' onClick={() => handleRemoveQuestion(auditInd, quesIndex)}>
              <CiCircleRemove size={24} />
            </button>
          </div>

          <DisplayOptions options={question.options} auditInd={auditInd} quesIndex={quesIndex} handleRemoveOption={handleRemoveOption} handleEditOption={handleEditOption} />
        </>
      ))}
    </>
  )
}

export function DisplayOptions({ options, auditInd, quesIndex, handleRemoveOption, handleEditOption }) {
  return (
    <>
      {options.map((option, optInd) => (
        <div key={optInd} className='transctpit-div-child' >
          <label className='transcript-div-label'>Option</label>

          <input
            className='transctpit-div-grandchild transcript-div-input'
            type="text"
            placeholder='Option'
            value={option}
            onChange={(e) => { handleEditOption(auditInd, quesIndex, optInd, e.target.value) }}
            required
          />

          <button className='transcript-div-remove' onClick={() => handleRemoveOption(auditInd, quesIndex, optInd)}>
            <CiCircleRemove size={24} />
          </button>
        </div>
      ))}
    </>
  )
}

export function DisplaySentiment({ objIndex, sentimentScore, handleEdit, handleAdjustSentiment }) {
  const handleSentimentEdit = (field, newValue) => {
    const newSentimentScore = { ...sentimentScore }
    newSentimentScore[field] = newValue
    handleEdit(objIndex, 'SentimentScore', newSentimentScore)
    handleAdjustSentiment(objIndex)
  }

  return (
    <>
      {sentimentScore ?
        <div className='transctpit-div-child'>

          <label className='transcript-div-label'>Sentiment</label>

          <input
            className='transctpit-div-grandchild transcript-div-input'
            type="number"
            step="0.01"
            placeholder='Positive'
            value={sentimentScore.Positive}
            onChange={(e) => handleSentimentEdit('Positive', parseFloat(e.target.value))}
            required
          />

          <input
            className='transctpit-div-grandchild transcript-div-input'
            type="number"
            step="0.01"
            placeholder='Negative'
            value={sentimentScore.Negative}
            onChange={(e) => handleSentimentEdit('Negative', parseFloat(e.target.value))}
            required
          />

          <input
            className='transctpit-div-grandchild transcript-div-input'
            type="number"
            step="0.01"
            placeholder='Neutral'
            value={sentimentScore.Neutral}
            onChange={(e) => handleSentimentEdit('Neutral', parseFloat(e.target.value))}
            required
          />

        </div>
        : null}
    </>
  )
}

export function DisplayAIWiki({ objIndex, wikiChats, handleEditInsightsAuditsWiki, handleRemoveWiki }) {
  const handleWikiEdit = (index, type, newValue) => {
    const newWikiChats = [...wikiChats]
    newWikiChats[index][type] = newValue
    handleEditInsightsAuditsWiki(objIndex, 'AIWikiChat', newWikiChats)
  }

  return (
    <>
      {wikiChats.map((chat, index) => (
        <div key={index} className='transctpit-div-child' >
          <label className='transcript-div-label'>Wiki</label>
          <input
            className='transctpit-div-grandchild transcript-div-input'
            type="text"
            placeholder='User'
            value={chat.user}
            onChange={(e) => handleWikiEdit(index, 'user', e.target.value)}
            required
          />

          <input
            className='transctpit-div-grandchild transcript-div-input'
            type="text"
            placeholder='Utterance'
            value={chat.utterance}
            onChange={(e) => handleWikiEdit(index, 'utterance', e.target.value)}
            required
          />

          <button className='transcript-div-remove' onClick={() => handleRemoveWiki(objIndex, index)}>
            <CiCircleRemove size={24} />
          </button>

        </div>
      ))}
    </>
  )
}

export function DisplayChannelTime({ obj, index, handleEdit, handleEditTime }) {
  return (
    <div className='transcript-div-child'>
      <label className='transcript-div-label'>Channel:</label>
      <input
        className='transcript-div-input'
        type="text"
        placeholder='Channel ID'
        value={obj.eventData.Transcript.ChannelId}
        onChange={(e) => handleEdit(index, 'ChannelId', e.target.value)}
        required
      />

      <label className='transcript-div-label'>Start Time:</label>
      <input
        className='transcript-div-input'
        type="number"
        placeholder='Start Time'
        value={obj.StartTime}
        onChange={(e) => handleEditTime(index, 'StartTime', e.target.value)}
        required
      />

      <label className='transcript-div-label'>End Time:</label>
      <input
        className='transcript-div-input'
        type="number"
        placeholder='End Time'
        value={obj.EndTime}
        onChange={(e) => handleEditTime(index, 'EndTime', e.target.value)}
        required
      />
    </div>
  )
}

export function DisplayWorkflow({ objIndex, guidance, handleEditWorkflow, handleEditWorkflowStep, handleEditKnowledgeArticle, handleRemoveKnowledgeArticle, handleAddWorkflowStep, handleRemoveGuidance, handleRemoveWorkflowStep, handleEditSpeech }) {
  return (
    <>
      {guidance.map((item, index) => (
        item.type === 'KNOWLEDGE_ARTICLE' ?
          <div key={index} className="transctpit-div-child">
            <DisplayKnowledgeArticle objIndex={objIndex} transcript={guidance} index={index} item={item} handleEditKnowledgeArticle={handleEditKnowledgeArticle} handleRemoveKnowledgeArticle={handleRemoveKnowledgeArticle} />

          </div>
          :
          item.type === 'ACTION_WORKFLOW' ? item.cardShown === false ?
            <div key={index} className='transctpit-div-child' >
              <label className='transcript-div-label'>Workflow</label>
              Workflow starts here.
            </div> :
            <>
              <div key={index} className='transctpit-div-child' >
                <label className='transcript-div-label'>Workflow</label>
                <select
                  className='transctpit-div-grandchild transcript-div-input'
                  onChange={(e) => { handleEditWorkflow(objIndex, index, 'workflowType', e.target.value) }}
                  required
                >
                  <option value={'detected'}>Detected</option>
                  <option value={'running'}>Running</option>
                  <option value={'completed'}>Completed</option>
                </select>

                <input
                  className='transctpit-div-grandchild transcript-div-input'
                  type="text"
                  placeholder='Intent Detected'
                  value={item.intent}
                  onChange={(e) => { handleEditWorkflow(objIndex, index, 'intent', e.target.value) }}
                  required
                />

                <input
                  className='transctpit-div-grandchild transcript-div-input'
                  style={{ width: '25%' }}
                  type="text"
                  placeholder='Name of Workflow'
                  value={item.name}
                  onChange={(e) => { handleEditWorkflow(objIndex, index, 'name', e.target.value) }}
                  required
                />

                {item.workflowType === 'running' &&
                  <button
                    className='transcript-div-button'
                    onClick={() => handleAddWorkflowStep(objIndex, index, '', 'InProgress', 'textBox', '')}
                  >
                    Add Step
                  </button>}

                <button className='transcript-div-remove' onClick={() => handleRemoveGuidance(objIndex, index)}>
                  <CiCircleRemove size={24} />
                </button>

              </div>
              {item.workflowType === 'running' && <DisplaySteps objIndex={objIndex} guidInd={index} steps={item.steps} handleEditWorkflowStep={handleEditWorkflowStep} handleRemoveWorkflowStep={handleRemoveWorkflowStep} />}
            </>
            : item.type === 'SPEECH_SUGGESTION' ?
              <DisplaySpeechSuggestion objIndex={objIndex} guidInd={index} item={item} handleEditSpeech={handleEditSpeech} handleRemoveGuidance={handleRemoveGuidance} /> : null
      ))}
      {/* 
        {guidance.map((item, index)=>
        item.type === 'ACTION_WORKFLOW' && item.cardShown === false &&
        <div key={index} className='transctpit-div-child' >
          <label className='transcript-div-label'>Workflow</label>
          <input />
        </div>

        ))} */}
    </>
  )
}

export function DisplayKnowledgeArticle({ objIndex, transcript, index, item, handleEditKnowledgeArticle, handleRemoveKnowledgeArticle }) {

  const handleEditName = (index, nameValue) => {
    const newTranscript = [...transcript]
    newTranscript[index].name = nameValue
    handleEditKnowledgeArticle(objIndex, 'Guidance', newTranscript)
  }

  const handleAddValue = (index) => {
    const newTranscript = [...transcript]
    newTranscript[index].value.push({ title: '', detail: [] })
    handleEditKnowledgeArticle(objIndex, 'Guidance', newTranscript)
  }

  const handleAddDetail = (index, valueInd) => {
    const newTranscript = [...transcript]
    newTranscript[index].value[valueInd].detail.push('')
    handleEditKnowledgeArticle(objIndex, 'Guidance', newTranscript)
  }

  const handleEditValue = (index, valueInd, type, value) => {
    const newTranscript = [...transcript]
    newTranscript[index].value[valueInd][type] = value
    handleEditKnowledgeArticle(objIndex, 'Guidance', newTranscript)
  }

  const handleEditDetail = (index, valueInd, detailInd, value) => {
    const newTranscript = [...transcript]
    newTranscript[index].value[valueInd].detail[detailInd] = value
    handleEditKnowledgeArticle(objIndex, 'Guidance', newTranscript)
  }

  const handleRemoveValue = (index, valueInd) => {
    const newTranscript = [...transcript]
    newTranscript[index].value.splice(valueInd, 1)
    handleEditKnowledgeArticle(objIndex, 'Guidance', newTranscript)
  }

  const handleRemoveDetail = (index, valueInd, detailInd) => {
    const newTranscript = [...transcript]
    newTranscript[index].value[valueInd].detail.splice(detailInd, 1)
    handleEditKnowledgeArticle(objIndex, 'Guidance', newTranscript)
  }


  return (
    <div>
      <>
        <label className="transcript-div-label">Knowledge Article</label>
        <input
          className='transctpit-div-grandchild transcript-div-input'
          type="text"
          placeholder='Name'
          value={item.name}
          onChange={(e) => { handleEditName(index, e.target.value) }}
          required
        />
        <button
          className='transcript-div-button'
          onClick={(e) => handleAddValue(index)}
        >
          Add Value
        </button>

        <button className='transcript-div-remove' onClick={() => handleRemoveKnowledgeArticle(objIndex, index)}>
          <CiCircleRemove size={24} />
        </button>
      </>
      <DisplayValue objIndex={objIndex} guidInd={index} value={item.value} handleEditValue={handleEditValue} handleRemoveValue={handleRemoveValue} handleAddDetail={handleAddDetail} handleEditDetail={handleEditDetail} handleRemoveDetail={handleRemoveDetail} />
    </div>
  )
}

export function DisplayValue({objIndex, guidInd, value, handleEditValue, handleRemoveValue, handleAddDetail, handleEditDetail, handleRemoveDetail }) {
  return (
    <>
      {value.map((item, index) => (
        <>
          <div key={index} className='transctpit-div-child' >
            <label className='transcript-div-label'>Value</label>
            <input
              className='transctpit-div-grandchild transcript-div-input'
              type="text"
              placeholder='title'
              value={item.title}
              onChange={(e) => { handleEditValue(guidInd, index, 'title', e.target.value) }}
              required
            />

            <button
              className='transcript-div-button'
              onClick={(e) => handleAddDetail(guidInd, index)}
            >
              Add Detail
            </button>

            <button className='transcript-div-remove' onClick={() => handleRemoveValue(guidInd, index)}>
              <CiCircleRemove size={24} />
            </button>
          </div>
          <DisplayDetail guidInd={guidInd} valueInd={index} detail={item.detail} handleEditDetail={handleEditDetail} handleRemoveDetail={handleRemoveDetail} />
        </>
      ))}
    </>
  )
}

export function DisplayDetail({ guidInd, valueInd, detail, handleEditDetail, handleRemoveDetail }) {
  return (
    <>
      {detail.map((item, index) => (
        <div key={index} className='transctpit-div-child' >
          <label className='transcript-div-label'>Detail</label>
          <input
            className='transctpit-div-grandchild transcript-div-input'
            type="text"
            placeholder='detail'
            value={item.detail}
            onChange={(e) => { handleEditDetail(guidInd, valueInd, index, e.target.value) }}
            required
          />

          <button className='transcript-div-remove' onClick={() => handleRemoveDetail(guidInd, valueInd, index)}>
            <CiCircleRemove size={24} />
          </button>
        </div>

      ))}
    </>
  )
}


export function DisplaySteps({ objIndex, guidInd, steps, handleEditWorkflowStep, handleRemoveWorkflowStep }) {
  return (
    <>
      {steps.map((item, index) => (
        <div key={index} className='transctpit-div-child' >
          <label className='transcript-div-label'>Step</label>
          <input
            className='transctpit-div-grandchild transcript-div-input'
            type="text"
            placeholder='Step Name'
            value={item.stepName}
            onChange={(e) => { handleEditWorkflowStep(objIndex, guidInd, index, 'stepName', e.target.value) }}
            required
          />

          <select
            className='transctpit-div-grandchild transcript-div-input'
            onChange={(e) => { handleEditWorkflowStep(objIndex, guidInd, index, 'state', e.target.value) }}
            defaultValue={item.card.type}
            required
          >
            <option value={'InProgress'}>In Progress</option>
            <option value={'Completed'}>Completed</option>
            <option value={'None'}>None</option>
          </select>

          <select
            className='transctpit-div-grandchild transcript-div-input'
            onChange={(e) => { handleEditWorkflowStep(objIndex, guidInd, index, 'type', e.target.value) }}
            defaultValue={item.card.type}
            required
          >
            <option value={'textBox'}>Text Box</option>
          </select>

          <input
            className='transctpit-div-grandchild transcript-div-input'
            type="text"
            placeholder='Text'
            value={item.card.messages[0].text}
            onChange={(e) => { handleEditWorkflowStep(objIndex, guidInd, index, 'text', e.target.value) }}
            required
          />

          <button className='transcript-div-remove' onClick={() => handleRemoveWorkflowStep(objIndex, guidInd, index)}>
            <CiCircleRemove size={24} />
          </button>

        </div>
      ))}
    </>
  )
}

export function DisplaySpeechSuggestion({ objIndex, guidInd, item, handleEditSpeech, handleRemoveGuidance }) {
  return (
    <div className='transctpit-div-child' >
      <label className='transcript-div-label'>Speech</label>

      <input
        className='transctpit-div-grandchild transcript-div-input'
        style={{ width: '100%' }}
        type="text"
        placeholder='Speech Suggestion'
        value={item.value}
        onChange={(e) => { handleEditSpeech(objIndex, guidInd, e.target.value) }}
        required
      />

      <button className='transcript-div-remove' onClick={() => handleRemoveGuidance(objIndex, guidInd)}>
        <CiCircleRemove size={24} />
      </button>
    </div>
  )
}
