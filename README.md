export function DisplaySentiment({ objIndex, sentimentScore, handleEdit, handleAdjustSentiment }) {
  const handleSentimentEdit = (field, newValue) => {
    const newSentimentScore = { ...sentimentScore };
    newSentimentScore[field] = newValue;
    handleEdit(objIndex, 'SentimentScore', newSentimentScore);
    handleAdjustSentiment(objIndex);
  };

  const handleRadioChange = (selectedField) => {
    const newSentimentScore = {
      Positive: 0.1,
      Negative: 0.1,
      Neutral: 0.1,
      [selectedField]: 0.8, // Set the selected field to 0.8
    };
    handleEdit(objIndex, 'SentimentScore', newSentimentScore);
    handleAdjustSentiment(objIndex);
  };

  return (
    <>
      {sentimentScore ? (
        <div className='transcript-div-child'>
          <label className='transcript-div-label'>Sentiment</label>

          <div className="radio-group">
            <label>
              <input
                type="radio"
                name={`sentiment-${objIndex}`}
                checked={sentimentScore.Positive === 0.8}
                onChange={() => handleRadioChange('Positive')}
              />
              Positive
            </label>

            <label>
              <input
                type="radio"
                name={`sentiment-${objIndex}`}
                checked={sentimentScore.Negative === 0.8}
                onChange={() => handleRadioChange('Negative')}
              />
              Negative
            </label>

            <label>
              <input
                type="radio"
                name={`sentiment-${objIndex}`}
                checked={sentimentScore.Neutral === 0.8}
                onChange={() => handleRadioChange('Neutral')}
              />
              Neutral
            </label>
          </div>
        </div>
      ) : null}
    </>
  );
}
