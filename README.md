<template>
  <div class="whole-comp" if:true={showContent}>
    <lightning-card>
      <div slot="title" class="summary-header">
        <img
          src={icon}
          alt="AI Assistant Icon"
          style="margin-left: 2vh; height: 25px; width: 25px; margin-right: 2vh"
        />
        <div
          slot="title"
          style="
            font-size: large;
            font-weight: 600;
            font-family: sans-serif;
            margin-left: -3px;
          "
        >
          Call Summary
        </div>
      </div>
    </lightning-card>
    <lightning-card>
      <div class="call-info">
        <template if:true={callInfo}>
          <h2
            slot="title"
            style="
              margin-left: 3vh;
              font-size: 1rem;
              font-weight: bold;
              color: #373737;
            "
          >
            Call Info
          </h2>
          <div
            style="padding: 1vh 1vh 1vh 2vh; margin-top: -2vh; margin-left: 3vh"
          >
            <div>
              <div style="margin: 1vh">
                <table>
                  <thead style="color: #a09fa3">
                    <tr
                      style="
                        font-weight: bold;
                        display: flex;
                        width: 100%;
                        align-items: center;
                        justify-content: space-around;
                      "
                    >
                      <th style="width: 50%">Intent Captured</th>
                      <th style="width: 50%">Repeat Call</th>
                    </tr>
                  </thead>
                  <tbody>
                    <!-- Loop through callInfo array -->
                    <template for:each={callInfo} for:item="call">
                      <tr
                        key={call.Id}
                        style="
                          display: flex;
                          width: 100%;
                          align-items: center;
                          justify-content: space-around;
                        "
                      >
                        <td style="width: 50%">{call.intentCaptured}</td>
                        <td style="width: 50%">{call.repeatCall}</td>
                      </tr>
                    </template>
                  </tbody>
                </table>
              </div>
            </div>
            <div>
              <div style="margin: 1vh">
                <table>
                  <thead style="color: #a09fa3">
                    <tr
                      style="
                        font-weight: bold;
                        display: flex;
                        width: 100%;
                        align-items: center;
                        justify-content: space-around;
                      "
                    >
                      <th style="width: 50%">Incoming Time</th>
                      <th style="width: 50%">Call duration</th>
                    </tr>
                  </thead>
                  <tbody>
                    <!-- Loop through callInfo array -->
                    <template for:each={callInfo} for:item="call">
                      <tr
                        key={call.Id}
                        style="
                          display: flex;
                          width: 100%;
                          align-items: center;
                          justify-content: space-around;
                        "
                      >
                        <td style="width: 50%">{call.incomingTime}</td>
                        <td style="width: 50%">{call.callDuration}</td>
                      </tr>
                    </template>
                  </tbody>
                </table>
              </div>
            </div>
          </div>
        </template>
      </div>
    </lightning-card>
    <lightning-card>
      <div onclick={AddLastRow} class="call-notes">
        <template if:true={Notes}>
          <h2
            slot="title"
            style="
              margin-left: 3vh;
              font-size: 1rem;
              font-weight: bold;
              color: #373737;
            "
          >
            Call Notes
          </h2>
          <ul style="margin-top: 5vh">
            <div style="margin-left: 3vh">
              <template for:each={Notes} for:item="note" for:index="index">
                <li
                  key={note.id}
                  style="
                    margin-top: -6vh;
                    padding-right: 15px;
                    padding-bottom: 10px;
                    padding-left: 15px;
                    margin-bottom: 3vh;
                  "
                >
                  <lightning-textarea
                    type="text"
                    value={note}
                    onchange={handleNoteChange}
                    onkeypress={handleKeyPress}
                    onclick={handleInputClick}
                    class="input-field"
                    style="max-height: 52px; margin-bottom: 4vh"
                    data-id={index}
                  >
                    ></lightning-textarea
                  >
                </li>
              </template>
            </div>
          </ul>
        </template>
        <template for:each={inputFieldItem} for:item="itemField">
          <div
            style="padding: 10px 15px 10px 15px; margin-bottom: 1.7vh"
            key={itemField.id}
            onkeypress={addRow}
          >
            <div
              style="
                margin-left: 3vh;
                margin-top: -7vh;
                display: flex;
                flex-direction: row;
                justify-content: center;
                align-items: center;
              "
              class="input-container slds-form-element__control slds-input-has-icon slds-input-has-icon_right"
            >
              <lightning-input
                type="text"
                value={itemField.value}
                data-id={itemField.id}
                onchange={handleInputChange}
                placeholder="Create New Notes....."
                class="input-field custom-input"
                onclick={handleInputClick}
                style="margin-right: 2vh; width: 100%"
              >
              </lightning-input>
              <lightning-button-icon
                style="
                  margin-top: 1vh;
                  width: 20px;
                  height: 20px;
                  margin-right: 2vh;
                "
                icon-name="utility:delete"
                data-id={itemField.id}
                onclick={deleteEmptyRow}
              >
              </lightning-button-icon>
            </div>
          </div>
        </template>
      </div>
    </lightning-card>
    <lightning-card>
      <div class="resolution">
        <h2
          slot="title"
          style="
            margin-left: 3vh;
            font-size: 1rem;
            font-weight: bold;
            color: #373737;
          "
        >
          Resolution
        </h2>
        <template if:true={myQuestions}>
          <ul>
            <div style="margin-left: 3vh">
              <template for:each={myQuestions} for:item="quiz">
                <div
                  key={quiz.id}
                  style="margin-bottom: 3vh; margin-left: 2.5vh"
                >
                  <div style="color: #a09fa3; font-weight: 500">
                    <strong>{quiz.question}</strong>
                  </div>
                  <div
                    style="
                      display: flex;
                      flex-direction: row;
                      justify-content: flex-start;
                      align-items: center;
                    "
                  >
                    <div>
                      <input
                        type="radio"
                        name={quiz.id}
                        value="a"
                        onchange={changeHandler}
                        style="margin-right: 1.5vh; accent-color: #713dba"
                      />
                      {quiz.answers.a}
                    </div>
                    <div style="margin-left: 1rem">
                      <input
                        type="radio"
                        name={quiz.id}
                        value="b"
                        onchange={changeHandler}
                        style="margin-right: 1.5vh; accent-color: #713dba"
                      />
                      {quiz.answers.b}
                    </div>
                    <!-- <div class="slds-col">
                                    <input type="radio" name={quiz.id} value="c" onchange={changeHandler}/>
                                    
                                </div> -->
                  </div>
                </div>
              </template>
            </div>
          </ul>
        </template>
      </div>
    </lightning-card>
    <lightning-card>
      <div class="footer">
        <button class="button" onclick={submitHandler}>Submit</button>
      </div>
    </lightning-card>
  </div>
</template>



import { LightningElement, api, track, wire } from "lwc";
import interactionCallSummary from "@salesforce/apex/CallSummaryInteraction.interactionCallSummary";
import { subscribe, MessageContext } from "lightning/messageService";
import { ShowToastEvent } from "lightning/platformShowToastEvent";
// import callMsg from "@salesforce/messageChannel/callMessage__c";
import CALL_SUMMARY from "@salesforce/resourceUrl/SolarNotes";
import summaryDataJson from "@salesforce/resourceUrl/bnym_summary";
import transcriptSimulate from "@salesforce/messageChannel/transcriptSimulate__c";

export default class SimulationCallSummary extends LightningElement {
  @api recordId;
  @track callInfo = [];
  @track callNotes = { statements: [] };
  @track myQuestions = [];
  Notes = this.callNotes.statements;
  @track showContent = false;
  interactionData = {};
  selected = {};
  correctAnswers = 0;
  isSubmitted = false;
  inputfield;
  followUpNeeded;
  issueResolved;
  intentCorrect;
  icon = CALL_SUMMARY;
  inputValue;
  keyIndex = 0;

  async loadStaticData() {
    try {
      const response = await fetch(summaryDataJson);
      if (response.ok) {
        const responseJson = await response.json();

        let infoObj = {
          intentCaptured: "",
          repeatCall: "",
          incomingTime: "",
          callDuration: ""
        };

        infoObj.intentCaptured = responseJson.callSummary["Intent Captured"];
        infoObj.repeatCall = responseJson.callSummary["Repeat Call"];
        infoObj.incomingTime = responseJson.callSummary["Incoming Time"];
        infoObj.callDuration = responseJson.callSummary["Call Duration"];

        this.callInfo.push(infoObj);

        responseJson.callNotes.forEach((item) => {
          this.callNotes.statements.push(item.description);
        });

        responseJson.resolution.forEach((item, index) => {
          let obj = {
            id: "Question" + String(index + 1),
            question: item.QUESTION,
            answers: {
              a: item.OPTIONS[0].value,
              b: item.OPTIONS[1].value
            }
          };

          this.myQuestions.push(obj);
        });
      } else {
        console.error("Error in fetching summary data ", response.status);
      }
    } catch (err) {
      console.error("Error in fetching summary json data ", err);
    }
  }

  inputFieldItem = [
    {
      id: 0
    }
  ];

  addRow(event) {
    console.log("event invoked");
    if (event.key === "Enter") {
      // const index = this.inputFieldItem.findIndex(
      //   (item) => item.id === event.target.dataset.id
      // );
      // console.log("INdex==> " + index);
      // const currentValue = event.target.value.trim();
      // console.log("Current Value " + currentValue);

      let isEmpty = false;

      console.log("Summary ", [...this.inputFieldItem]);

      this.inputFieldItem.forEach((item) => {
        console.log("Summary ", { ...item });
        if (!item.value || item.value === "") {
          isEmpty = true;
        }
      });

      if (!isEmpty) {
        ++this.keyIndex;
        const newInputField = [{ id: this.keyIndex }];

        this.inputFieldItem = this.inputFieldItem.concat(newInputField);
      } else {
        console.log("Error found");
        const toastEvent = new ShowToastEvent({
          title: "Error",
          message: "Input Fields Can Not Be Empty",
          variant: "Error"
        });
        this.dispatchEvent(toastEvent);
      }
    }
  }

  handleNoteChange(event) {
    const noteId = event.target.dataset.id;
    const editedNote = event.target.value;
    if (editedNote !== this.Notes[noteId]) {
      // Update the corresponding call note in the array
      this.Notes[noteId] = editedNote;
    }
    // this.Notes[noteId] = editedNote;
    console.log("this.Notes[noteId] :", this.Notes[noteId]);
    console.log("this.Notes :", this.Notes);
  }

  handleInputChange(event) {
    const dataId = parseInt(event.target.dataset.id, 10);
    console.log("dataId :", dataId);
    const editNotes = event.target.value;
    console.log("editNotes :", editNotes);
    this.inputFieldItem = this.inputFieldItem.map((item) => {
      return item.id === dataId ? { ...item, value: editNotes } : item;
    });
    console.log("inputField :", JSON.stringify(this.inputFieldItem));
  }

  deleteEmptyRow(event) {
    event.stopPropagation();

    console.log("Delete Method Invoke");

    const itemId = parseInt(event.target.dataset.id, 10); // Convert data-id to integer
    console.log("Item ID:", itemId);

    const index = this.inputFieldItem.findIndex((item) => item.id === itemId);
    console.log("Index:", index);

    if (index !== -1) {
      if (
        !this.inputFieldItem[index].value ||
        this.inputFieldItem[index].value.trim() === ""
      ) {
        this.inputFieldItem.splice(index, 1);
        // To ensure reactivity, assign a new array reference
        this.inputFieldItem = [...this.inputFieldItem];
      } else {
        console.log("The field is not empty, so it will not be removed.");
        const toastEvent = new ShowToastEvent({
          title: "Error",
          message: "Input Fields have data",
          variant: "Error"
        });
        this.dispatchEvent(toastEvent);
      }
    } else {
      console.error("Item not found in the array.");
      const toastEvent = new ShowToastEvent({
        title: "Error",
        message: "Not Found",
        variant: "Error"
      });
      this.dispatchEvent(toastEvent);
    }
  }

  AddLastRow() {
    console.log("AddlastRow invoke");
    // if (this.inputFieldItem.length > 1) {
    //     this.inputFieldItem = this.inputFieldItem.slice(0, -1);
    // }
    // console.log('this.inputFieldItem :',this.inputFieldItem);
    const allFilled = this.inputFieldItem.every(
      (item) => item.value && item.value.trim() !== ""
    );

    if (allFilled) {
      ++this.keyIndex;
      const newInputField = [
        {
          id: this.keyIndex
        }
      ];
      this.inputFieldItem = this.inputFieldItem.concat(newInputField);
    } else {
      console.log("2nd Error found");
      const toastEvent = new ShowToastEvent({
        title: "Error",
        message: "Input Fields Can Not Be Empty",
        variant: "Error"
      });
      this.dispatchEvent(toastEvent);
      console.log("Not all input fields are filled");
    }

    console.log("inputField :", this.inputFieldItem);
  }

  handleInputClick(event) {
    event.stopPropagation();
    console.log("this.inputFieldItem :", this.inputFieldItem);
  }

  // handleInputChange(event) {

  //     const noteId = event.target.dataset.id;
  //     const editedNote = event.target.value;
  //     if (editedNote !== this.Notes[noteId]) {
  //         // Update the corresponding call note in the array
  //         this.Notes[noteId] = editedNote;
  //     }
  //    // this.Notes[noteId] = editedNote;
  //     const cardID  = event.target.dataset.id;
  //     if(cardID === "1"){
  //        this.inputfield = event.target.value;
  //     }

  // }

  submitHandler() {
    if (this.callInfo[0].repeatCall === "Yes") {
      this.callInfo[0].repeatCall = true;
    } else {
      this.callInfo[0].repeatCall = false;
    }
    console.log("SubmitHandler called");
    console.log("Call Notes:", this.Notes);
    console.log("input Field :", this.inputfield);

    console.log("Interaction Data:", this.interactionData);
    interactionCallSummary({
      recordId: this.recordId,
      notes: this.Notes,
      customCallNotes: this.inputFieldItem,
      followUpNeeded: this.followUpNeeded,
      issueResolved: this.issueResolved,
      intentCorrect: this.intentCorrect,
      callDuration: this.callInfo[0].callDuration,
      incomingTime: this.callInfo[0].incomingTime,
      repeatCall: this.callInfo[0].repeatCall,
      intendCaptured: this.callInfo[0].intendCaptured
    })
      .then(() => {
        const successToastEvent = new ShowToastEvent({
          title: "Success",
          message: "Interation History Record Created Successfully",
          variant: "success"
        });
        this.dispatchEvent(successToastEvent);

        // this.selected = {};
      })
      .catch((error) => {
        console.error("Error is" + JSON.stringify(error));
        const errorToastEvent = new ShowToastEvent({
          title: "Error",
          message: error.body.message,
          variant: "error"
        });
        this.dispatchEvent(errorToastEvent);
      });
  }

  changeHandler(event) {
    console.log("Change handler called");
    const questionId = event.target.name;
    const selectedAnswer = event.target.value;
    this.selected[questionId] = selectedAnswer;
    for (const key in this.selected) {
      if (Object.hasOwnProperty.call(this.selected, key)) {
        const question = this.myQuestions.find((item) => item.id === key);
        const questionText = question ? question.question : "Unknown Question";
        const selectAnswer = this.selected[key];
        let answerValue;
        if (selectAnswer === "a") {
          answerValue = true;
        } else if (selectAnswer === "b") {
          answerValue = false;
        } else {
          answerValue = null; // Or any default value if needed
        }
        this.interactionData[questionText] = selectAnswer;
        // Assigning to different properties based on the condition
        if (questionText === "Was the captured intent correct?") {
          this.intentCorrect = answerValue;
        } else if (questionText === "Was this issue resolved?") {
          this.issueResolved = answerValue;
        } else if (questionText === "Is any follow-up needed?") {
          this.followUpNeeded = answerValue;
        }
        console.log("followUpNeeded :", this.followUpNeeded);
        console.log("issueResolved :", this.issueResolved);
        console.log("intentCorrect :", this.intentCorrect);
        console.log("Interaction Data:", this.interactionData);
      }
    }
  }

  @wire(MessageContext)
  messageContext;

  subscription;

  connectedCallback() {
    this.subscribeToMessageChannel();
    this.loadStaticData();
  }

  subscribeToMessageChannel() {
    // this.subscription = subscribe(this.messageContext, callMsg, (message) => {
    //   this.handleMessage(message);
    // });
    this.subscription = subscribe(
      this.messageContext,
      transcriptSimulate,
      (message) => {
        this.handleMessage(message);
      }
    );
  }

  handleMessage(message) {
    console.log("summary ", message);
    // if (message.callStatus) {
    //   let status = message.callStatus;
    //   if (status === "ENDED") {
    //     this.showContent = true;
    //   } else {
    //     this.showContent = false;
    //   }
    // }
    if (message?.transcriptSegment?.isCallEnded) {
      this.showContent = true;
    }
  }
}
