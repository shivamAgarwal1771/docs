import { LightningElement, wire } from 'lwc';
import INSIDE_ICON from '@salesforce/resourceUrl/Inside_Icon';
import AI_Assistant from '@salesforce/resourceUrl/chatBotHeaderIcon';
import CROSS_ICON from '@salesforce/resourceUrl/Cross_Icon';
import CHAT_BOT from '@salesforce/resourceUrl/chatBot';
import LIKE from '@salesforce/resourceUrl/LikeAgentAssist';
import UNLIKE from '@salesforce/resourceUrl/unLikeAgentAssist';
import sendMessageToBot from '@salesforce/apex/awc_ChatBotController.sendMessageToBot';
import { ShowToastEvent } from 'lightning/platformShowToastEvent';
import { subscribe, MessageContext } from 'lightning/messageService';
import aiWikiSimulate from '@salesforce/messageChannel/aiWikiSimulate__c';
import { AGENT_TYPES } from "c/exlConstants_version2";
import LIKEFILL from '@salesforce/resourceUrl/likeFillAgentAssistant';
import UNLIKEFILL from '@salesforce/resourceUrl/unlikeFillAgentAssistant';

export default class SimulationAiWiki extends LightningElement {
    insideIcon = INSIDE_ICON;
    svgUrl = AI_Assistant;
    crossIcon = CROSS_ICON;
    userInput = '';
    isChat = true;
    isSection = false;
    iconButton = CHAT_BOT;
    like = LIKE;
    unlike = UNLIKE;
    likefill = LIKEFILL;
    unlikefill = UNLIKEFILL;
    communicationData = [];
    @wire(MessageContext)
    messageContext;
    chatDataToProcess = [];
    isProcessing = false;

    connectedCallback() {
        this.subscribeToAiWikiChannel();
    }

    subscribeToAiWikiChannel() {
        this.subscription = subscribe(
            this.messageContext,
            aiWikiSimulate,
            (message) => {
            this.handleChatMessage(message);
            }
        );
    }
    
    handleChatMessage(message) {
        console.log("ai wiki message recieved", message);
        const aiWikiData = message?.wikiChat;
        const id = `${this.chatDataToProcess?.length + 1}`;
        if (aiWikiData) {
            let wikiChat = {
                ...aiWikiData, 
                speaker: aiWikiData?.user,
                text: aiWikiData?.utterance,
                id: id, 
                isBot: aiWikiData.user === "Agent Wiki",
                isAgent: aiWikiData.user !== "Agent Wiki"
            };
            this.chatDataToProcess.push(wikiChat);
            this.handleChat();
            this.processNextChat();
        }
    }

    async processNextChat() {
        if (this.isProcessing || this.chatDataToProcess.length === 0) {
            return;
        }
        this.isProcessing = true;
        const currentItem = this.chatDataToProcess.shift();
        await this.processItem(currentItem);
        this.isProcessing = false;

        if(this.chatDataToProcess.length > 0) {
            this.processNextChat()
        }
        this.scrollToBottom();
    }
    
    async processItem(item) {
        // eslint-disable-next-line no-async-promise-executor
        return new Promise(async (resolve) => {
            if (AGENT_TYPES.has(item.user)) {
                let strArray = [...item.utterance];
                this.userInput = '';
                for (const char of strArray) {
                    this.userInput += char;
                    this.scrollToBottom();
                    // eslint-disable-next-line no-await-in-loop
                    await this.delay(100);
                }
                this.communicationData = [...this.communicationData, item];
                this.userInput = '';
            } else {
                await this.delay(1500);
                this.communicationData = [...this.communicationData, item];
            }
            this.scrollToBottom();
            resolve(item);
        });
    }
    
    delay(ms) {
        // eslint-disable-next-line @lwc/lwc/no-async-operation
        return new Promise(resolve => setTimeout(resolve, ms))
    }

    scrollToBottom() {
        const customCardContainer = this.template.querySelector('.chat-container');
        if (customCardContainer) {
            customCardContainer.scrollTop = customCardContainer.scrollHeight;
        }
    }

    handleChat(){
        this.isSection = true;
    }
    closeChatHandler(){
        this.isSection = false;
    }

    positiveResponseHandler(event) {
        // const postiveResponse = new ShowToastEvent({
        //     title: 'Success',
        //     message : 'Your Response is captured',
        //     variant: 'success',
        // })
        // this.dispatchEvent(postiveResponse);
        // const targetDiv = event.target.closest('.like-container');
        // targetDiv.classList.toggle('liked');

        let guidInd = Number(event.target.dataset.id)

        let dupe = JSON.parse(JSON.stringify([...this.communicationData]))

        if (this.communicationData[guidInd].liked) {
            dupe[guidInd].liked = false
            dupe[guidInd].disliked = false
        } else {
            dupe[guidInd].liked = true
            dupe[guidInd].disliked = false
        }

        this.communicationData = dupe
    }

    negativeResponseHandler(event) {
        // const negativeResponse = new ShowToastEvent({
        //     title: 'Warning',
        //     message : 'Your Response is captured',
        //     variant: 'Warning',
        // })
        // this.dispatchEvent(negativeResponse);
        // const targetDiv = event.target.closest('.dislike-container');
        // targetDiv.classList.toggle('disliked');

        let guidInd = Number(event.target.dataset.id)

        let dupe = JSON.parse(JSON.stringify([...this.communicationData]))

        if (this.communicationData[guidInd].disliked) {
            dupe[guidInd].disliked = false
            dupe[guidInd].liked = false
        } else {
            dupe[guidInd].disliked = true
            dupe[guidInd].liked = false
        }

        this.communicationData = dupe;
    }

    handleInput(event){
        this.userInput = event.target.value;
    }
    handleUserInput(){
        if(this.userInput == null || this.userInput.trim() === ''){
                // console.log('First click in if part');
                // const negativeResponse = new ShowToastEvent({
                //     title: 'Error',
                //     message : 'No Input Found',
                //     variant: 'Error',
                // })
                // this.dispatchEvent(negativeResponse);  
            
        }
        else{
            console.log('Second click in if part');
        const userInput = {
            speaker : 'agent',
            text : this.userInput,
            isBot: false,
            isAgent: true
        };
        console.log('userInput :',userInput)
        
        this.communicationData.unshift(userInput);
        console.log('UserInput Array :',this.communicationData);
        
        sendMessageToBot({
         userMessage : this.userInput
        }).then((response)=>{
          console.log('response :',response);
          const botResponse = {
             speaker : 'AssistBot',
             text : response,
             isBot: true,
             isAgent: false
          }
        //  this.communicationData.unshift(botResponse);
        this.communicationData = [botResponse, ...this.communicationData];
          console.log('communicationData :',this.communicationData);
        }).catch((err)=>{
           console.log('err :',err.meesage.body);
        })
        this.userInput = '';
        this.template.querySelector('.inputFieldContainer').value = '';
        }
    }
    HandleKeyPress(event){
        if(event.key === 'Enter'){
           this.handleUserInput(); 
        }
        else{
            console.log('other keys pressed');
        }
    }
}
