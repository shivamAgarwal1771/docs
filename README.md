/* General styles for the transcript container */
.transcript-div {
  display: flex;
  flex-direction: column;
  border: 1px solid #ddd;
  border-radius: 5px;
  padding: 16px;
  margin-bottom: 16px;
  background-color: #fff;
  box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.transcript-div-child {
  margin-bottom: 12px;
}

/* Input styles */
.transcript-div-recommendation-input {
  width: 100%;
  padding: 8px;
  margin-bottom: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 14px;
}

.transcript-div-textarea {
  width: 100%;
  min-height: 100px;
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  font-size: 14px;
  resize: vertical;
}

/* Button styles */
.transcript-div-button {
  padding: 8px 16px;
  margin-right: 8px;
  margin-top: 8px;
  background-color: #007bff;
  color: #fff;
  border: none;
  border-radius: 4px;
  font-size: 14px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.transcript-div-button:hover {
  background-color: #0056b3;
}

.customize-btns {
  display: flex;
  flex-wrap: wrap;
  gap: 8px;
}

/* Slider arrow styles */
.slick-prev, .slick-next {
  z-index: 1;
  color: #000;
  font-size: 24px;
}

/* Headings and other text elements */
h2, h3 {
  margin: 16px 0;
  font-weight: 600;
  color: #333;
}

/* Additional context and guidance styles */
.context-item, .guidance-item {
  padding: 8px;
  border: 1px solid #ccc;
  border-radius: 4px;
  margin-bottom: 8px;
  background-color: #f9f9f9;
}

.context-item input,
.guidance-item input {
  width: calc(100% - 16px);
  padding: 4px;
  border: 1px solid #ddd;
  border-radius: 4px;
}

/* Styles for section headers */
.section-header {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 8px;
  color: #444;
}

/* Improved spacing for form elements */
.form-group {
  margin-bottom: 16px;
}

/* Better display for sentiment and other elements */
.sentiment-score {
  display: flex;
  gap: 12px;
}

.sentiment-score span {
  font-size: 14px;
  color: #555;
}
