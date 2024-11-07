@import url("https://fonts.googleapis.com/css2?family=Open+Sans:ital,wght@0,300..800;1,300..800&display=swap");

* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  font-family: 'Open Sans', sans-serif;
}

.container {
  position: relative;
  left: 5vw;
  width: 90vw;
  padding-top: 50px;
}

.body-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 20px;
}

.header {
  position: fixed;
  width: 95%;
  left: 2.5vw;
  display: flex;
  align-items: center;
  justify-content: space-between;
  background-color: #ffffff;
  z-index: 1;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
  padding: 1rem 2rem;
}

.demo-header-btns {
  display: flex;
  gap: 15px;
}

.demo-header-button {
  padding: 8px 16px;
  border: 1px solid #ddd;
  border-radius: 5px;
  font-weight: 500;
  font-size: 1rem;
  background-color: #f1f1f1;
  transition: background-color 0.3s, color 0.3s;
}

.demo-header-button:hover {
  background-color: #5095f8;
  color: #fff;
}

.section-container {
  display: flex;
  flex-direction: column;
  background-color: #f8f8f8;
  padding: 30px;
  border-radius: 12px;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
  margin-top: 60px;
}

textarea {
  width: 100%;
  height: 100px;
  border: 1px solid #ddd;
  border-radius: 10px;
  font-size: 1rem;
  padding: 10px;
  resize: none;
}

textarea:focus {
  outline: none;
  border-color: #5095f8;
}

.btn-convert,
.submit-btn {
  background-color: #5095f8;
  color: #fff;
  padding: 10px 20px;
  border-radius: 5px;
  font-size: 1rem;
  cursor: pointer;
  transition: background-color 0.3s;
}

.btn-convert:hover,
.submit-btn:hover {
  background-color: #3b7fd6;
}

.main-body-container {
  display: flex;
  justify-content: space-around;
  align-items: center;
  margin-top: 1rem;
}

.speaker-selection {
  padding: 5px 10px;
  border-radius: 5px;
  background-color: #f0f0f0;
  color: #333;
  cursor: pointer;
  transition: background-color 0.2s;
}

.speaker-selection:hover {
  background-color: #5095f8;
  color: #fff;
}

.audio-button {
  font-size: 2.5rem;
  border: none;
  background-color: transparent;
  cursor: pointer;
  transition: color 0.3s;
}

.audio-button:hover {
  color: #5095f8;
}

.progress-container {
  width: 100%;
  background-color: #e0e0e0;
  height: 8px;
  border-radius: 4px;
}

.progress-bar {
  background-color: #5095f8;
  height: 100%;
  border-radius: 4px;
}

.tab {
  padding: 8px 12px;
  border-radius: 5px;
  font-size: 1rem;
  transition: background-color 0.3s, color 0.3s;
}

.tab:hover {
  background-color: #5095f8;
  color: #fff;
}

.tab.active {
  background-color: #007bff;
  color: #fff;
}

.add-item-btn,
.remove-item-btn,
.demo-btn,
.section-btn {
  background-color: #fff;
  color: #333;
  padding: 10px 15px;
  border: 1px solid #ddd;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s, color 0.3s;
}

.add-item-btn:hover,
.remove-item-btn:hover,
.demo-btn:hover,
.section-btn:hover {
  background-color: #5095f8;
  color: #fff;
}

.success-btn {
  background-color: lightgreen;
  color: green;
  padding: 8px 12px;
  border-radius: 5px;
  display: flex;
  align-items: center;
  gap: 6px;
}

.file-notification {
  color: red;
  font-weight: bold;
}

.transcript-json-div {
  position: fixed;
  top: 15vh;
  right: 2.5vw;
  width: 300px;
  height: 70vh;
  border-radius: 8px;
  background-color: #5095f8;
  box-shadow: 0 0 8px rgba(0, 0, 0, 0.2);
  padding: 15px;
  color: white;
  transition: width 0.3s;
}

.transcript-json-div:hover {
  width: 350px;
}

.transcript-preview {
  height: 85vh;
  padding: 15px;
  background-color: #f9f9f9;
  box-shadow: 0 0 5px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
  overflow-y: auto;
}

.transcript-download-btn {
  background-color: #28a745;
  color: #fff;
  padding: 12px 20px;
  font-size: 1rem;
  border: none;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s;
}

.transcript-download-btn:hover {
  background-color: #218838;
}

.transcript-preview-div.selected {
  background-color: rgba(100, 100, 255, 0.1);
  border-left: 4px solid #5095f8;
}

.upload-demo-container {
  position: relative;
  width: 100%;
  overflow-y: auto;
  scrollbar-width: none;
}

.error-btn {
  background-color: #ffdddd;
  color: #a00;
  border: 1px solid #a00;
  padding: 10px;
  border-radius: 5px;
}

.transcript-div-button {
  padding: 8px 12px;
  font-size: 1rem;
  border: 1px solid #ddd;
  border-radius: 5px;
  background-color: #f0f0f0;
  color: #333;
  transition: background-color 0.3s, color 0.3s;
}

.transcript-div-button:hover {
  background-color: #5095f8;
  color: #fff;
}

.transcript-div-remove {
  color: #a00;
  cursor: pointer;
  font-weight: bold;
  border: none;
  background-color: transparent;
}

.transcript-div-remove:hover {
  color: #c00;
}

.input,
.transcript-div-input,
.transcript-div-recommendation-input {
  padding: 10px;
  font-size: 1rem;
  border: 1px solid #ddd;
  border-radius: 5px;
  width: 100%;
}

.transcript-div-textarea {
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  resize: vertical;
  font-size: 1rem;
}
