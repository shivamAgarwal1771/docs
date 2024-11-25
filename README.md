import { BsEmojiSmileFill, BsEmojiFrownFill, BsEmojiNeutralFill } from 'react-icons/bs';

export function DisplaySentiment({ nudge, objIndex, sentimentScore, handleEdit, handleAdjustSentiment }) {
  const handleSentimentEdit = (field, newValue) => {
    const newSentimentScore = { ...sentimentScore };
    newSentimentScore[field] = newValue;
    handleEdit(objIndex, 'SentimentScore', newSentimentScore);
    handleAdjustSentiment(objIndex);
  };

  return (
    <>
      {sentimentScore && !(nudge === "aiNudgeTab") ? (
        <div className='transcript-div-child-semtiment'>
          <label className='transcript-div-label'>Sentiment :</label>

          <div className='sentiment-radio-wrapper'>
            {/* Positive Sentiment */}
            <label className='sentiment-radio-option'>
              <BsEmojiSmileFill className='sentiment-emoji positive-sentiment' />
              <input
                type="radio"
                name="sentiment"
                value="Positive"
                checked={sentimentScore.Positive === 0.08}
                onChange={() => handleSentimentEdit('Positive', 0.08)}
              />
              Positive
            </label>

            {/* Negative Sentiment */}
            <label className='sentiment-radio-option'>
              <BsEmojiFrownFill className='sentiment-emoji' />
              <input
                type="radio"
                name="sentiment"
                value="Negative"
                checked={sentimentScore.Negative === 0.08}
                onChange={() => handleSentimentEdit('Negative', 0.08)}
              />
              Negative
            </label>

            {/* Neutral Sentiment */}
            <label className='sentiment-radio-option'>
              <BsEmojiNeutralFill className='sentiment-emoji neutral-sentiment' />
              <input
                type="radio"
                name="sentiment"
                value="Neutral"
                checked={sentimentScore.Neutral === 0.08}
                onChange={() => handleSentimentEdit('Neutral', 0.08)}
              />
              Neutral
            </label>
          </div>

          {/* Set the unselected sentiments to 0.01 */}
          <div className="sentiment-hidden">
            {sentimentScore.Positive !== 0.08 && handleSentimentEdit('Positive', 0.01)}
            {sentimentScore.Negative !== 0.08 && handleSentimentEdit('Negative', 0.01)}
            {sentimentScore.Neutral !== 0.08 && handleSentimentEdit('Neutral', 0.01)}
          </div>
        </div>
      ) : null}
    </>
  );
}
