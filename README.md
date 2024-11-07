@import url("https://fonts.googleapis.com/css2?family=Open+Sans:ital,wght@0,300..800;1,300..800&display=swap");

* {
  margin: 0;
  padding: 0;
}

.container {
  left: 5vw;
  width: 90vw;
  /* padding-top: 50px; */
}

.body-container {
  display: flex;
  flex-direction: column;
  justify-items: center;
  align-items: center;
}

.header {
  position: fixed;
  width: 95%;
  left: 3vw;
  display: flex;
  align-items: center;
  justify-content: center;
  background-color: #f9f9f9;
  z-index: 1;
  margin: 0rem 1rem;
}

.demo-header-btns {
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 20px;
}

.demo-header-button {
  border: 10px;
  border-color: black;
  border-radius: 5px;
  padding: 5px;
  font-weight: 450;
  font-size: large;
}

.demo-header-button:hover {
  background-color: lightgrey;
}

.audio-player_container {
  z-index: 1;
}

.dropdown-button {
  float: none;
  color: black;
  padding: 12px 16px;
  text-decoration: none;
  display: block;
  text-align: center;
  font-size: small;
}

.dropdown-button:hover {
  background-color: #ddd;
}

.section-container {
  display: flex;
  flex-direction: column;
  height: 45vh;
  /* margin-top: 40px; */
  background-color: #fff;
  padding: 30px;
  border-radius: 20px;
}

.dropdown {
  padding: 5px;
  margin: 5px;
}

.dropdown-div {
  float: left;
  overflow: hidden;
}

.dropdown-div:hover {
  background-color: transparent;
}

.dropdown-div:hover .dropdown-content {
  display: block;
  margin-top: -6px;
  margin-left: 92px;
}

.dropdown-content {
  display: none;
  position: absolute;
  background-color: #f9f9f9;
  min-width: max-content;
  z-index: 1;
  box-shadow: 0 0 2px rgb(185, 185, 185);
}

.dropdown-content a {
  float: none;
  color: black;
  padding: 12px 16px;
  text-decoration: none;
  display: block;
  text-align: left;
  font-size: small;
}

.dropdown-content a:hover {
  background-color: #ddd;
}

.dropdown-div:hover .dropdown-icon {
  rotate: 180deg;
  transition: 500ms;
}

.personal-info-field-value {
  display: flex;
  flex-direction: column;
}

.personal-info-field-value-child {
  display: flex;
  flex-direction: row;
  align-items: center;
}

.customer-info-items {
  display: flex;
  flex-direction: row;
  align-items: flex-start;
}

textarea {
  width: 95%;
  height: 60%;
  border: 1px solid #ced4da;
  border-radius: 10px;
  font-family: Inter;
  font-size: 1.1rem;
  resize: none;
}

textarea:focus {
  outline: none;
}

.btn-convert {
  display: flex;
  background-color: #5095f8;
  border: 1px solid #5095f8;
  color: #fff;
  padding: 10px;
  margin-top: 10px;
  width: 160px;
  border-radius: 10px;
  font-size: 1rem;
  cursor: pointer;
}

.submit-btn {
  background-color: #5095f8;
  border: 1px solid #5095f8;
  color: #fff;
  padding: 12px;
  margin-top: 3px;
  width: 130px;
  border-radius: 10px;
  font-size: 1.25rem;
  cursor: pointer;
}

.main-body-container {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  /* margin-left: 300px; */
  align-items: center;
}

.play-download-button {
  display: flex;
  flex-direction: row;
}

.upload-body-container {
  display: flex;
  flex-direction: column;
  /* margin-top: 10vw; */
  margin-left: 270px;
  justify-items: center;
  align-items: center;
}

.speaker-selection-container1 {
  display: flex;
  flex-direction: row;
  align-items: center;
}

.speaker-selection {
  display: flex;
  flex-direction: row;
  background-color: transparent;
  color: black;
  font-size: medium;
  border-radius: 5px;
  padding: 5px;
  margin: 5px;
  cursor: pointer;
  transition: 150ms;
}

.upload-body-child {
  display: inline-block;
  min-width: 40vw;
  margin-top: 5vw;
}

.uploading-files-container {
  display: flex;
  flex-direction: column;
  margin-top: 10rem;
  gap: 0.5rem;
}

.file-notification {
  color: red;
}

.file-upload {
  visibility: hidden;
}

.uploaded-file-details {
  margin-right: 300px;
  width: auto;
  color: #5095f8;
}

.audio-container {
  position: fixed;
  bottom: 0;
  width: 100%;
  background-color: #ffffff;
  height: 100px;
  display: flex;
  align-items: center;
  flex-direction: column;
}

.play-download-button {
  display: flex;
  flex-direction: row;
}

.play {
  margin-right: 50px;
}

.download {
  margin-left: 50px;
}

.audio-button {
  font-size: 4rem;
  border: transparent;
  background-color: transparent;
  cursor: pointer;
}

.audio-button:disabled {
  opacity: 0.5;
  cursor: context-menu;
}

.icon-btn {
  color: #5095f8;
}

.progress-container {
  width: 100%;
  background-color: #ccc;
  height: 8px;
  margin: 0 0 10px;
}

.progress-bar {
  background-color: #007bff;
  height: 100%;
  width: 0;
}

.main-body {
  display: flex;
  flex-direction: row;
  justify-content: space-around;
  align-items: baseline;
}

.main-body-child {
  display: inline-block;
  min-width: 40vw;
}

.text-box {
  width: 60vw;
}

.status-component {
  margin: 20px;
}

.status-completed {
  font-weight: bold;
  color: black;
  gap: 4px;
}

.status-in_progress {
  display: flexbox;
  flex-direction: row;
  font-weight: normal;
  color: gray;
  gap: 4px;
}

.status-child {
  display: inline-block;
}

.success {
  width: fit-content;
  display: flex;
  flex-direction: row;
  align-items: center;
  gap: 6px;
  background-color: lightgreen;
  color: rgb(65, 169, 65);
  padding: 10px;
  margin: 10px;
  border: 0px solid rgb(58, 167, 58);
  border-radius: 5px;
}

.input {
  width: 25vw;
  padding: 10px;
  margin: 5px;
  border: 1px solid black;
  background-color: transparent;
}

.active-page {
  font-weight: bold;
}

.tab {
  background-color: transparent;
  color: black;
  font-size: large;
  text-decoration: none;
  border: 0;
  padding: 5px;
  border-radius: 5px;
  margin: 5px;
  transition: 150ms;
}

.tab:hover {
  background-color: #eeeeee;
  cursor: pointer;
}

.tab:active {
  background-color: #ebebeb;
}

.btn {
  font-size: medium;
  text-decoration: underline;
}

.btn-active {
  border: 1px solid black;
  border-radius: 5px;
}

.login-msg {
  color: red;
  margin: 10px;
}

.upload-btn {
  /* margin-top: 30vh; */
}

.transcript-container {
  /* position: relative; */
  /* top: 15vh; */
  /* left: 6.7vw; */
  width: 100%
}

.transcript-body {
  width: 60vw;
}

.transcript-json-div {
  position: fixed;
  top: 15vh;
  left: 70vw;
  height: 70vh;
  width: 30vw;
  display: flex;
  align-items: center;
  border-radius: 10px 0 0 10px;
  box-shadow: 0 0 5px black;
  gap: 0;
  transition: 400ms;
  background-color: #5095f8;
  z-index: 2;
}

.transcript-json-div:hover {
  cursor: pointer;
}

.json-collapse {
  position: fixed;
  left: 97vw;
  height: 70vh;
  width: 3vw;
  box-shadow: 0 0 5px black;
  display: flex;
  align-items: center;
  border-radius: 10px 0 0 10px;
  z-index: 1;
}

.expand-btn {
  width: fit-content;
  padding: 5px;
  border: 0;
  color: white;
  transition: 500ms;
}

.rotate {
  rotate: 180deg;
}

.transcript-json {
  background-color: white;
  color: blue;
  height: 70vh;
  overflow-x: scroll;
}

.scrollbar-none {
  scrollbar-width: none;
}

.transcript-preview {
  height: 83vh;
  width: 25vw;
  border: 0;
  box-shadow: 0 0 2px black;
  display: flex;
  flex-direction: column;
  justify-content: space-between;
  gap: 5px;
  margin-left: 5vw;
  overflow-y: scroll;
  scrollbar-width: none;
  position: relative;
}

.transcript-download-btn {
  background-color: rgb(39, 133, 39);
  color: white;
  /* padding: 10px; */
  border: 0;
  position: fixed;
  bottom: 0;
  width: 12.5vw;
  height: 7vh;
  box-shadow: 0 0 2px black;
}

.transcript-download-btn:hover {
  cursor: pointer;
  background-color: rgb(48, 159, 48);
}

.back-btn-right-align {
  right: 3.25vw;
}

.transcript-preview-list {
  display: flex;
  flex-direction: column;
}

.transcript-preview-div {
  display: flex;
  flex-direction: column;
  box-shadow: 0 0 1px black;
}

.transcript-preview-div.selected {
  background-color: rgba(164, 164, 164, 0.562);
}

.transcript-preview-child {
  display: flex;
  align-items: center;
  gap: 5px;
  border: 0;
  padding: 10px 2px 10px 10px;
}

.transcript-preview-child-insight {
  display: flex;
  flex-direction: column;
  align-items: baseline;
  border: 0;
  padding: 10px 2px 10px 10px;
}

.transcript-preview-child-type {
  font-weight: bold;
  width: 8%;
  align-content: center;
}

.transcript-preview-child-type:hover {
  cursor: pointer;
}

.transcript-preview-child-text {
  width: 62%;
  font-size: small;
}

.transcript-preview-child-text:hover {
  cursor: pointer;
}

.transcript-preview-child-times {
  display: flex;
  flex-direction: row;
  justify-content: space-between;
  color: blue;
  margin: 5px;
  font-size: small;
}

.transcript-preview-child-btn {
  width: 12%;
  background-color: transparent;
  border: 0;
  transition: 500ms;
}

.transcript-preview-child-btn:hover {
  cursor: pointer;
}

.btn-expand {
  rotate: 45deg;
}

.transcript-buttons-div {
  display: flex;
  justify-content: space-evenly;
  overflow: hidden;
  transition: 250ms;
  height: 5vh;
}

.div-collapse {
  height: 0;
}

.transcript-btn {
  background-color: black;
  color: white;
  padding: 5px;
  border: 0;
  border-radius: 5px;
}

.transcript-btn:hover {
  cursor: pointer;
}

.transcript-div {
  height: 80vh;
  display: flex;
  flex-direction: column;
  overflow-y: scroll;
  scrollbar-width: none;
}

.transcript-div-recommendation {
  display: flex;
  flex-direction: column;
  align-items: baseline;
  width: 90%;
  font-size: medium;
}

.transcript-div-recommendation-input {
  width: 25%;
  font-size: large;
  padding: 10px;
  margin: 5px;
  border: 1px solid black;
  background-color: transparent;
}

.transcript-div-textarea {
  width: 95%;
  height: 100px;
  padding: 10px;
  margin: 10px;
  border-radius: 0;
  border: 1px solid black;
  background-color: transparent;
  scroll-behavior: smooth;
  scrollbar-width: none;
}

.transcript-div-input {
  width: 7.5vw;
  padding: 10px;
  margin: 5px;
  border: 1px solid black;
  background-color: transparent;
}

.transcript-div-grandchild1 {
  width: 150px;
}

.customize-btns {
  display: flex;
  flex-direction: row;
}

.transcript-div-button {
  background-color: rgb(240, 240, 240);
  color: black;
  font-size: medium;
  border: 1px solid black;
  padding: 10px;
  margin: 10px;
  transition: 150ms;
}

.transcript-div-remove {
  background-color: transparent;
  color: red;
  border: 0;
  padding: 10px;
}

.transcript-div-remove:hover {
  cursor: pointer;
}

.transcript-div-button:hover {
  background-color: #eeeeee;
  cursor: pointer;
}

.transcript-div-button:active {
  background-color: #ebebeb;
}

.transcript-div-child {
  margin: 10px;
  padding: 5px;
  display: flex;
  align-items: center;
}

.transcript-div-label {
  font-weight: bold;
  width: 7.5vw;
  margin-left: 10px;
}

.upload-demo-container {
  position: relative;
  left: 5vw;
  /* top: 12.5vh; */
  width: 90vw;
  overflow: scroll;
  scroll-behavior: smooth;
  scrollbar-width: none;
}

.row-flex {
  display: flex;
  flex-direction: row;
  justify-content: flex-start;
  align-items: center;
}

.space-evenly {
  justify-content: space-evenly;
}

.error-btn {
  color: red;
  background-color: rgb(228, 107, 107);
}

.success-btn {
  display: flex;
  color: green;
  background-color: lightgreen;
  width: 180px;
  padding: 5px;
  border-radius: 5px;
}

.upload-demo-section {
  margin: 2.5vh;
}

.audio-json-div {
  display: flex;
  flex-direction: row;
  align-items: center;
}

.add-item-btn {
  background-color: transparent;
  color: black;
  border: 1px solid black;
  height: 45px;
  width: 180px;
  font-size: medium;
  padding: 5px;
  border-radius: 0;
  transition: 150ms;
  cursor: pointer;
  margin: 2vh 2vh;
}

.remove-item-btn {
  background-color: transparent;
  color: red;
  border: 1px solid red;
  height: 45px;
  width: 100px;
  font-size: medium;
  /* padding: 5px; */
  border-radius: 0;
  transition: 150ms;
  cursor: pointer;
  margin: 2vh 2vh;
}

.demo-btn {
  background-color: transparent;
  color: black;
  border: 1px solid black;
  font-size: medium;
  padding: 10px;
  border-radius: 0;
  transition: 150ms;
  cursor: pointer;
  margin: 0 2vh;
}

.demo-btn:hover {
  background-color: #eeeeee;
}

.remove-btn {
  font-size: medium;
  color: red;
  width: fit-content;
  padding: 10px;
  border-color: red;
}

.section-btn {
  background-color: transparent;
  color: black;
  border: 1px solid black;
  font-size: medium;
  /* padding: 10px; */
  border-radius: 0;
  transition: 150ms;
  cursor: pointer;
}

.section-btn:hover {
  background-color: #eeeeee;
}

.section-remove-btn {
  font-size: medium;
  color: red;
  width: fit-content;
  padding: 10px;
  border-color: red;
}

.file-name {
  color: #2580ff;
  margin: 1vh;
}

.sub-btn {
  font-size: large;
  color: black;
  transition: 150ms;
  border: none;
  background-color: transparent;
  padding: 6px;
  margin: 6px;
  cursor: pointer;
}

.sub-btn:hover {
  background-color: #eeeeee;
}

.active-sub-btn {
  border-bottom: 2px solid #007bff;
}

.customer-input-fields {
  display: flex;
  flex-direction: row;
  gap: 2rem;
}

.customer-details-column1 {
  display: flex;
  flex-direction: column;
}

.customer-details-column2 {
  display: flex;
  flex-direction: column;
}

.demo-section-container {
  display: flex;
  flex-direction: column;
  gap: 0.5 rem;
}

.demo-btn-container {
  display: flex;
  flex-direction: row;
}

.case-details {
  display: flex;
  flex-direction: column;
}

.demo-submit {
  background-color: rgb(0, 198, 0);
  color: white;
  font-size: large;
  border: 0;
  padding: 10px;
  cursor: pointer;
  margin: 2vh;
  justify-content: space-between;
  transition: 150ms;
}

.demo-submit:hover {
  background-color: rgb(0, 170, 0);
}

.demo-cancel-btn {
  background-color: #928f8f;
}
.demo-cancel-btn:hover {
  background-color: #616060;
}

.upload-demo-input {
  min-width: 15vw;
  width: fit-content;
  padding: 10px;
  margin: 5px;
  border: 1px solid black;
  background-color: transparent;
  font-size: medium;
}

.upload-demo-subject-description {
  width: 50vw;
  padding: 10px;
  margin: 5px;
  border: 1px solid black;
  border-radius: 0;
  background-color: transparent;
  font-size: medium;
}

.no-border {
  border: none;
}

.demo-label {
  font-size: large;
}

.demo-section {
  display: flex;
  flex-direction: row;
  align-items: center;
  margin: 2vh;
}

.section-child {
  display: flex;
  flex-direction: row;
  align-items: center;
  margin: 2vh;
}

.demo-section-child {
  width: 15vw;
}

/* VIDEO JSON CSS */
.flex-row {
  display: flex;
  flex-direction: row;
  align-items: center;
}

.flex-column {
  display: flex;
  flex-direction: column;
  align-items: center;
}

.space-between {
  justify-content: space-between;
}

.space-evenly {
  justify-content: space-evenly;
}

.space-around {
  justify-content: space-around;
}

.video-container {
  width: 35%;
}

.status-container {
  margin-top: 100px;
}

.status-container > .main-body-child {
  height: 40vh;
  display: flex;
  flex-direction: row;
  align-items: center;
}

.video-btn {
  background-color: transparent;
  color: black;
  border: 1px solid black;
  font-size: medium;
  padding: 10px;
  border-radius: 0;
  transition: 150ms;
  cursor: pointer;
  margin: 0 2vh;
}

.blue-btn {
  background-color: blue;
  color: white;
  border: none;
}

.green-btn {
  background-color: green;
  color: white;
  border: none;
}

.upload-video-btn {
  width: 100%;
  padding-top: 100px;
}

.margin-20v {
  margin: 20px 0;
}

.width-100 {
  width: 100%;
}

.language-dropdown {
  margin-left: 30px;
  width: 100px;
  padding: 10px;
  border: 1px solid grey;
}

.language-label {
  font-size: small;
  font-weight: bold;
}
/* Change colour of slider */
.noUi-connect {
  background: rgb(214, 0, 214) !important;
}

/* Reduce size of drag-tips */
.noUi-handle {
  width: 12px !important;
}

/* Handle placing for reduced size of drag-tips */
.noUi-horizontal .noUi-handle {
  right: -5px !important;
}

/* Remove vertical bars inside drag-tip */
.noUi-handle::before {
  left: 0 !important;
}

.noUi-handle::after {
  left: 0 !important;
}

@media screen and (max-width: 390px) {
  * {
    padding: 0;
    margin: 0;
  }

  .container {
    margin-left: 20px;
    margin-right: 1px;
  }

  .section-container {
    width: 32vh;
    height: 50vh;
    padding-bottom: 0;
    padding-top: 40px;
  }

  textarea {
    width: 88%;
  }

  .btn-convert {
    width: 32vh;
  }

  .audio-container {
    margin-top: 0;
  }

  .play {
    margin-right: 30px;
  }

  .download {
    margin-left: 30px;
  }

  /* .select {
        cursor: pointer;
        display: inline-block;
        position: relative;
        font: normal 11px/22px Arial, Sans-Serif;
        color: black;
        border: 1px solid #ccc;
      }
      
      .styledSelect {
        position: absolute;
        top: 0;
        right: 0;
        bottom: 0;
        left: 0;
        background-color: white;
        padding: 0 10px;
        font-weight: bold;
      }
      
      .styledSelect:after {
        content: "";
        width: 0;
        height: 0;
        border: 5px solid transparent;
        border-color: black transparent transparent transparent;
        position: absolute;
        top: 9px;
        right: 6px;
      }
      
      .styledSelect:active,
      .styledSelect.active {
        background-color: #eee;
      }
      
      .options {
        display: none;
        position: absolute;
        top: 100%;
        right: 0;
        left: 0;
        z-index: 999;
        margin: 0 0;
        padding: 0 0;
        list-style: none;
        border: 1px solid #ccc;
        background-color: white;
        -webkit-box-shadow: 0 1px 2px rgba(0, 0, 0, 0.2);
        -moz-box-shadow: 0 1px 2px rgba(0, 0, 0, 0.2);
        box-shadow: 0 1px 2px rgba(0, 0, 0, 0.2);
      }
      
      .options li {
        padding: 0 6px;
        margin: 0 0;
        padding: 0 10px;
      }
      
      .options li:hover {
        background-color: #39f;
        color: white;
      } */

  /* The container must be positioned relative: */
  .custom-select {
    position: relative;
    font-family: Arial;
  }

  .custom-select select {
    display: none; /*hide original SELECT element: */
  }

  .select-selected {
    background-color: DodgerBlue;
  }

  /* Style the arrow inside the select element: */
  .select-selected:after {
    position: absolute;
    content: "";
    top: 14px;
    right: 10px;
    width: 0;
    height: 0;
    border: 6px solid transparent;
    border-color: #fff transparent transparent transparent;
  }

  /* Point the arrow upwards when the select box is open (active): */
  .select-selected.select-arrow-active:after {
    border-color: transparent transparent #fff transparent;
    top: 7px;
  }

  /* style the items (options), including the selected item: */
  .select-items div,
  .select-selected {
    color: #ffffff;
    padding: 8px 16px;
    border: 1px solid transparent;
    border-color: transparent transparent rgba(0, 0, 0, 0.1) transparent;
    cursor: pointer;
  }

  /* Style items (options): */
  .select-items {
    position: absolute;
    background-color: DodgerBlue;
    top: 100%;
    left: 0;
    right: 0;
    z-index: 99;
  }

  /* Hide the items when the select box is closed: */
  .select-hide {
    display: none;
  }

  .select-items div:hover,
  .same-as-selected {
    background-color: rgba(0, 0, 0, 0.1);
  }
}
