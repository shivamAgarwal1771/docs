import { LightningElement, api, track, wire } from 'lwc';
import bnym_v1 from '@salesforce/resourceUrl/bnym_v1';
import bnym_v1_json from '@salesforce/resourceUrl/bnym_v1_json';
import transcriptSimulate from '@salesforce/messageChannel/transcriptSimulate__c';
import contactDataJson from "@salesforce/resourceUrl/bnym_contact_data";
import autoAuditSimulate from '@salesforce/messageChannel/autoAuditSimulate__c';
import { publish, MessageContext } from 'lightning/messageService';
import HideLightningHeader from '@salesforce/resourceUrl/HideLightningHeader';
import { loadStyle, loadScript } from 'lightning/platformResourceLoader';
import aiWikiSimulate from '@salesforce/messageChannel/aiWikiSimulate__c';

export default class AudioPlayer extends LightningElement {
    @api mp3Url = `${bnym_v1}/bnym.mp3`;
    @api autoPlay = false;
    @track customerName;
    staticData = [];
    completedIndexes  = [];
    @wire(MessageContext)
    messageContext
    
    /**
     * Adding initial computation here which is required for this component.
     */
    connectedCallback() {
        this.loadName()
        // Responsible for loading static json for mutating other components
        this.loadStaticData();
        loadStyle(this, HideLightningHeader)
    }

    loadName() {
        try {
          const response = fetch(contactDataJson);
          if (response.ok) {
            const responseJson = response.json();
            Object.keys(responseJson).forEach((key) => {
             if(key=="Name"){
                this.customerName=responseJson[key]
                console.log(this.customerName,"shivam")
             }
            }
        );
          } else {
            console.error("Error in fetching contact data ", response.status);
          }
        } catch (err) {
          console.error("Error in fetching contact json data ", err);
        }
      }

    async loadStaticData() {
        // Create a request for the JSON data and load it synchronously,
        // parsing the response as JSON into the tracked property

        try {
            const response = await fetch(bnym_v1_json);
            if (response.ok) {
                const responseJson = await response.json();
                this.staticData = responseJson;
                this.playAudio();
            }else {
                console.error("Error in fetching data", response.status);
            }
        } catch(err) {
            console.error("Error in fetching json data", err);
        }
    }

    playAudio() {
        const audioElement = this.template.querySelector('audio');
        console.log("audioElement", audioElement)
        if (audioElement) {
            audioElement.play()
            .then(() => {
                console.log('Audio playback started successfully.');
            })
            .catch(error => {
                console.error('Error attempting to play audio:', error);
            });
        }
    }

    publishTranscriptSegments(transcriptSegment) {
        if(transcriptSegment) {
            // Define the message payload
            const messagePayload = {
                transcriptSegment: transcriptSegment
            };
            // Publish the message
            publish(this.messageContext, transcriptSimulate, messagePayload);
        }
    }

    publishAutoAudit(auditData) {
        if(auditData) {
            // Define the message payload
            const messagePayload = {
                auditData: auditData
            };
            // Publish the message
            publish(this.messageContext, autoAuditSimulate, messagePayload);
        }
    }

    publishAiWiki(wikiChat) {
        if(wikiChat) {
            // Define the message payload
            const messagePayload = {
                wikiChat: wikiChat
            };
            // Publish the message
            publish(this.messageContext, aiWikiSimulate, messagePayload);
        }
    }

    extractLiveTranscript(currentTime) {
        let transcript = this.staticData
        let currentIndex = transcript?.findIndex(obj => (currentTime > obj.StartTime && currentTime < obj.EndTime))
        if (!this.completedIndexes.includes(currentIndex) && currentIndex > -1) {
            this.completedIndexes.push(currentIndex);
            setTimeout(async () => {
                let eventData = transcript?.[currentIndex].eventData
                let endTime = transcript?.[currentIndex].EndTime
                let {
                    Transcript: {
                        ChannelId = undefined,
                        Transcript: utterance = undefined,
                        IsPartial = undefined,
                        ResultId = undefined,
                        Sentiment = undefined,
                        SentimentScore = undefined,
                        showDisabilityForm = false
                    },
                    Insights = undefined,
                    Audits = undefined,
                    AIWikiChat = undefined,
                    Guidance = undefined,
                    Cases = undefined,
                    InteractionHistory = undefined,
                    CallSummary = undefined,
                    ContactInfo = undefined
                } = eventData

                if (IsPartial === true || IsPartial === undefined) {
                    return
                }

                let timeToSort = Date.now() + endTime * 1000
                let session = new Date().toISOString()

                if (Audits?.length !== 0) {
                    Audits?.forEach(audit => {
                        this.publishAutoAudit(audit);
                    })
                }

                if (Guidance && Guidance?.length !== 0) {
                    Guidance?.forEach(item => {
                        if (item.type === "SPEECH_SUGGESTION") {
                            this.publishTranscriptSegments({
                                "user": "AGENT_ASSISTANT",
                                "isSpeechSuggestion": true,
                                "key": currentIndex,
                                "liked": false,
                                "disliked": false,
                                "type": item.type,
                                "entityName": item.name,
                                "entityValue": item.value,
                                "createdAt": session
                            });
                        } else if(item.type === "KNOWLEDGE_ARTICLE"){
                            this.publishTranscriptSegments({
                                "user": "AGENT_ASSISTANT",
                                "isKnowledgeArticle": true,
                                "key": currentIndex,
                                "liked": false,
                                "disliked": false,
                                "type": item.type,
                                "entityName": item.name,
                                "entityValue": item.value,
                                "createdAt": session
                            });
                        }

                        if (item.type === "ACTION_WORKFLOW" && item.cardShown) {
                            let steps = item?.steps || [];
                            steps?.map((step)=>{
                                step.promptValue = step?.card?.messages && step?.card?.messages[0] && step?.card?.messages[0]?.text ? step?.card?.messages[0]?.text : "";
                                step.isDisabled = (step?.card?.isEditable === true || step?.card?.isEditable === false) ? step?.card?.isEditable : false;
                            });
                            this.publishTranscriptSegments({
                                "user": "AGENT_ASSISTANT",
                                "type": item?.type,
                                "intent": item?.intent,
                                "steps": steps,
                                "workflowType": item?.workflowType,
                                "name": item?.name,
                                "createdAt": new Date(),
                                "isActionWorkflow": true,
                            });
                        }
                    })
                }

                // if (Cases && Cases?.length !== 0) {
                //     Cases?.forEach(item => {
                //         dispatch(updateCases(item))
                //     })
                // }

                // if (InteractionHistory && InteractionHistory?.length !== 0) {
                //     InteractionHistory?.forEach(item => {
                //         dispatch(updateInteractionHistory(item))
                //     })
                // }

                let trancriptObj = {
                    "user": ChannelId === 'ch_0' ? this.customerName : "Agent",
                    "segmentId": ResultId,
                    utterance,
                    session,
                    "sentiment": Sentiment,
                    "sentimentScore": SentimentScore,
                    timeToSort
                }

                console.log("trancriptObj", trancriptObj);

                if (ChannelId === 'AGENT_ASSISTANT') {
                    let agentAssitObj = JSON.parse(utterance);
                    if(agentAssitObj?.Title && agentAssitObj?.message){
                        this.publishTranscriptSegments({
                            "user": "AGENT_ASSISTANT",
                            "type": "AI_GUIDANCE",
                            "key": currentIndex,
                            "isAiGuidance": true,
                            "liked": false,
                            "disliked": false,
                            "createdAt": session,
                            "entityName": agentAssitObj.Title,
                            "entityValue": agentAssitObj.message,
                        });
                    }
                } else {
                    this.publishTranscriptSegments(trancriptObj);
                }

                // if (Insights?.length !== 0) {
                //     Insights?.forEach(insight => {
                //         let insightData = {
                //             "type": AI_ASSISTANT_TYPE.AI_GUIDANCE,
                //             "createdAt": session,
                //             "entityName": insight.entity,
                //             "entityValue": insight.value,
                //             "user": USER_TYPE.CALLER
                //         }

                //         dispatch(setAiAssistantData(insightData))
                //     })
                // }

                if (AIWikiChat && AIWikiChat?.length !== 0) {
                    AIWikiChat?.forEach(wiki => {
                        let wikiChat = {
                            "utterance": wiki?.utterance,
                            "user": wiki?.user,
                            "feedback": 0,
                            "time": new Date().toLocaleTimeString([], {hour: 'numeric', minute: 'numeric'})
                        }
                        this.publishAiWiki(wikiChat);
                    })
                }

                // if (ContactInfo) {
                //     dispatch(setSimulationContactInfo(ContactInfo))
                // }

            }, (transcript?.[currentIndex].EndTime - currentTime) * 1000)
        }
    }

    handlePlay(event) {
        console.log('Audio playback started at: ', event.target.currentTime);
    }

    handlePause(event) {
        console.log('Audio playback paused at: ', event.target.currentTime);
    }

    handleEnded(event) {
        console.log('Audio playback ended at: ', event.target.currentTime);
        this.publishTranscriptSegments({
            isCallEnded: true
        });
    }
    
    handleTimeUpdate(event) {
        const currentTime = event.target.currentTime;
        this.extractLiveTranscript(currentTime);
    }
}
