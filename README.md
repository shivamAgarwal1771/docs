import React from "react";
import { BsSoundwave, BsMicMute, BsMic } from "react-icons/bs";
import { FiPhoneForwarded, FiPhoneOff } from "react-icons/fi";
import { IoPauseOutline, IoPlaySharp } from "react-icons/io5";
import { useDispatch, useSelector } from "react-redux";
import {
  setIsCallConnected,
  setIsCallMuted,
  updateCallDisconnectTime,
  updateCallDuration,
  updateCallDurationString,
  updateCallSummary,
  setIsCallOnHold
} from "../../../store/callSlice";
import { useEffect, useState } from "react";
import moment from "moment";
import {
  CALL_INTERACTION_WIDGET_TEXT,
  INITIAL_DURATION_DATA_PROPS,
} from "../../../utility/constants";
import { changeSecondsToMinutes } from '../../../utility'
import currentInstance from "../../cti/session";

const NewCallWidget = () => {
  const dispatch = useDispatch();
  const [callDurationInCall, setCallDurationInCall] = useState(
    INITIAL_DURATION_DATA_PROPS
  );
  const appCallData = useSelector((state: any) => state.call);
  const callDisconnectTime = appCallData.callDisconnectTime;

  const [callConnectionIntervalUpdated, setCallConnectionIntervalUpdated] = useState(null);

  const disconnectCall = () => {
    dispatch(updateCallSummary(""))
    dispatch(setIsCallConnected(false))
    dispatch(updateCallDisconnectTime({ disconnectTime: new Date() }))
  }

  // useEffect(()=>{
  //   currentInstance.contact?.getAgentConnection()?.hold({
  //     success: function () {
  //       console.log("Contact is on hold");
  //     },
  //     failure: function () {
  //       console.log("Failed to put contact on hold");
  //     },
  //   });
  //   updateInteractionState
  // },[appCallData?.ringingCall])
  // Interaction related event handler
  const updateInteractionState = (event) => {
    if (
      event.target.outerText == CALL_INTERACTION_WIDGET_TEXT.ACCEPT_CALL ||
      event.target.outerText == CALL_INTERACTION_WIDGET_TEXT.ACCEPT_CONVERSATION ||
      event.target.id == "pickupInteraction"
    ) {
      console.log("Accept contact");
      currentInstance.contact?.accept({
        success: function () {
          console.log("Accepted contact via Streams");
        },
        failure: function () {
          console.log("Failed to accept contact via Streams");
        },
      });
    }
    // hold conversation
    else if (event.target.id == "holdInteraction") {
      currentInstance.contact?.getAgentConnection()?.hold({
        success: function () {
          console.log("Contact is on hold");
        },
        failure: function () {
          console.log("Failed to put contact on hold");
        },
      });
    } else if (event.target.id == "resumeInteraction") {
      currentInstance.contact?.getAgentConnection()?.resume({
        success: function () {
          console.log("Contact resumed");
        },
        failure: function () {
          console.log("Failed to resume contact");
        },
      });
    }
    else if (event.target.id == "disconnectInteraction") {
      currentInstance.contact?.getAgentConnection()?.destroy({
        success: function () {
          console.log("Disconnected contact via Streams");
        },
        failure: function () {
          console.log("Failed to disconnect contact via Streams");
        },
      });
    }
    else if (
      event.target.outerText == "Mute" ||
      event.target.id == "muteInteraction"
    ) {
      // mute unmute not working
      if (!appCallData.isCallMuted) {
        // eslint-ignore-next-line
        currentInstance?.agent?.mute();
      } else {
        currentInstance?.agent?.unmute();
      }
      dispatch(setIsCallMuted(!appCallData.isCallMuted));
    }
  }

  useEffect(() => {
    if (callDisconnectTime && callConnectionIntervalUpdated) {
      onDisconnectAction();
    }
  }, [callDisconnectTime]);

  const onDisconnectAction = () => {
    clearInterval(callConnectionIntervalUpdated);
    setCallConnectionIntervalUpdated(null);
  }

  const msToTime = (duration) => {
    let seconds: number | string = Math.floor((duration / 1000) % 60);
    let minutes: number | string = Math.floor((duration / (1000 * 60)) % 60);
    let hours: number | string = Math.floor((duration / (1000 * 60 * 60)) % 24);

    hours = (hours < 10) ? "0" + hours : hours;
    minutes = (minutes < 10) ? "0" + minutes : minutes;
    seconds = (seconds < 10) ? "0" + seconds : seconds;
    return hours + ":" + minutes + ":" + seconds;
  }

  useEffect(() => {
    if (currentInstance.contact) {
      const id = setInterval(() => {
        try {
          const timer = currentInstance.contact?.getStateDuration();
          const currentMoment = moment();
          const durationString = msToTime(timer);
          const connectTime = moment(new Date(appCallData.callConnectedTime));
          const durationInSeconds = Math.floor(
            currentMoment.diff(connectTime) / 1000
          )
          // setCallDurationInCall({
          //   durationString,
          //   durationInSeconds 
          // });
          dispatch(updateCallDuration(durationInSeconds));
          dispatch(updateCallDurationString(durationString));
        } catch (error) {
          console.error('Error while fetching contact : getStateDuration', error);
          clearInterval(callConnectionIntervalUpdated);
        }
      }, 1000);
      setCallConnectionIntervalUpdated(id);
    }
    return () => onDisconnectAction();
  }, [appCallData.callConnectedTime])


  return (
    <div className="new-call-widget">
      <div className="new-call-widget_duration-container">
        <div className="new-call-widget_wave">
          <BsSoundwave size={24} />
        </div>
        <p className="new-call-widget_call-duration">
          {appCallData?.isSimulation ? (changeSecondsToMinutes(appCallData?.callSimulation?.time) || '00:00') : appCallData.totalCallDurationString}
        </p>
      </div>
      <div className="new-call-widget-button-container">
        <button id="transferInteraction" onClick={updateInteractionState}>
          <FiPhoneForwarded />
        </button>
        <button id="muteInteraction" title={` ${appCallData.isCallMuted ? `Muted` : `Unmuted`}`}>
          {appCallData.isCallMuted ? (
            <BsMic id="muteInteraction" onClick={updateInteractionState} />
          ) : (
            <BsMicMute id="muteInteraction" onClick={updateInteractionState} />
          )}
        </button>
        <button id="holdInteraction" onClick={()=>appCallData.isCallOnHold==true?dispatch(setIsCallOnHold(false)):dispatch(setIsCallOnHold(true))} >
          {appCallData.isCallOnHold ? (
            <IoPlaySharp id="holdInteraction" onClick={updateInteractionState} />
          ) : (
            <IoPauseOutline id="resumeInteraction" onClick={updateInteractionState} />
          )}
        </button>
        <button
          id="disconnectInteraction"
          onClick={() => disconnectCall()}
          className="new-call-widget__call-end"
        >
          <FiPhoneOff id="disconnectInteraction" />
        </button>
      </div>
    </div>
  );
};

export default NewCallWidget;





import { useState, useEffect } from 'react'
import { useSelector, useDispatch } from 'react-redux';
import { useRouter } from 'next/router';
import { selectClient } from "../../store/userSlice"
import { updateCallStartTime } from '../../store/callSlice'
import {
    setRingingCall,
    setContextDetails,
    setSimulationUseCase,
    updateTranscriptData,
    setAiAssistantData,
    updateCallAudits,
    updateCases,
    updateInteractionHistory,
    updateCallSimulationStatus,
    updateCallSimulationPlayDuration,
    updateAIWikiConversation,
    updateCallSummary,
    setIsCallConnected,
    updateCallDisconnectTime,
    setDetectedWorkFlow,
    setCompletedWorkFlow,
    setActionWorkFlowData,
    setRunningWorkFlow,
    setSimulationContactInfo,
    updateClickButton,
    setCardDetails,
    setExtratedDetails,
} from '../../store/callSlice';
import ReactAudioPlayer from 'react-audio-player';
import { BsFillPlayFill, BsPauseFill } from "react-icons/bs";
import { AI_ASSISTANT_TYPE, USER_TYPE, HEADERS } from '../../utility/constants'
import { changeSecondsToMinutes } from '../../utility'
import { getStaticResource } from '../../utility/automationBotHandler'

const CallSimulatonPlayer = () => {
    const locale = useSelector((state) => state.user.locales)
    const dispatch = useDispatch()
    const call = useSelector(state => state.call)
    const [resources, setResources] = useState(undefined)
    const status = call?.callSimulation?.status
    const [completedIndexes, setCompletedIndexes] = useState([])
    const [transcriptInfo, setTranscriptInfo] = useState(null)
    const [audioInfo, setAudioInfo] = useState(null)
    const [audioRef, setAudioRef] = useState(undefined)
    const setSelectedClient = useSelector(state => state.user)
    const clientJson = setSelectedClient.selectedClient
    const router = useRouter();
    useEffect(() => {
        let audioTranscriptInfo;
        try {
                getStaticResource("transcript.json", "fnol_v1").then((data) => setTranscriptInfo(data))
                getStaticResource("audio.mp3", "fnol_v1").then((data) => setAudioInfo(data))
        } catch (error) { console.log(error) }
    }, [])

    useEffect(() => {
        if (transcriptInfo && audioInfo) {
            setResources({
                audio: audioInfo,
                transcript: transcriptInfo
            })
          dispatch(setRingingCall(true))
        }
    }, [audioInfo, transcriptInfo])



    useEffect(() => {
        if (call?.callSimulation.status === 'PLAY') {
            audioRef?.audioEl?.current.play()
        }
        if (call?.callSimulation.status === 'PAUSED') {
            audioRef?.audioEl?.current.pause()
        }
    }, [call?.callSimulation.status && audioRef])

    function handleOnPlay() {
        let currentTime = audioRef?.audioEl?.current.currentTime

        audioRef?.audioEl?.current.play()
        dispatch(updateCallSimulationStatus('PLAY'))
        dispatch(updateCallSimulationPlayDuration(currentTime))
    }

    useEffect(() => {
        if (call?.isCallOnHold) {
            handleOnPause()
        } else {
            handleOnPlay()
        }
    }, [call?.isCallOnHold])

    function handleOnPause() {
        let currentTime = audioRef?.audioEl?.current.currentTime
        audioRef?.audioEl?.current.pause()

        dispatch(updateCallSimulationStatus('PAUSED'))
        dispatch(updateCallSimulationPlayDuration(currentTime))
    }

    function handleOnListen() {
        let currentTime = audioRef?.audioEl?.current.currentTime

        dispatch(updateCallSimulationPlayDuration(currentTime))
        extractLiveTranscript(currentTime)
    }

    function handleOnError(e) {
        console.error('Audio play error: ', e)
    }

    function extractLiveTranscript(currentTime) {
        let transcript = resources?.transcript;
        let currentIndex = transcript?.findIndex(obj => (currentTime > obj.StartTime && currentTime < obj.EndTime))
        if (!completedIndexes.includes(currentIndex) && currentIndex > -1) {
            setCompletedIndexes([...completedIndexes, currentIndex])

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
                        showDisabilityForm = false,
                        showButton = false,
                        amortizationSchedule = false
                    },
                    Context= undefined,
                    Cards= undefined,
                    Extrated=undefined,
                    Insights = undefined,
                    Audits = undefined,
                    AIWikiChat = undefined,
                    Guidance = undefined,
                    Cases = undefined,
                    InteractionHistory = undefined,
                    CallSummary = undefined,
                    ContactInfo = undefined,
                } = eventData

                if (IsPartial === true || IsPartial === undefined) {
                    return
                }

                let timeToSort = Date.now() + endTime * 1000
                let session = new Date().toISOString()

                if (Audits?.length !== 0) {
                    Audits?.forEach(audit => {
                        dispatch(updateCallAudits(audit))
                    })
                }

                if (Guidance && Guidance?.length !== 0) {
                    Guidance?.forEach(item => {
                        if (item.type === AI_ASSISTANT_TYPE.SPEECH_SUGGESTION) {
                            let gui_item = {
                                "type": item.type,
                                "entityName": item.name,
                                "entityValue": item.value,
                                "createdAt": session,
                                "user": USER_TYPE.CALLER
                            }

                            dispatch(setAiAssistantData(gui_item))
                        }

                        if (item.type === AI_ASSISTANT_TYPE.KNOWLEDGE_ARTICLE) {
                            let gui_item = {
                                "type": item.type,
                                "entityName": item.name,
                                "entityValue": item.value,
                                "createdAt": session
                            }

                            dispatch(setAiAssistantData(gui_item))
                        }

                        if (item.type === AI_ASSISTANT_TYPE.ACTION_WORKFLOW) {
                            if (!item.cardShown) {
                                dispatch(setAiAssistantData({
                                    "type": item?.type,
                                    "createdAt": new Date()
                                }))
                            } else {
                                if (item?.workflowType === 'detected') {
                                    dispatch(setDetectedWorkFlow({ "name": item?.name }))
                                }

                                if (item?.workflowType === 'completed') {
                                    let filterData = []

                                    call.detectedWorkFlow?.forEach((data) => {
                                        if (data?.name === item?.name) {
                                            dispatch(setCompletedWorkFlow(data));
                                        } else {
                                            filterData.push(data);
                                        }
                                    })

                                    dispatch(setDetectedWorkFlow(filterData))
                                    dispatch(setRunningWorkFlow(null))
                                }

                                if (item?.workflowType === 'running') {
                                    dispatch(setRunningWorkFlow(item?.name))
                                    dispatch(setActionWorkFlowData({
                                        "name": item?.name,
                                        "intentDetected": item?.intent,
                                        "totalSteps": item?.steps?.length,
                                        "steps": item?.steps
                                    }
                                    ))
                                }
                            }
                        }
                    })
                }

                if (Cases && Cases?.length !== 0) {
                    Cases?.forEach(item => {
                        dispatch(updateCases(item))
                    })
                }

                if (InteractionHistory && InteractionHistory?.length !== 0) {
                    InteractionHistory?.forEach(item => {
                        dispatch(updateInteractionHistory(item))
                    })
                }

                let trancriptObj = {
                    "user": ChannelId === 'ch_0' ? USER_TYPE.CALLER : USER_TYPE.AGENT,
                    "segmentId": ResultId,
                    utterance,
                    session,
                    "sentiment": Sentiment,
                    "sentimentScore": SentimentScore,
                    timeToSort
                }

                if (ChannelId === 'AGENT_ASSISTANT') {
                    let agentAssitObj = JSON.parse(utterance)
                    dispatch(setAiAssistantData({
                        "type": AI_ASSISTANT_TYPE.AI_GUIDANCE,
                        "createdAt": session,
                        "entityName": agentAssitObj.Title,
                        "entityValue": agentAssitObj.message,
                        "user": 'CALLER',
                        "showButton": showButton || false,
                        "amortizationSchedule": amortizationSchedule || false
                    }))
                } else {
                    dispatch(updateTranscriptData(trancriptObj))
                }

                if(Context){
                    dispatch(setContextDetails(Context))
                }
              
                if(Cards){
                    
                    dispatch(setCardDetails(Cards))
                }
                
                if(Extrated){
                    
                    dispatch(setExtratedDetails(Extrated))
                }

                if (Insights?.length !== 0) {
                    Insights?.forEach(insight => {
                        let insightData = {
                            "type": AI_ASSISTANT_TYPE.AI_GUIDANCE,
                            "createdAt": session,
                            "formData": [insight.answer, insight.question, insight.disabled],
                            "entityName": insight.entity,
                            "entityValue": insight.value,
                            "user": USER_TYPE.CALLER
                        }

                        dispatch(setAiAssistantData(insightData))
                    })
                }

                if (AIWikiChat && AIWikiChat?.length !== 0) {
                    AIWikiChat?.forEach(wiki => {
                        let wikiChat = {
                            "utterance": wiki?.utterance,
                            "user": wiki?.user === 'Agent Wiki' ? 'Agent Wiki' : USER_TYPE.AGENT,
                            "feedback": 0,
                            "time": new Date().toLocaleTimeString([], { hour: 'numeric', minute: 'numeric' })
                        }

                        setTimeout(() => {
                            dispatch(updateAIWikiConversation(wikiChat))
                        }, 0.5 * 1000)
                    })
                }

                if (ContactInfo) {
                    dispatch(setSimulationContactInfo(ContactInfo))
                }

            }, (transcript?.[currentIndex].EndTime - currentTime) * 1000)
        }
    }


    return (
        <div>
            {resources?.audio && (
                <ReactAudioPlayer
                    src={resources.audio}
                    ref={el => setAudioRef(el)}
                    onPlay={handleOnPlay}
                    onPause={handleOnPause}
                    onListen={handleOnListen}
                    onEnded={()=>{}}
                    autoPlay={false}
                    listenInterval={100}
                    onError={handleOnError}
                />
            )}

        </div>
    )
}

export default CallSimulatonPlayer
