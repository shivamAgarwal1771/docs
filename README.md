/* General container styling */
.transcript-div {
  display: flex;
  flex-direction: column;
  border: 1px solid #e0e0e0;
  border-radius: 8px;
  padding: 20px;
  margin-bottom: 20px;
  background-color: #fafafa;
  box-shadow: 0 3px 6px rgba(0, 0, 0, 0.1);
}

/* Child element styling for proper spacing */
.transcript-div-child {
  margin-bottom: 16px;
}

/* Input field styling */
.transcript-div-recommendation-input {
  width: 100%;
  padding: 10px;
  border: 1px solid #bdbdbd;
  border-radius: 6px;
  font-size: 15px;
  margin-bottom: 12px;
  box-sizing: border-box;
}

.transcript-div-textarea {
  width: 100%;
  min-height: 120px;
  padding: 10px;
  border: 1px solid #bdbdbd;
  border-radius: 6px;
  font-size: 15px;
  resize: vertical;
  box-sizing: border-box;
}

/* Button styling with defined colors */
.transcript-div-button {
  padding: 10px 20px;
  margin-top: 10px;
  background-color: #28a745;
  color: #ffffff;
  border: none;
  border-radius: 6px;
  font-size: 15px;
  cursor: pointer;
  transition: background-color 0.3s ease;
}

.transcript-div-button:hover {
  background-color: #218838;
}

/* Button group alignment */
.customize-btns {
  display: flex;
  flex-wrap: wrap;
  gap: 12px;
  justify-content: flex-start;
  align-items: center;
}

/* Slider arrow styling for better visibility */
.slick-prev,
.slick-next {
  z-index: 1;
  color: #6c757d;
  font-size: 24px;
}

/* Heading styling for a polished look */
h2, h3 {
  margin: 20px 0 12px;
  font-weight: 700;
  color: #343a40;
}

/* Additional styles for context and guidance items */
.context-item,
.guidance-item {
  display: flex;
  align-items: center;
  padding: 10px;
  border: 1px solid #e0e0e0;
  border-radius: 6px;
  margin-bottom: 10px;
  background-color: #ffffff;
}

.context-item input,
.guidance-item input {
  width: calc(100% - 20px);
  padding: 6px;
  border: 1px solid #ced4da;
  border-radius: 4px;
  font-size: 14px;
}

/* Section header styling */
.section-header {
  font-size: 16px;
  font-weight: bold;
  margin-bottom: 10px;
  color: #495057;
}

/* Improved form group spacing */
.form-group {
  margin-bottom: 18px;
  display: flex;
  flex-direction: column;
}

/* Sentiment score display styling */
.sentiment-score {
  display: flex;
  align-items: center;
  gap: 14px;
}

.sentiment-score span {
  font-size: 14px;
  color: #6c757d;
}
