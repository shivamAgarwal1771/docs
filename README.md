import { LightningElement, wire, api, track } from 'lwc';
import callMsg from '@salesforce/messageChannel/callMessage__c';
import transcriptChannel from '@salesforce/messageChannel/transcriptEvent__c';
import { subscribe, MessageContext, APPLICATION_SCOPE, unsubscribe } from 'lightning/messageService';
import botImg from '@salesforce/resourceUrl/awc_ai_assistance_icon';
import { apiKey, resourceWSURI, resourceHttpURI, authBase, payloadBase, callIdPlaceholder,textPlaceholder, registerOnAddTranscript,automationBotQuery, getTranscriptsQuery, getBotResponse, unregisterSubscription, getCallQuery, consoleLog, registerOnUpdateCall } from "c/exlConstants";
import awc_ai_pencil_icon from '@salesforce/resourceUrl/awc_ai_pencil_icon';
import { CurrentPageReference } from 'lightning/navigation';
import LIKE from '@salesforce/resourceUrl/LikeAgentAssist';
import UNLIKE from '@salesforce/resourceUrl/unLikeAgentAssist';
import LIKEFULL from '@salesforce/resourceUrl/awc_like_full';
import UNLIKEFULL from '@salesforce/resourceUrl/awc_dislike_full';
import { ShowToastEvent } from 'lightning/platformShowToastEvent';
import END_LABEL from '@salesforce/label/c.awc_ended_keyword_label';

export default class Ai_assists extends LightningElement {
    svgUrl = botImg;
    @track showAssist = true;
    @track showContent = true;
    @api isEndCall;
    messages = [];
    like = LIKE;
    unlike = UNLIKE;
    flike = LIKEFULL;
    funlike = UNLIKEFULL;
    subscription;
    callId = ''; //Temporary CallId, to be removed later
    Status;

    @track donelike1 = true;
    @track doneUnlike1 = true;
    @track donelike2 = true;
    @track doneUnlike2 = true;
    @track donelike3 = true;
    @track doneUnlike3 = true;

    @track dataStep = {};
    @track guidanceData;
    @track guidanceTitle = '';
    @track speach_suggestion = "";
    @track knowledgeArticlesName='';
    @track intentDetectedKA='';
    @track knowledgeArticlesValue=[];
    @track aliasId ;
    @track botId;
    @track knowledgeArticlesTitle;
    @track messageAIAssist;
    @track channelAIAssist;

    phoneNumber;
    endLabel = END_LABEL;

    @wire(MessageContext)
    messageContext;

    //fetching data from apex : 
    callStatus;

    connectedCallback() {
        this.subscribeToMessageChannel();
        this.subscribeToTranscriptChannel();
    }

    renderedCallback() {
        // Call the Apex method to fetch data
        //this.retrieveData();
    }

    retrieveData() {
        publishMessage()
            .then(result => {
                // Log the result to the console
                console.log('Data from Apex---->', result);
                this.data = result;
            })
            .catch(error => {
                console.error('Error fetching data-----> ', error);
            });
    }


    recievedmsg;

    subscribeToMessageChannel() {
        this.subscription = subscribe(
            this.messageContext,
            callMsg,
            (message) => {
                this.handleMessage(message);
            }
        );
    }

    subscribeToTranscriptChannel() {
        console.log('ACT_WOR subscribeToTranscriptChannel')
        this.subscription = subscribe(
            this.messageContext,
            transcriptChannel,
            (message) => {
                this.handleTranscriptMessage(message);
            }
        );
    }

    handleMessage(message) {
        // Handle received message
        consoleLog('Call Status ---> '+JSON.stringify(message.callStatus));
        if (message.callStatus) {
            // Extract sentiment value and add data to the chart
            const status = message?.callStatus?.Status;
            this.showAssist = status && status.toUpperCase() === 'ENDED' ? false : true;
        }// if null then header should be visible, hideAssist will remain false

    }

    handleTranscriptMessage(message) {
        console.log('ACT_WOR Got transcript ---> '+JSON.stringify(message.transcriptSegments))
        if (message.transcriptSegments) {
            let segment = message.transcriptSegments[0];
            if(segment.IsPartial != true && segment.Channel == 'CALLER' && segment.Sentiment){
                console.log('ACT_WOR Transcript', segment)
                if (this.ifClickOnParticularWorkflow) {
                    this.makeBotQuery(segment.Transcript, true)
                }
            }
        }
    }

    @wire(CurrentPageReference)
    getCurrentUrlParameters(currentPageReference) {

        if (currentPageReference) {
            let state = currentPageReference.state;
            this.phoneNumber = state.c__phone;
            this.callId = state.c__callid;
            this.Status = state.c__status;
        }

        //get transcript data
        this.connectSocket();

    }

    /* Creating Open and hide section for toggle*/
    @track isContentVisible = false;

    get buttonLabel() {
        return this.isContentVisible ? 'Hide' : 'Show More';
    }
    handleToggle() {
        this.isContentVisible = !this.isContentVisible;
        this.isDivVisible = !this.isDivVisible;
    }


    @track isDivVisible = true;

    toggleDiv() {
    }

    /* open toste on click of like and dislike*/
    /*GUIDANCE*/

    get likeIcon() {
        return this.donelike1 ? this.like : this.flike;
    }

    get dislikeIcon() {
        return this.doneUnlike1 ? this.unlike : this.funlike;
    }

    toggleLike() {
        this.donelike1 = !this.donelike1;
        this.doneUnlike1 = true;  // Reset the dislike state when like is toggled
        const event = new ShowToastEvent({
            title: this.donelike1 ? 'Disliked the Assist.' : 'Liked the Assist.',
            message: 'Thanks for providing feedback',
            variant: this.donelike1 ? 'warning' : 'success',
            mode: 'dismissable'
        });
        this.dispatchEvent(event);
    }

    toggleDislike() {
        this.doneUnlike1 = !this.doneUnlike1;
        this.donelike1 = true;  // Reset the like state when dislike is toggled
        const event = new ShowToastEvent({
            title: this.doneUnlike1 ? 'Liked the Assist.' : 'Disliked the Assist.',
            message: 'Thanks for providing feedback',
            variant: this.doneUnlike1 ? 'warning' : 'error',
            mode: 'dismissable'
        });
        this.dispatchEvent(event);
    }


    /*SPEECH SUGESTION*/
    get likeIcon1() {
        return this.donelike2 ? this.like : this.flike;
    }

    get dislikeIcon1() {
        return this.doneUnlike2 ? this.unlike : this.funlike;
    }

    toggleLike1() {
        this.donelike2 = !this.donelike2;
        this.doneUnlike2 = true;  // Reset the dislike state when like is toggled
        const event = new ShowToastEvent({
            title: this.donelike2 ? 'Disliked the Assist.' : 'Liked the Assist.',
            message: 'Thanks for providing feedback',
            variant: this.donelike2 ? 'warning' : 'success',
            mode: 'dismissable'
        });
        this.dispatchEvent(event);
    }

    toggleDislike1() {
        this.doneUnlike2 = !this.doneUnlike2;
        this.donelike2 = true;  // Reset the like state when dislike is toggled
        const event = new ShowToastEvent({
            title: this.doneUnlike2 ? 'Liked the Assist.' : 'Disliked the Assist.',
            message: 'Thanks for providing feedback',
            variant: this.doneUnlike2 ? 'warning' : 'error',
            mode: 'dismissable'
        });
        this.dispatchEvent(event);
    }

    /*KNOWLEDGE ARTICLE*/
    get likeIcon2() {
        return this.donelike3 ? this.like : this.flike;
    }

    get dislikeIcon2() {
        return this.doneUnlike3 ? this.unlike : this.funlike;
    }

    toggleLike2() {
        this.donelike3 = !this.donelike3;
        this.doneUnlike3 = true;  // Reset the dislike state when like is toggled
        const event = new ShowToastEvent({
            title: this.donelike3 ? 'Disliked the Assist.' : 'Liked the Assist.',
            message: 'Thanks for providing feedback',
            variant: this.donelike3 ? 'warning' : 'success',
            mode: 'dismissable'
        });
        this.dispatchEvent(event);
    }

    toggleDislike2() {
        this.doneUnlike3 = !this.doneUnlike3;
        this.donelike3 = true;  // Reset the like state when dislike is toggled
        const event = new ShowToastEvent({
            title: this.doneUnlike3 ? 'Liked the Assist.' : 'Disliked the Assist.',
            message: 'Thanks for providing feedback',
            variant: this.doneUnlike3 ? 'warning' : 'error',
            mode: 'dismissable'
        });
        this.dispatchEvent(event);
    }

    /* Knowledge article model and body */
    isModalOpen = false;
    articleUrl = '';

    handleOpenModal() {
    // Replace with the actual URL of the Knowledge Article
        this.articleUrl = 'https://help.salesforce.com/s/articleView?id=000384767&type=1';
        this.isModalOpen = true;
    }

    handleCloseModal() {
        this.isModalOpen = false;
    }

    /**
     * Action Workflow
     */
    @track isJsonShowing = true;
    @track ifClickOnParticularWorkflow = false;
    @track currentWorkflowIndex = null;
    @track completedWorkflow = false;
    @track showRecentUpdatedActionWorkflow = false;
    @track recentCompletedWorkflow = null;
    @track isDisabled = true;
    @track isDataPresent1 = false;
    @track isDataPresent2 = false;


    @track currentAddress = '';
    @track newAddress = '';
    @track newmovementDate = '';
    @track intentDetected = '';
    @track intentDescription = '';
    @track totalSteps = '';
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
        let message = 'start workflow'; // this.messageAIAssist   
        let botIds = this.botId; // 'VVCTYRGZUO'      
        let aliasIds = this.aliasId; // 'TSTALIASID'     
        console.log('=========FIRST CALL=============');
        // this.isLoading = true;  // Show the spinner
        this.makeBotQuery('start workflow');
    }


    async makeBotQuery(text, stop, stopAgain) {

            this.ifClickOnParticularWorkflow = true;
            this.isJsonShowing = false;
            this.isLoading = true;  // Show the spinner
            console.log('ACT_WOR makeBotQuery', JSON.stringify(text))


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

            console.log('ACT_WOR data', JSON.stringify(data))
            // Parse the stringified array inside getAutomationBotResponse
            const botResponses = JSON.parse(data.data.getAutomationBotResponse);

            // Ensure botResponses is an array
            if (Array.isArray(botResponses)) {
                botResponses.forEach(currentItem => {
                    let content;
                    
                    // Check if currentItem.content is a string, and parse it if necessary
                    if (typeof currentItem.content === 'string') {
                        content = JSON.parse(currentItem.content);
                    } else {
                        content = currentItem.content;
                    }

                    // Handle different types of content
                    if (content.message) {
                        this.speach_suggestion = content.message;
                    }
                    if (content.prompt) {
                        this.speach_suggestion = content.prompt;
                    }
                    if (content.confirmation) {
                        this.speach_suggestion = content.confirmation;
                    }
                    if (content.intentDetected) {
                        console.log('Inside ID');
                        this.intentDetected = content.intentDetected;
                        this.intentDescription = content.intentDescription;
                        this.totalSteps = content.totalSteps;

                        // Process steps if they exist
                        if (content.steps) {
                            this.steps = []
                            content.steps.forEach(step => {
                                const objStep = {
                                    stepName: step.stepName,
                                    isEditable: step.card.isEditable,
                                    messageText: step.card.messages.length > 0 ? step.card.messages[0].text : ""
                                };
                                this.steps.push(objStep);
                            });
                        }
                    }
                });
                console.log('STEP ------>'+JSON.stringify(this.steps));
                this.isLoading = false; // Added for Spinner
                if (stop !== true) {
                    console.log('ACT_WOR =========SECOND CALL=============');
                    this.makeBotQuery('Elizabeth Smith', true);
                }
                // if (stop == true && stopAgain == true) {
                //     console.log('=========FINAL CALL=============');
                //     this.makeBotQuery(' I understand. The amount I need to withdraw is $2,000.', true, true);
                // }
            }
        } catch (error) {
            console.error(error);
        }
    } 

    
    
    

    handleBotResponse(response) {
        console.log('response from apex ================>> ' + JSON.stringify(response.data));
    }

    handleStopWorkFlow(event) {
        this.ifClickOnParticularWorkflow = false;
        this.isJsonShowing = true;
        System.debug('this.ifClickOnParticularWorkflow ' + this.ifClickOnParticularWorkflow);
    }

    handleInputChange(event) {
        // Get the name of the input field
        const fieldName = event.target.name;
        console.log('fieldName -------->', fieldName);
        // Set the corresponding property based on the input field name
        if (fieldName === 'newAddress') {
            this.newAddress = event.target.value;
            if (this.newAddress) {
                this.isDataPresent1 = true;
            } else {
                this.isDataPresent1 = false;
            }
        }
        if (fieldName === 'movementDate') {
            this.movementDate = event.target.value;
            if (this.movementDate) {
                this.isDataPresent2 = true;
            } else {
                this.isDataPresent2 = false;
            }
        }
        console.log('Add -------->', this.newAddress);
        console.log('Data -------->', this.movementDate);

        if (this.isDataPresent2 == true && this.isDataPresent1 == true) {
            this.isDisabled = false;
        } else {
            this.isDisabled = true;
        }
    }

    handleSave() {
        consoleLog('<--------------HANDLE CLICK IS ACTIVE--------------->');
        let newJsonData = JSON.parse(JSON.stringify(this.jsonData));
        const currentWorkflow = newJsonData.data.getAutomationBotResponse[this.currentWorkflowIndex];
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
        this.speach_suggestion = '';
    }

    /* ConnectSocket */
    socket;

    connectSocket(){
        let exlWSURI = resourceWSURI+"?header="+authBase()+"&payload="+payloadBase();
        this.socket = new WebSocket(exlWSURI,"graphql-ws");
        this.socket.onopen = () => {
            if(this.callId) {
                this.subscribeToAddTranscript();
                this.subscribeToBot();
            }
        };
        this.socket.onmessage = (event) => this.handleSubscription(event);
    }

    subscribeToAddTranscript() {
        let jsonRegisterAddTranscript = JSON.stringify(registerOnAddTranscript).replace(callIdPlaceholder,this.callId);
        this.socket.send(jsonRegisterAddTranscript);
    }

    subscribeToBot(){
        let jsonAIAssistanceBot = JSON.stringify(registerOnAddTranscript).replace(callIdPlaceholder,this.callId);
        this.socket.send(jsonRegisterAddTranscript);  
    }
    
    async handleSubscription(event) {
        if(event !== undefined && event.data) {
           console.log('Subscription Event Received AI Assist 00==> '+JSON.stringify(event.data));
            let response = JSON.parse(event.data);
            if(response.payload && response.payload.data) {
                if(response.payload.data.onAddTranscriptSegment != null){
                    const transcriptSegment = response.payload.data.onAddTranscriptSegment;
                    this.processTranscriptSegments([transcriptSegment]);
                }
                if(response.payload.data.onUpdateCall != null) {
                    //alert('Update call response received');
                    const status = response.payload.data.onUpdateCall.Status;
                    this.callStatus = status;
                    this.publishCallStatus(this.callStatus);
                }
            }
        }
    }

    processTranscriptSegments(transcriptSegments) {
        console.log('%%%%%%%% ai assistant processTranscriptSegments %%%%%%%'+JSON.stringify(transcriptSegments));
        if(transcriptSegments) {
            for(let segment of transcriptSegments) {  
                console.log('@@@@@@@@@@@@@@@@@@@@@@@@@ segment @@@@@@@@@@@@@@@@@@@@@@@@@'+JSON.stringify(segment));
                console.log('IsPartial------>'+JSON.stringify(segment.IsPartial));
                consoleLog('message for AI-Assist:', this.messageAIAssist);
                consoleLog('channel for AI-Assist:', this.channelAIAssist);


                if(segment.IsPartial == false) {
                    if(segment.Channel == 'AGENT_ASSISTANT'){
                        const dataString = segment.Transcript;
                        let data;
                        try {
                            data = JSON.parse(dataString);
                        } catch (error) {
                            console.error('Error parsing JSON:', error);
                            return;
                        }

                        data.forEach(item => {
                            console.log('Titles:', item.Title);
                            console.log('message:', item.message);
                            console.log('botId:', item.botId);
                            this.botId = item.botId;
                            
                            if(item.Title === 'AI Guidance'){
                                this.guidanceTitle = item.Title;
                                 let msg = item.message;
                                 const guidanceArray = msg.split('<br>');
                                 this.guidanceData = guidanceArray.map((item, index) => {
                                    return {
                                        key: index,  
                                        value: item  
                                    };
                                });
                                
                                console.log('guidanceData =====>', JSON.stringify(this.guidanceData));                         
                            }

                            if(item.name === 'Knowledge Articles'){
                                this.knowledgeArticlesName = item.name;
                                this.knowledgeArticlesTitle = data[1].value[0].title;
                                let valueKA = data[1].value[0].detail;
                                this.knowledgeArticlesValue = valueKA.map((item, index) => {
                                    return {
                                        key: index,  
                                        value: item  
                                    };
                                });
                            }

                            if(item.botId != null){
                                this.aliasId = item.aliasId;
                                this.intentDetectedKA = item.intentDetected;
                            }
                          });
                        }else if(segment.Channel == 'CALLER'){
                            this.messageAIAssist = segment.Transcript;
                            this.channelAIAssist = segment.Channel;
                        }else{

                        }
                    }
            }

        }
    }

    wfCompleted() {
        const event = new ShowToastEvent({
            title: 'Workflow Completed Successfully.',
            message: '',
            variant: 'success',
            mode: 'dismissable'
        });
        this.dispatchEvent(event);
    }  
}
