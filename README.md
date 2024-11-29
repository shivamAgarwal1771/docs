import { BsEmojiSmileFill, BsEmojiFrownFill, BsEmojiNeutralFill } from 'react-icons/bs';

export function DisplaySentiment({ nudge, objIndex, sentimentScore, handleEdit, handleAdjustSentiment }) {

  // Function to handle sentiment score updates based on selected radio button
  const handleSentimentEdit = (field, newValue) => {
    const newSentimentScore = { ...sentimentScore };
    newSentimentScore[field] = newValue;
    handleEdit(objIndex, 'SentimentScore', newSentimentScore);
    handleAdjustSentiment(objIndex);
  };

  // Handle the radio button change to set values correctly
  const handleRadioChange = (selectedSentiment) => {
    // Initialize all to 0.1
    const newSentimentScore = {
      Positive: 0.1,
      Neutral: 0.1,
      Negative: 0.1
    };

    // Set the selected sentiment to 0.8
    newSentimentScore[selectedSentiment] = 0.8;

    // Update the sentiment score based on the selected sentiment
    handleEdit(objIndex, 'SentimentScore', newSentimentScore);
    handleAdjustSentiment(objIndex);
  };

  return (
    <>
      {sentimentScore && !(nudge === "aiNudgeTab") ?
        <div className='transcript-div-child-semtiment'>

          <label className='transcript-div-label'>Sentiment:</label>

          {/* Positive Sentiment */}
          <div className='sentiment-input-wrapper positive-sentiment'>
            <BsEmojiSmileFill className='sentiment-emoji positive-sentiment' />
            <input
              type="radio"
              name="sentiment"
              value="Positive"
              checked={sentimentScore.Positive === 0.8}
              onChange={() => handleRadioChange('Positive')}
            />
            <input
              className='transcript-div-grandchild transcript-div-input-sentiment'
              type="number"
              step="0.01"
              placeholder='Positive'
              value={sentimentScore.Positive}
              readOnly
            />
          </div>

          {/* Negative Sentiment */}
          <div className='sentiment-input-wrapper negative-sentiment'>
            <BsEmojiFrownFill className='sentiment-emoji' />
            <input
              type="radio"
              name="sentiment"
              value="Negative"
              checked={sentimentScore.Negative === 0.8}
              onChange={() => handleRadioChange('Negative')}
            />
            <input
              className='transcript-div-grandchild transcript-div-input-sentiment'
              type="number"
              step="0.01"
              placeholder='Negative'
              value={sentimentScore.Negative}
              readOnly
            />
          </div>

          {/* Neutral Sentiment */}
          <div className='sentiment-input-wrapper neutral-sentiment'>
            <BsEmojiNeutralFill className='sentiment-emoji' />
            <input
              type="radio"
              name="sentiment"
              value="Neutral"
              checked={sentimentScore.Neutral === 0.8}
              onChange={() => handleRadioChange('Neutral')}
            />
            <input
              className='transcript-div-grandchild transcript-div-input-sentiment'
              type="number"
              step="0.01"
              placeholder='Neutral'
              value={sentimentScore.Neutral}
              readOnly
            />
          </div>

        </div>
        : null}
    </>
  );
}
