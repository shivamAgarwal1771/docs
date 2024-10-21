import { LightningElement, wire, api, track } from "lwc";
import callMsg from "@salesforce/messageChannel/callMessage__c";
import transcriptChannel from "@salesforce/messageChannel/transcriptEvent__c";
import {
    subscribe,
    MessageContext,
    APPLICATION_SCOPE,
    unsubscribe
} from "lightning/messageService";
import botImg from "@salesforce/resourceUrl/awc_ai_assistance_icon";
import { USER_FEEDBACK_EVENTS, CALL_STATUS, SPEAKER, VISIBILITY, WORKFLOW, FIELDS, NUDGES } from 'c/exlConstants';
import {
    apiKey,
    resourceWSURI,
    resourceHttpURI,
    authBase,
    payloadBase,
    callIdPlaceholder,
    textPlaceholder,
    registerOnAddTranscript,
    automationBotQuery,
    getTranscriptsQuery,
    getBotResponse,
    unregisterSubscription,
    getCallQuery,
    consoleLog,
    registerOnUpdateCall
} from "c/exlConstants";
import awc_ai_pencil_icon from "@salesforce/resourceUrl/awc_ai_pencil_icon";
import { CurrentPageReference } from "lightning/navigation";
import LIKE from "@salesforce/resourceUrl/LikeAgentAssist";
import UNLIKE from "@salesforce/resourceUrl/unLikeAgentAssist";
import LIKEFULL from "@salesforce/resourceUrl/awc_like_full";
import UNLIKEFULL from "@salesforce/resourceUrl/awc_dislike_full";
import { ShowToastEvent } from "lightning/platformShowToastEvent";
export default class Ai_assists extends LightningElement {
    svgUrl = botImg;
    @track showAssist = true;
    @track showContent = true;
    @api isEndCall;
    messages = [];
    like = LIKE;
    dislike = UNLIKE;
    liked = LIKEFULL;
    disliked = UNLIKEFULL;
    subscription;
    callId = ""; //Temporary CallId, to be removed later
    Status;

    @track aiAssistantData = [];

    @track dataStep = {};
    @track guidanceData;
    @track guidanceTitle = "";
    @track speach_suggestion = [];
    @track knowledgeArticlesName = "";
    @track intentDetectedKA = "";
    @track knowledgeArticlesValue = [];
    @track aliasId;
    @track botId;
    @track knowledgeArticlesTitle;
    @track messageAIAssist;
    @track channelAIAssist;

    phoneNumber;

    @wire(MessageContext)
    messageContext;

    //fetching data from apex :
    callStatus;

    connectedCallback() {
        this.subscribeToTranscriptChannel();
    }

    renderedCallback() {
        this.scrollToBottom();
    }

    scrollToBottom() {
        const customCardContainer = this.template.querySelector(
            ".ai-assistant-container"
        );
        console.log(customCardContainer,"test5")
        if (customCardContainer) {
            customCardContainer.scrollTop = customCardContainer.scrollHeight;
        }
    }

    subscribeToTranscriptChannel() {
        this.subscription = subscribe(
            this.messageContext,
            transcriptChannel,
            (message) => {
                this.handleTranscriptMessage(message);
            }
        );
    }

    handleTranscriptMessage(message) {
        if (message?.transcriptSegments) {
            for (let segment of message?.transcriptSegments) {
                if (segment.Channel == SPEAKER.AGENT_ASSISTANT) {
                    const dataString = segment?.Transcript;
                    try {
                        let parsedTranscript = JSON.parse(dataString);

                        // If we get an object, change it to an array
                        if (!Array.isArray(parsedTranscript)) {
                            parsedTranscript = [parsedTranscript]
                        }

                        parsedTranscript.forEach((item) => {
                            if (item?.Title === NUDGES.AI_GUIDANCE) {
                                this.guidanceTitle = item?.Title;
                                let msg = item?.message;
                                const guidanceArray = msg.split("<br>");
                                this.guidanceData = guidanceArray?.map((item, index) => {
                                    return {
                                        key: index,
                                        value: item
                                    };
                                });
                                this.aiAssistantData.push({
                                    guidance: true,
                                    createdAt: new Date().toISOString(),
                                    title: this.guidanceTitle,
                                    data: this.guidanceData,
                                    key: this.aiAssistantData.length + 1,
                                    liked: false,
                                    disliked: false
                                })
                            }

                            if (item?.name === NUDGES.KNOWLEDGE_ARTICLES) {
                                this.knowledgeArticlesName = item?.name || '';
                                this.knowledgeArticlesTitle = item?.value[0]?.title || '';
                                let valueKA = item?.value[0]?.detail || '';
                                this.knowledgeArticlesValue = valueKA?.map((item, index) => {
                                    return {
                                        key: index,
                                        value: item
                                    };
                                });
                                this.aiAssistantData.push({
                                    articles: true,
                                    createdAt: new Date().toISOString(),
                                    name: this.knowledgeArticlesName,
                                    title: this.knowledgeArticlesTitle,
                                    data: this.knowledgeArticlesValue,
                                    key: this.aiAssistantData.length + 1,
                                    liked: false,
                                    disliked: false
                                })
                            }

                            this.botId = item?.botId;

                            if (item.botId) {
                                this.aliasId = item.aliasId;
                                this.intentDetectedKA = item?.intentDetected || '';
                            }
                        });
                    } catch (error) {
                        console.error("Error parsing JSON:", error);
                        return;
                    }
                }

                if (
                    segment?.IsPartial != true &&
                    segment?.Channel == SPEAKER.CALLER &&
                    segment?.Sentiment
                ) {
                    if (this.ifClickOnParticularWorkflow) {
                        this.makeBotQuery(segment?.Transcript, true);
                    }
                }
            }
        }
    }

    @wire(CurrentPageReference)
    getCurrentUrlParameters(currentPageReference) {
        if (currentPageReference) {
            let state = currentPageReference?.state;
            this.phoneNumber = state.c__phone;
            this.callId = state.c__callid;
            this.Status = state.c__status;
        }
    }

    /* Creating Open and hide section for toggle*/
    @track isContentVisible = false;

    get buttonLabel() {
        return this.isContentVisible ? VISIBILITY.HIDE : VISIBILITY.SHOW;
    }
    handleToggle() {
        this.isContentVisible = !this.isContentVisible;
        this.isDivVisible = !this.isDivVisible;
    }

    @track isDivVisible = true;

    /* open toste on click of like and dislike*/
    /*GUIDANCE*/

    /* Knowledge article model and body */
    isModalOpen = false;
    articleUrl = "";

    handleOpenModal() {
        // Replace with the actual URL of the Knowledge Article
        this.articleUrl = "https://help.salesforce.com/s/articleView?id=000384767&type=1";
        this.isModalOpen = true;
    }

    handleCloseModal() {
        this.isModalOpen = false;
    }

    /**
     * Action Workflow
     */
    @track isJsonShowing = true;
    @track jsonData = {
        data: {
            getAutomationBotResponse: [
                {
                    content: {
                        intentDetected: this.intentDetected,
                        intentDescription: this.intentDescription,
                        totalSteps: this.totalSteps,
                        steps: [
                            {
                                stepName: this.resStep,
                                state: this.resState,
                                card: {
                                    type: "textBox",
                                    isEditable: false,
                                    messages: [
                                        {
                                            text: "current_address = Unit 31, Martin Street, Chicago, New York, 52351"
                                        }
                                    ]
                                }
                            }
                        ],
                        contentType: "CustomPayload"
                    },
                    contentType: "CustomPayload"
                }
            ]
        }
    };
    @track ifClickOnParticularWorkflow = false;
    @track currentWorkflowIndex = null;
    @track completedWorkflow = false;
    @track showRecentUpdatedActionWorkflow = false;
    @track recentCompletedWorkflow = null;
    @track isDisabled = true;
    @track isDataPresent1 = false;
    @track isDataPresent2 = false;

    @track currentAddress = "";
    @track newAddress = "";
    @track newmovementDate = "";
    @track intentDetected = "";
    @track intentDescription = "";
    @track totalSteps = "";
    @track resStep = [];
    @track resState = [];
    @track editable;
    @track cardData = [];
    @track stepRunData = [];

    promptLabel;
    promptValue;

    @track typeCard = [];
    @track editCard = [];
    @track messageCard = [];
    @track textValue = [];
    //fetching data from apex :

    callId; //Temporary CallId, to be removed later
    Status;
    phoneNumber;

    //Icons List
    edit_icon = awc_ai_pencil_icon;

    @track vSteps = {
        stepName: this.resStep,
        state: this.resState
    };
    @track workflowSteps = [];
    @track steps = [];
    @track isFirt = false;
    @track isLoading = false;
    @track callWF = false;

    handleRunClick(event) {
        // POST Method call
        //let conversationId = this.callId; //'220335042009125';
        let message = WORKFLOW.START_WORKFLOW; // this.messageAIAssist
        let botIds = this.botId; // 'VVCTYRGZUO'
        let aliasIds = this.aliasId; // 'TSTALIASID'
        // this.isLoading = true;  // Show the spinner
        this.makeBotQuery(WORKFLOW.START_WORKFLOW);
    }

    async makeBotQuery(text, stop) {
        this.ifClickOnParticularWorkflow = true;
        this.isJsonShowing = false;
        this.isLoading = true; // Show the spinner

        try {
            const response = await fetch(resourceHttpURI, {
                method: "POST",
                headers: {
                    "Content-Type": "application/json",
                    "x-api-key": `${apiKey}`
                },
                body: JSON.stringify(automationBotQuery).replace(textPlaceholder, text)
            });

            const data = await response.json();
            // Parse the stringified array inside getAutomationBotResponse
            const botResponses = JSON.parse(data?.data?.getAutomationBotResponse);

            // Ensure botResponses is an array
            if (Array.isArray(botResponses)) {
                botResponses.forEach((currentItem) => {
                    let content;

                    // Check if currentItem.content is a string, and parse it if necessary
                    if (typeof currentItem?.content === "string") {
                        content = JSON.parse(currentItem?.content);
                    } else {
                        content = currentItem.content;
                    }

                    // Handle different types of content
                    if (content?.message) {
                        this.aiAssistantData.push({
                            speech_suggestion: true,
                            createdAt: new Date().toISOString(),
                            data: content.message,
                            key: this.aiAssistantData.length + 1,
                            liked: false,
                            disliked: false
                        })
                    }
                    if (content?.prompt) {
                        this.aiAssistantData.push({
                            speech_suggestion: true,
                            createdAt: new Date().toISOString(),
                            data: content.prompt,
                            key: this.aiAssistantData.length + 1,
                            liked: false,
                            disliked: false
                        })
                    }
                    if (content?.confirmation) {
                        this.aiAssistantData.push({
                            speech_suggestion: true,
                            createdAt: new Date().toISOString(),
                            data: content.confirmation,
                            key: this.aiAssistantData.length + 1,
                            liked: false,
                            disliked: false
                        })
                    }
                    if (content.intentDetected) {
                        this.intentDetected = content?.intentDetected;
                        this.intentDescription = content?.intentDescription;
                        this.totalSteps = content?.totalSteps;

                        // Process steps if they exist
                        if (content.steps) {
                            this.steps = [];
                            content.steps.forEach((step) => {
                                const objStep = {
                                    stepName: step?.stepName,
                                    isEditable: step?.card?.isEditable,
                                    messageText:
                                        step?.card?.messages?.length > 0
                                            ? step?.card?.messages[0].text
                                            : ""
                                };
                                this.steps.push(objStep);
                            });
                        }
                    }
                });

                this.isLoading = false; // Added for Spinner

                if (stop !== true) {
                    this.makeBotQuery("Elizabeth Smith", true);
                }
            }
        } catch (error) {
            console.error(error);
        }
    }

    handleStopWorkFlow(event) {
        this.ifClickOnParticularWorkflow = false;
        this.isJsonShowing = true;
        System.debug(
            "this.ifClickOnParticularWorkflow " + this.ifClickOnParticularWorkflow
        );
    }

    handleInputChange(event) {
        // Get the name of the input field
        const fieldName = event.target.name;
        // Set the corresponding property based on the input field name
        if (fieldName === FIELDS.ADDRESS) {
            this.newAddress = event.target.value;
            if (this.newAddress) {
                this.isDataPresent1 = true;
            } else {
                this.isDataPresent1 = false;
            }
        }
        if (fieldName === FIELDS.DATE) {
            this.movementDate = event.target.value;
            if (this.movementDate) {
                this.isDataPresent2 = true;
            } else {
                this.isDataPresent2 = false;
            }
        }

        if (this.isDataPresent2 == true && this.isDataPresent1 == true) {
            this.isDisabled = false;
        } else {
            this.isDisabled = true;
        }
    }

    handleSave() {
        this.handleBack();
        this.wfCompleted();
    }

    seeCompletedWorkflow() {
        this.ifClickOnParticularWorkflow = false;
        this.isJsonShowing = false;
        this.completedWorkflow = false;
        this.showRecentUpdatedActionWorkflow = true;
    }
    handleBack() {
        this.ifClickOnParticularWorkflow = false;
        this.isJsonShowing = false;
        this.completedWorkflow = true;
        this.showRecentUpdatedActionWorkflow = false;
        this.speach_suggestion = "";
    }

    wfCompleted() {
        const event = new ShowToastEvent({
            title: "Workflow Completed Successfully.",
            message: "",
            variant: "success",
            mode: "dismissable"
        });
        this.dispatchEvent(event);
    }

    handleUserFeedback(event) {
        const dataKey = event.target.getAttribute('data-key');
        const dataValue = event.target.getAttribute('data-value');
        let selectedData = this.aiAssistantData[dataKey];
        if (selectedData && Object.keys(selectedData).length) {
            selectedData.liked = dataValue === USER_FEEDBACK_EVENTS.LIKE ? true : false;
            selectedData.disliked = dataValue === USER_FEEDBACK_EVENTS.DISLIKE ? true : false;
        }
        // const e = new ShowToastEvent({
        //     title: selectedData.liked ? "Liked the Assist." : "DisLiked the Assist.",
        //     message: "Thanks for providing feedback",
        //     variant: selectedData.liked ? "success" : "error",
        //     mode: "dismissable"
        // });
        // this.dispatchEvent(e);
        this.aiAssistantData[dataKey] = selectedData;
        event.stopPropogation();
    }
}


<template>
  <div class="ai-assistant-container">
    <lightning-card>
      <div>
        <span> <img src={svgUrl} class="roboIconHeaderCss" /> </span>
        <span class="headerCSS"> AI Assistant </span>
      </div>
    </lightning-card>
    <template lwc:if={showAssist}>
      <template lwc:if={aiAssistantData} for:each={aiAssistantData} for:item="assistantData" for:index="index">
        <!-- ########## HEADER AI-ASSISTANCE  ########## -->

        <!-- ########## AI-GUIDANCE  ########## -->
        <lightning-card if:true={assistantData.guidance} key={assistantData.key}>
          <div class="secondContainer">
            <h1 class="heading">{assistantData.title}</h1>
            <div class="sign">
              <div class="like_dislike_iconCss">
                <div class="likeCss">
                  <img lwc:if={assistantData.liked} src={liked} alt="AI Assistant Icon" data-key={index}
                    data-value="liked" onclick={handleUserFeedback} />
                  <img lwc:else src={like} alt="AI Assistant Icon" data-key={index} data-value="like"
                    onclick={handleUserFeedback} />
                </div>
                <div class="dislikeCss">
                  <img lwc:if={assistantData.disliked} src={disliked} alt="AI Assistant Icon" class="dislike-container"
                    data-key={index} data-value="disliked" onclick={handleUserFeedback} />
                  <img lwc:else src={dislike} alt="AI Assistant Icon" class="dislike-container" data-key={index}
                    data-value="dislike" onclick={handleUserFeedback} />
                </div>
              </div>
            </div>
            <template for:each={assistantData.data} for:item="item" for:index="index">
              <p class="paragraph" key={item.key}>{item.value}</p>
            </template>
          </div>
        </lightning-card>

        <!-- ########## KNOWLEDGE ARTICLES  ########## -->
        <lightning-card if:true={assistantData.articles} key={assistantData.key}>
          <div class="secondContainer">
            <h1 class="heading">{assistantData.name}</h1>

            <div class="sign">
              <div class="like_dislike_iconCss">
                <div class="likeCss">
                  <img lwc:if={assistantData.liked} src={liked} alt="AI Assistant Icon" data-key={index}
                    data-value="liked" onclick={handleUserFeedback} />
                  <img lwc:else src={like} alt="AI Assistant Icon" data-key={index} data-value="like"
                    onclick={handleUserFeedback} />
                </div>
                <div class="dislikeCss">
                  <img lwc:if={assistantData.disliked} src={disliked} alt="AI Assistant Icon" class="dislike-container"
                    data-key={index} data-value="disliked" onclick={handleUserFeedback} />
                  <img lwc:else src={dislike} alt="AI Assistant Icon" class="dislike-container" data-key={index}
                    data-value="dislike" onclick={handleUserFeedback} />
                </div>
              </div>
            </div>

            <div class="slds-box boxCss">
              <p>{assistantData.title}</p>

              <lightning-button variant="base" label={buttonLabel} onclick={handleToggle}></lightning-button>

              <div if:false={isDivVisible} class="slds-m-top_medium showMore">
                <template for:each={assistantData.data} for:item="item" for:index="index">
                  <p class="paragraph" key={item.key}>{item.value}</p>
                </template>
              </div>
            </div>
          </div>
        </lightning-card>

        <!-- ########## SPEECH SUGGESTION  ########## -->
        <lightning-card if:true={assistantData.speech_suggestion} key={assistantData.key}>
          <div class="secondContainer">
            <h1 class="heading">Speech Suggestions</h1>
            <div class="sign">
              <div class="like_dislike_iconCss">
                <div class="likeCss">
                  <img lwc:if={assistantData.liked} src={liked} alt="AI Assistant Icon" data-key={index}
                    data-value="liked" onclick={handleUserFeedback} />
                  <img lwc:else src={like} alt="AI Assistant Icon" data-key={index} data-value="like"
                    onclick={handleUserFeedback} />
                </div>
                <div class="dislikeCss">
                  <img lwc:if={assistantData.disliked} src={disliked} alt="AI Assistant Icon" class="dislike-container"
                    data-key={index} data-value="disliked" onclick={handleUserFeedback} />
                  <img lwc:else src={dislike} alt="AI Assistant Icon" class="dislike-container" data-key={index}
                    data-value="dislike" onclick={handleUserFeedback} />
                </div>
              </div>
            </div>

            <div>
              <div class="dottedBoxCss">
                <div class="dottedboxTEXT_Css">
                  <h1>{assistantData.data}</h1>
                </div>
              </div>
            </div>
          </div>
        </lightning-card>
      </template>
      <!-- ########## KNOWLEDGE ARTICLES BODY ########## -->
      <template if:true={isModalOpen}>
        <!--<c-awc_knowledge_article article-url={articleUrl} onclose={handleCloseModal}></c-awc_knowledge_article>-->
        <template for:each={knowledgeArticlesValue} for:item="KnowledgeArticle" for:index="ka">
          <div key={KnowledgeArticle}>{KnowledgeArticle}</div>
        </template>
      </template>

      <!-- ########## WORKFLOW ########## -->
      <template if:true={botId}>
        <lightning-card>
          <!--#### HEADER ####-->
          <div class="secondContainer">
            <h1 class="heading">Action Workflow</h1>
          </div>
          <br />

          <!--Run WF Functionality-->
          <template if:true={isJsonShowing}>
            <template for:each={jsonData.data.getAutomationBotResponse} for:item="workflow" for:index="index">
              <div key={intentDetected}>
                <div>
                  <h4 style="margin-left: 25px">
                    <b>Intent Detected</b>
                    <span style="color: #a32eff">{intentDetected}</span>
                  </h4>
                  <span> <img src={edit_icon} class="roboIconCss" /> </span>
                </div>
                <br />
                <h4 style="margin-left: 25px"><b>Proposed Workflow</b></h4>
                <br />
                <div style="
                      display: flex;
                      width: 89%;
                      border: 1px solid lightgray;
                      margin-left: 35px;
                      padding: 10px;
                      border-radius: 5px;
                    ">
                  <div class="runSectionCss"><b>Start Workflow</b></div>
                  <button class="runbuttonCss" data-index={index} onclick={handleRunClick}>
                    Run
                  </button>
                </div>
                <br />
              </div>
            </template>
          </template>

          <!--Completed Workflow-->
          <template if:true={completedWorkflow}>
            <h4 style="margin-left: 25px"><b>Completed Workflow</b></h4>
            <br />
            <div style="
                  display: flex;
                  width: 89%;
                  border: 1px solid lightgray;
                  margin-left: 35px;
                  padding: 10px;
                  border-radius: 5px;
                ">
              <div style="width: 70%"><b>{intentDetected}</b></div>
            </div>
          </template>

          <lightning-spinner if:true={isLoading} alternative-text="Loading"></lightning-spinner>
          <!--Intent Detected Workflow-->
          <template if:false={isLoading}>
            <template if:true={ifClickOnParticularWorkflow}>
              <div class="slds-p-around_medium">
                <div if:true={intentDetected}>
                  <h4 style="margin-left: 25px">
                    <b>Intent Detected</b>
                    <span style="color: #a32eff">{intentDetected}</span>
                  </h4>
                  <br />

                  <h4 style="margin-left: 25px">
                    <b>Default Workflow: Initiator</b>
                  </h4>
                  <b style="margin-left: 25px">Total Steps {totalSteps}</b>
                  <span>
                    <img src={edit_icon} class="stopwfpencilIconCss" />
                  </span>
                </div>
                <button class="stopworkflow" onclick={handleStopWorkFlow}>
                  Stop Workflow
                </button>
                <br />

                <div if:true={intentDetected}>
                  <!--Workflow Fields-->
                  <template for:each={steps} for:item="step">
                    <div key={step.stepName}>
                      <div class="slds-box" style="margin-top: 7px; border: 2px solid #f0ecec">
                        <lightning-input label={step.stepName} value={step.messageText}
                          disabled={step.isEditable}></lightning-input>
                      </div>
                    </div>
                  </template>

                  <!--Mark workflow as cpmpleted Button-->
                  <template if:true={isDisabled}>
                    <button class="wfCompletedButtonCss" data-index={index} onclick={handleSave}>
                      Mark workflow as completed
                    </button>
                  </template>
                  <template if:false={isDisabled}>
                    <button class="wfCompletedButtonCss_disabled" data-index={index} onclick={handleSave}
                      disabled={isDisabled}>
                      Mark workflow as completed
                    </button>
                  </template>
                </div>
              </div>
            </template>
            <template if:true={showRecentUpdatedActionWorkflow}>
              <div class="slds-p-around_medium">
                <b>Total Steps {totalSteps}</b>
                <button class="backButtonCss" data-index={index} onclick={handleBack}>
                  Back
                </button>
                <!--Workflow Fields-->
                <template for:each={steps} for:item="step">
                  <div key={step.stepName}>
                    <div class="slds-box" style="margin-top: 7px; border: 2px solid #f0ecec">
                      <lightning-input label={step.stepName} value={step.messageText} disabled="true"></lightning-input>
                    </div>
                  </div>
                </template>
                <!--Date Fields-->
                <template if:true={newmovementDate}>
                  <lightning-input type="date" label="Date of Movement" value={newmovementDate}
                    disabled></lightning-input>
                </template>
              </div>
            </template>
          </template>
        </lightning-card>
      </template>
    </template>
  </div>
</template>
