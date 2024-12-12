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
        <div className='transcript-div-child'>

          <label className='transcript-div-label'>Sentiment</label>

          <input
            className='transcript-div-grandchild transcript-div-input'
            type="number"
            step="0.01"
            placeholder='Positive'
            value={sentimentScore.Positive}
            onChange={(e) => handleSentimentEdit('Positive', parseFloat(e.target.value))}
            required
          />

          <input
            className='transcript-div-grandchild transcript-div-input'
            type="number"
            step="0.01"
            placeholder='Negative'
            value={sentimentScore.Negative}
            onChange={(e) => handleSentimentEdit('Negative', parseFloat(e.target.value))}
            required
          />

          <input
            className='transcript-div-grandchild transcript-div-input'
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
