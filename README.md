import { useState, useEffect } from 'react'
import { useSelector, useDispatch } from 'react-redux';
import { useRouter } from 'next/router';
import { selectClient } from "../../store/userSlice"
import {
    setAmortizationSchedule,
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
    setSimulationContactInfo
} from '../../store/callSlice';
import ReactAudioPlayer from 'react-audio-player';
import { BsFillPlayFill, BsPauseFill } from "react-icons/bs";
import { AI_ASSISTANT_TYPE, USER_TYPE, HEADERS } from '../../utility/constants'
import { changeSecondsToMinutes } from '../../utility'
import AndyDemoAudio from '../../assets/resource/andydemo.mp3'
import AndyDemoJson from '../../assets/resource/andydemo.json'
import PNCDemoAudio from '../../assets/resource/pnc_bank_demo.mp3'
import PNCDemoJson from '../../assets/resource/pnc_bank_demo.json'
import BNYMAudio from '../../assets/resource/bnym.mp3'
import BNYMJson from '../../assets/resource/bnym.json'
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
            if (clientJson) {
                clientJson === HEADERS.DEMO ? locale === "en" ? audioTranscriptInfo = ["transcript_en.json", "audio_en.mp3"] : audioTranscriptInfo = ["transcript_hi.json", "audio_hi.mp3"] : audioTranscriptInfo = ["transcript.json", "audio.mp3"];
                getStaticResource(audioTranscriptInfo[0], clientJson).then((data) => setTranscriptInfo(data))
                getStaticResource(audioTranscriptInfo[1], clientJson).then((data) => setAudioInfo(data))
            }
        } catch (error) { console.log(error) }
    }, [clientJson])

    useEffect(() => {
        if (transcriptInfo && audioInfo) {
            setResources({
                audio: audioInfo,
                transcript: transcriptInfo
            })
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
                        "user": 'CALLER'
                    }))
                } else {
                    dispatch(updateTranscriptData(trancriptObj))
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

    // function handleOnEnd() {
    //     let { CallSummary = undefined } = resources?.transcript?.[resources?.transcript.length - 1].eventData

    //     dispatch(updateCallSummary(CallSummary))
    //     dispatch(setIsCallConnected(false))
    //     dispatch(updateCallDisconnectTime({ disconnectTime: new Date() }))
    // }

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

            {/* {status === 'PAUSED' && (
                <BsFillPlayFill
                    className="cursor-pointer"
                    size={30}
                    onClick={() => handleOnPlay()}
                />
            )}

            {status === 'PLAY' && (
                <BsPauseFill
                    className="cursor-pointer"
                    size={30}
                    onClick={() => handleOnPause()}
                />
            )} */}

            {/* {`${changeSecondsToMinutes(audioRef?.audioEl?.current.currentTime)}/${changeSecondsToMinutes(audioRef?.audioEl?.current.duration)}`} */}
        </div>
    )
}

export default CallSimulatonPlayer
