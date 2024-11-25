export function DisplaySentiment({ nudge,objIndex, sentimentScore, handleEdit, handleAdjustSentiment }) {
  const handleSentimentEdit = (field, newValue) => {
    const newSentimentScore = { ...sentimentScore }
    newSentimentScore[field] = newValue
    handleEdit(objIndex, 'SentimentScore', newSentimentScore)
    handleAdjustSentiment(objIndex)
  }

  return (
    <>
      {sentimentScore && !(nudge=="aiNudgeTab") ?
        <div className='transcript-div-child-semtiment'>

          <label className='transcript-div-label'>Sentiment :</label>

          <div className='sentiment-input-wrapper positive-sentiment'>
            <BsEmojiSmileFill className='sentiment-emoji positive-sentiment'/>
            <input
              className='transcript-div-grandchild transcript-div-input-sentiment'
              type="number"
              step="0.01"
              placeholder='Positive'
              value={sentimentScore.Positive}
              onChange={(e) => handleSentimentEdit('Positive', parseFloat(e.target.value))}
              required
            />
          </div>

          <div className='sentiment-input-wrapper negative-sentiment'>
            <BsEmojiFrownFill className='sentiment-emoji'/>
            <input
              className='transcript-div-grandchild transcript-div-input-sentiment'
              type="number"
              step="0.01"
              placeholder='Negative'
              value={sentimentScore.Negative}
              onChange={(e) => handleSentimentEdit('Negative', parseFloat(e.target.value))}
              required
            />
          </div>

          <div className='sentiment-input-wrapper neutral-sentiment'>
            <BsEmojiNeutralFill className='sentiment-emoji'/>
            <input
              className='transcript-div-grandchild transcript-div-input-sentiment'
              type="number"
              step="0.01"
              placeholder='Neutral'
              value={sentimentScore.Neutral}
              onChange={(e) => handleSentimentEdit('Neutral', parseFloat(e.target.value))}
              required
            />
          </div>

        </div>
        : null}
    </>
  )
}
