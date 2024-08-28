import React, { useState, useEffect, useRef } from "react";
import { useDispatch, useSelector } from "react-redux";
import currentInstance from "./connectCTISession";
import { isArray } from "util";
import {
  ADD_TRANSCRIPT,
  SAVE_CONVERSATION,
  SEARCH_CUSTOMER_BY_TEXT,
  UPDATE_CONVERSATION,
  UPDATE_TRANSCRIPT,
  Update_Call_CustomSetting,
} from "../../graphql/userMicroservice/mutations";
import {
  setIsCallConnected,
  setIsCallRinging,
  setIsCallMuted,
  updateLiveTranscriptId,
  updateTranscriptData,
  updateConversationId,
  updateCallTime,
  updateCallDisconnectTime,
  updateAgentAHT,
  updateCallInsights,
  updateCallRecommendations,
  updateCallSummary,
  setAiAssistantData,
  setDetectedWorkFlow,
  updateTranscriptDataWithSentiment,
  setIsCallOnHold,
  setIsIdle,
  updateInteractionChannel,
  setIncomingAlertData,
  updateTranscriptSentiment,
  updateCallDuration,
  updateCallDurationString,
} from "../../store/callSlice";
import { useMutation } from "@apollo/client";
import { API, graphqlOperation } from "aws-amplify";
import onAddTranscriptSegment from "../../queries/onAddTranscriptSegment";
import onUpdateCall from "../../queries/onUpdateCall";
import { delay, parseResponse, smartAgentLog } from "../../utility";
import { updateCustomer } from "../../store/customerSlice";
import { updateCurrentCTIStatus } from "../../store/userSlice";
import axios from "axios";
import { GET_AGENT_AHT } from "../../graphql/userMicroservice/queries";
import client from "../../apollo-client";
import { useRouter } from "next/router";
import IncomingCallModal from "./IncomingCallModal";
import { useAppContext } from "../../components/common/AppContext";
import ChatProcessor from "./chatProcessor";

import {
  CTI_USER_STATUS,
  SHOW_INITIAL_RECOMMENDATION,
  INITIAL_RECOMMEDATION,
  AI_ASSISTANT_TYPE,
  USER_EVENTS,
  ACTION_TYPE,
  COMPONENT_NAME,
  CTI_PROVIDER,
  LOG_TYPE,
  GENESYS_EVENT_TYPES,
  GENESYS_INTERACTION_STATE,
  GENESYS_CALL_STATE,
  CTI_USER_STATUS_MAPPED_WITH_CTI,
  GENESYS_EVENT_CATEGORY_TYPE,
  INTERACTION_CHANNEL,
  STRING_CONSTANTS,
  ERROR_MESSAGES,
  SUCCESS_MESSAGES,
  USER_TYPE,
  CALL_WRAP_UP_STATE,
  PARTICIPANT_TYPE,
  AWS_CONNECT_CALL_STATE,
} from "../../utility/constants";

import {
  setAttachChatListenerForId,
  setChatConversations,
  setConversationIdToSelect,
  setIsRefetchTriggered,
  setTriggerDataUpdateForConversationId,
} from "../../store/conversationSlice";

import { checkForJsonInChat, handleChatRequestData } from "../../utility";
import { getVoiceCTIInstance } from "../adapter/adaptorFactory";
import { VoiceCTIAdapter } from "../adapter/voiceCTIAdapter";

const mapTranscriptSegmentValue = (transcriptSegmentValue) => {
  const {
    CallId: callId,
    SegmentId: segmentId,
    StartTime: startTime,
    EndTime: endTime,
    Transcript: transcript,
    IsPartial: isPartial,
    Channel: channel,
    CreatedAt: createdAt,
    Sentiment: sentiment,
    SentimentScore: sentimentScore,
    SentimentWeighted: sentimentWeighted,
  } = transcriptSegmentValue;

  return {
    callId,
    segmentId,
    startTime,
    endTime,
    transcript,
    isPartial,
    channel,
    createdAt,
    sentiment,
    sentimentScore,
    sentimentWeighted,
  };
};

const CTIComponent = () => {
  const cRef = useRef(null);
  const router = useRouter();
  const dispatch = useDispatch();
  const appCallData = useSelector((state) => state.call);
  const appUserData = useSelector((state) => state.user);
  const user = useSelector((state) => state.user);
  const [interactionIdForSubscription, setInteractionIdForSubscription] =
    useState(undefined);
  const [appInteractionId, setAppInteractionId] = useState("");
  const [transcriptArray, setTranscriptArray] = useState([]);
  const [isChat, setIsChat] = useState(false);
  const [segment, setSegment] = useState(null);
  const [currentLiveTranscriptionId, setCurrentLiveTranscriptionId] =
    useState("");
  const appConversationData = useSelector((state) => state.conversation);
  const [isFirstSegmentAddon, setIsFirstSegmentAddon] = useState(true);
  const [memberId, setMemberId] = useState("");
  const [requiredTranscriptLength, setRequiredTranscriptLength] = useState(0);
  const [conversationObject, setConversationObject] = useState({
    conversationId: "",
    callChannel: "",
  });
  const { userTrackingEvent } = useAppContext();
  const interactionChannel = appCallData.interactionChannel;
  // Get Configured CTI Information
  const configuredCTI = process.env.NEXT_PUBLIC_CONFIGURED_CTI;

  const getRandomArbitrary = (min, max) => {
    return Math.random() * (max - min) + min;
  };

  const getRandomScore = (min, max) => {
    return Math.floor(Math.random() * (max - min) + min);
  };

  const handleProcessCallLogEvent = (message) => {
    // message['lexAliasId']=process.env.LEX_BOT_ID;
    // message['lexBotId']=process.env.LEX_ALIAS_ID
    // write data to KDS in case of chat
    // setIsChat(message?.data?.interactionId?.isChat);
    if (message?.data?.interactionId?.isChat) {
      writeCallEventApi(message);
    }
    // update data with lex bot configuration
    if (message?.data?.interactionId?.isConnected == true) {
      // updateDbCustomSetting(message?.data?.interactionId?.id) // 2 times
    }
    if (
      message.data?.interactionId?.state === GENESYS_INTERACTION_STATE.CONNECTED
    ) {
      if (
        message.data?.interactionId?.id &&
        currentLiveTranscriptionId !== message.data?.interactionId?.id
      ) {
        if (message?.data?.interactionId?.isChat) {
          writeCallEventApi(message);
        }
        setIsFirstSegmentAddon(true);
        setCurrentLiveTranscriptionId(message.data?.interactionId?.id);
        dispatch(updateLiveTranscriptId(message.data?.interactionId?.id));
        setConversationObject({
          ...conversationObject,
          callChannel: message?.data?.interactionId?.isChat
            ? INTERACTION_CHANNEL.TEXT
            : INTERACTION_CHANNEL.VOICE,
          conversationId: message.data?.interactionId?.id,
          customerName: message.data?.interactionId?.name,
          ani: message.data?.interactionId?.ani,
        });
      }
    }
  };

  const handleInteractionMissed = (data) => {
    smartAgentLog(
      "handleInteractionMissed",
      STRING_CONSTANTS.INTERACTION_MISSED,
      data
    );
  };

  const initializeGenesysCTI = () => {
    if (typeof window !== undefined) {
      //** Function Definition to handle Events */

      const handleOnDisconnectedInteraction = (message) => {
        if (message?.data?.interaction?.interactionDurationSeconds) {
          handleCTICallEnd(
            message.data?.interaction?.id,
            message?.data?.interaction?.interactionDurationSeconds,
            message?.data?.interaction?.endTime
          );
        }
      };
      const handleOnConnectedInteraction = (message) => {
        // Chat Flow
        if (message.data?.interaction?.isChat) {
          handleChatConnected(message?.data?.interaction?.id);
        }
        // Voice Flow + Chat Flow
        handleCTICallConnected(
          message?.data?.interaction?.id,
          message.data.interaction?.connectedTime,
          message.data.interaction?.startTime
        );
      };

      const handleStateUpdateInteractionEvent = (
        connectedConversationId,
        oldState,
        newState
      ) => {
        if (newState == GENESYS_INTERACTION_STATE.HELD) {
          dispatch(setIsCallOnHold(true));
          dispatch(
            setChatConversations(
              handleChatRequestData(
                connectedConversationId,
                true,
                null,
                true,
                CALL_WRAP_UP_STATE.UNKNOWN
              )
            )
          );
        }
        if (newState == GENESYS_INTERACTION_STATE.IDLE) {
          dispatch(setIsIdle(true));
        }
        if (
          newState === GENESYS_INTERACTION_STATE.CONNECTED &&
          oldState == GENESYS_INTERACTION_STATE.HELD
        ) {
          dispatch(setIsCallOnHold(false));
          dispatch(
            setChatConversations(
              handleChatRequestData(
                connectedConversationId,
                true,
                null,
                false,
                CALL_WRAP_UP_STATE.UNKNOWN
              )
            )
          );
        }
      };

      const handleOnNotification = (message) => {
        if (
          message.type == GENESYS_EVENT_TYPES.NOTIFICATION_SUBSCRIPTION &&
          message?.data?.category == GENESYS_EVENT_TYPES.CHAT_UPDATE
        ) {
          // smartAgentLog("handleOnNotification", STRING_CONSTANTS.INCOMING_CHAT, message?.data?.data?.id);
          // STARTED SUPPORT FOR CHAT
          dispatch(
            setTriggerDataUpdateForConversationId(message?.data?.data?.id)
          );
        }
      };

      const phoneToggleEvent = (message) => {
        smartAgentLog(
          "eventListener.phoneToggle",
          STRING_CONSTANTS.PHONE_TOGGLE_DATA,
          message
        );
        if (
          message.data.interactionId.state == GENESYS_INTERACTION_STATE.ALERTING
        ) {
          if (message?.data?.interactionId?.isChat) {
            // smartAgentLog("eventListener.phoneToggleEvent", STRING_CONSTANTS.NO_SUPPORT_CHAT, message);
            // STARTED SUPPORT FOR CHAT
            setIsChat(message?.data?.interactionId?.isChat);
            // return;
          }
          dispatch(setIsCallRinging(true));
          dispatch(setIncomingAlertData(null));
          dispatch(
            setIncomingAlertData({
              customerName:
                message.data?.interactionId?.name ||
                message.data?.interaction?.name,
              ani:
                message.data?.interactionId?.ani ||
                message.data?.interaction?.ani,
              conversationId:
                message.data?.interactionId?.id ||
                message.data?.interaction?.id,
              queueName:
                message.data?.interactionId?.queueName ||
                message.data?.interaction?.queueName,
              isChat:
                message.data?.interactionId?.isChat ||
                message.data?.interaction?.isChat,
            })
          );
        }
      };
      const handleInteractionEvent = (message) => {
        smartAgentLog(
          "eventListener.handleInteractionEvent",
          STRING_CONSTANTS.INTERACTION_EVENT_DATA,
          message.data
        );
        if (
          message?.data?.interaction?.state ===
          GENESYS_INTERACTION_STATE.CONNECTED
        ) {
          handleOnConnectedInteraction(message);
        }
        if (
          message?.data?.interaction?.state ===
            GENESYS_INTERACTION_STATE.DISCONNECTED &&
          message.data.category === GENESYS_CALL_STATE.DISCONNECT
        ) {
          if (message?.data?.interaction?.isChat) {
            // smartAgentLog("eventListener.handleInteractionEvent", STRING_CONSTANTS.NO_SUPPORT_CHAT, message, LOG_TYPE.ERROR);
            setIsChat(message?.data?.interactionId?.isChat); //TODO CHECK USAGE
            // return;
          }
          handleOnDisconnectedInteraction(message);
          dispatch(setIncomingAlertData(null));
          dispatch(setIsCallRinging(false));
        }
        if (
          message.data.interaction?.new?.state !=
          message.data?.interaction?.old?.state
        ) {
          if (message?.data?.interaction?.isChat) {
            // smartAgentLog("eventListener.handleInteractionEvent", STRING_CONSTANTS.NO_SUPPORT_CHAT, message, LOG_TYPE.ERROR);
            setIsChat(message?.data?.interactionId?.isChat); //TODO CHECK USAGE
            // return;
          }
          handleStateUpdateInteractionEvent(
            message.data.interaction.id,
            message.data?.interaction?.old?.state,
            message.data.interaction?.new?.state
          );
        }
      };

      //** EventListener attachment for Genesys CTI Events */
      window.addEventListener("message", async function (event) {
        if (!event || !event?.data) {
          return;
        }
        try {
          var message = JSON.parse(event.data);
          smartAgentLog(
            "initializeGenesysCTI.eventListener",
            STRING_CONSTANTS.GENESYS_CTI_MESSAGE,
            message
          );
          const eventType = message?.type;
          if (!message || !eventType) {
            smartAgentLog(
              "initializeGenesysCTI.eventListener",
              ERROR_MESSAGES.CTI_MESSAGE_PARSING,
              message,
              LOG_TYPE.ERROR
            );
            return;
          }
          switch (eventType) {
            case GENESYS_EVENT_TYPES.PROCESS_CALL_LOG:
              handleProcessCallLogEvent(message);
              break;
            case GENESYS_EVENT_TYPES.SCREEN_POP:
              phoneToggleEvent(message);
              break;
            case GENESYS_EVENT_TYPES.INTERACTION_SUBSCRIPTION:
              try {
                var eventData = JSON.parse(event?.data);
                if (
                  eventData?.data?.interaction?.state ===
                    GENESYS_INTERACTION_STATE.ALERTING &&
                  eventData?.data?.category === GENESYS_EVENT_CATEGORY_TYPE.ADD
                ) {
                  const interactionId =
                    eventData?.data?.interaction?.old?.id ||
                    eventData?.data?.interaction?.id;
                  setInteractionIdForSubscription(interactionId);
                }
              } catch (e) {
                console.error(
                  `SmartAgent LogType Cti.index : updateInteractionState : ${ERROR_MESSAGES.CTI_MESSAGE_PARSING} `,
                  e
                );
              }
              handleInteractionEvent(message);
              break;
            case GENESYS_EVENT_TYPES.USER_ACTION_SUBSCRIPTION:
              if (
                message?.data?.category === GENESYS_EVENT_CATEGORY_TYPE.STATUS
              ) {
                updateUserActionStatus(message?.data?.data?.status);
              }
              if (
                message?.data?.category ===
                GENESYS_EVENT_CATEGORY_TYPE.ROUTING_STATUS
              ) {
                handleUserRoutingStatus(message?.data?.data);
              }
              break;
            case GENESYS_EVENT_TYPES.NOTIFICATION_SUBSCRIPTION:
              handleOnNotification(message);
              break;

            default:
              break;
          }
        } catch (e) {
          console.error(ERROR_MESSAGES.EVENT_LISTENER_ATTACHMENT, e);
          return;
        }
      });
    }
  };

  const initializeConnectCTI = () => {
    try {
      connect.core.initCCP(cRef.current, {
        ccpUrl: "https://cx-smartagent.my.connect.aws/connect/ccp-v2", // REQUIRED
        loginPopup: true, // optional, defaults to `true`
        loginPopupAutoClose: true, // optional, defaults to `false`
        loginOptions: {
          // optional, if provided opens login in new window
          autoClose: true, // optional, defaults to `false`
          height: 0, // optional, defaults to 578
          width: 0, // optional, defaults to 433
          top: 0, // optional, defaults to 0
          left: 0, // optional, defaults to 0
        },
        region: "us-east-1", // REQUIRED for `CHAT`, optional otherwise
        softphone: {
          // optional, defaults below apply if not provided
          allowFramedSoftphone: true, // optional, defaults to false
          disableRingtone: false, // optional, defaults to false
          //ringtoneUrl: "./ringtone.mp3" // optional, defaults to CCP’s default ringtone if a falsy value is set
        },
        pageOptions: {
          // optional
          enableAudioDeviceSettings: true, // optional, defaults to 'false'
          enablePhoneTypeSettings: true, // optional, defaults to 'true'
        },
        ccpAckTimeout: 5000, //optional, defaults to 3000 (ms)
        ccpSynTimeout: 3000, //optional, defaults to 1000 (ms)
        ccpLoadTimeout: 5000, //optional, defaults to 5000 (ms)
      });
      smartAgentLog("connect.core.initCCP", "CCP initialized!");
      connect.contact(function (contact) {
        smartAgentLog(
          "connect.contact",
          "Connect Contact initialized!",
          contact
        );
        const contactId = contact.contactId;
        currentInstance[contactId] = { contact };
        // currentInstance.contact = contact; //TODO : RATNESH Remove dependency
        if (
          contact.getActiveInitialConnection() &&
          contact.getActiveInitialConnection().getEndpoint()
        ) {
          smartAgentLog(
            "contact.getActiveInitialConnection",
            "New contact is from ",
            contact.getActiveInitialConnection().getEndpoint().phoneNumber
          );
        } else {
          smartAgentLog(
            "contact.getActiveInitialConnection",
            "This is an existing contact for this agent"
          );
        }
        smartAgentLog(
          "contact.getActiveInitialConnection",
          "Contact is from queue",
          contact.getQueue().name
        );
        smartAgentLog(
          "contact.getActiveInitialConnection",
          "Contact attributes are ",
          contact.getAttributes()
        );

        // Route to the respective handler
        contact.onIncoming(handleContactIncoming);
        contact.onAccepted(handleContactAccepted);
        contact.onConnecting(handleContactConnecting);
        contact.onConnected(handleContactConnected);
        contact.onEnded(handleContactEnded);
        contact.onMissed(handleMissedCall);
        contact.onDestroy(handleContactDestroyed);

        function handleMissedCall(contact) {
          smartAgentLog(
            "handleMissedCall",
            STRING_CONSTANTS.CALL_MISSED,
            contact
          );
          // Call handleInteractionMissed
        }

        function handleContactIncoming(contact) {
          smartAgentLog(
            "handleContactIncoming",
            STRING_CONSTANTS.CALL_INCOMING,
            contact
          );
        }

        async function handleContactAccepted(contact) {
          smartAgentLog(
            "handleContactAccepted",
            STRING_CONSTANTS.CALL_ACCEPTED,
            contact
          );
        }

        function handleContactConnecting(contact) {
          smartAgentLog(
            "handleContactConnecting",
            STRING_CONSTANTS.CALL_RINGING,
            contact
          );
          if (contact) {
            handleCTICallRingingForConnect(contact);
          }
        }

        function handleContactConnected(contact) {
          smartAgentLog(
            "handleContactConnected",
            STRING_CONSTANTS.CALL_CONNECTED,
            contact
          );
          // Set contactDetails key is not present in currentInstance
          if (!currentInstance.contactDetails)
            currentInstance.contactDetails = {};
          if (contact && contact?.getContactId()) {
            // For chat only
            const contactId = contact?.getContactId();
            if (contact.getType() === connect.ContactType.CHAT) {
              let contactDetails = [];
              contact.getConnections().forEach(async (cnn) => {
                const paticipantType =
                  cnn.getType() === PARTICIPANT_TYPE.INBOUND
                    ? PARTICIPANT_TYPE.CUSTOMER
                    : cnn.getType();
                contactDetails.push({ ...cnn, userType: paticipantType });
                if (cnn.getType() === connect.ConnectionType.AGENT) {
                  const agentSession = await cnn.getMediaController();
                  currentInstance[contactId] = {
                    ...currentInstance?.[contactId],
                    agentSession: agentSession,
                  };
                  handleChatConnected(contact?.getContactId());
                }
              });
              currentInstance.contactDetails[contactId] = contactDetails;
            }
            // For voice and chat both
            const state = contact.getState();
            // TODO Perform all checks
            handleCTICallConnected(
              contact?.getContactId(),
              contact?.getStateDuration(),
              state.timestamp?.toString()
            );
          }
        }
        function handleContactEnded(contact) {
          smartAgentLog(
            "handleContactEnded",
            STRING_CONSTANTS.CALL_ENDED,
            contact
          );
          // For chat only
          if (
            contact.getType() === connect.ContactType.CHAT &&
            contact?.getContactId()
          ) {
            handleChatDisconnected();
          }
          // const state = contact.getState();
          // smartAgentLog('handleContactEnded.contact?.getStateDuration()', STRING_CONSTANTS.CALL_ENDED, contact?.getStateDuration());
          // For voice and chat both
          // handleCTICallEnd(contact?.getContactId(), contact?.getStateDuration(), state.timestamp?.toString());
          contact?.clear();
        }

        function handleContactDestroyed(contact) {
          smartAgentLog(
            "handleContactDestroyed",
            STRING_CONSTANTS.CALL_DESTROYED,
            contact
          );
          const state = contact.getState();
          if (state.type === AWS_CONNECT_CALL_STATE.ENDED || state.type === AWS_CONNECT_CALL_STATE.DISCONNECTED) {
            handleCTICallEnd(contact?.getContactId(), contact?.contactData?.contactDuration, state.timestamp?.toString());
          }
        }
      });

      connect.agent(function (agent) {
        // Agent Session Event Subscription
        currentInstance.agent = agent;
        smartAgentLog(
          "connect.agent event Subscription attached!",
          `Agent ${agent.getName()} is ${agent.getStatus().name}.`,
          agent
        );
        agent.onRefresh(handleAgentRefresh);
        // agent.onStateChange(handleAgentStateChange);
        // agent.onRoutable(handleRoutable);
        agent.onNotRoutable(handleNotRoutable);
        agent.onOffline(handleAgentOffline);
        agent.onSoftphoneError(handleSoftphoneError);
        agent.onWebSocketConnectionLost(handleWebSocketConnectionLost);
        agent.onWebSocketConnectionGained(handleWebSocketConnectionGained);
        agent.onAfterCallWork(handleAfterCallWork);
        agent.onMuteToggle(handleAgentMuteToggle);

        // Agent handlers
        function handleAgentMuteToggle(agentMutedStatus) {
          smartAgentLog(
            "handleAgentMuteToggle",
            STRING_CONSTANTS.CALL_MUTED_TOGGLE,
            agentMutedStatus
          );
          handleMuteToggle(agentMutedStatus.muted);
        }

        function handleAgentRefresh(agent) {
          // smartAgentLog('handleAgentRefresh', "Call Agent Status on refresh", agent.getStatus().name);
          updateUserActionStatus(agent.getStatus().name);
        }

        function handleAgentStateChange(agent) {
          smartAgentLog(
            "handleAgentStateChange",
            STRING_CONSTANTS.CALL_AGENT_STATE,
            agent
          );
        }

        function handleRoutable(agent) {
          smartAgentLog(
            "handleRoutable",
            STRING_CONSTANTS.CALL_AGENT_LOG_ROUTING,
            agent
          );
          document
            .getElementById("goAvailableDiv")
            ?.classList?.remove("glowingButton");
        }

        function handleNotRoutable(agent) {
          smartAgentLog(
            "handleNotRoutable",
            STRING_CONSTANTS.CALL_AGENT_LOG_NOT_ROUTING,
            agent
          );
        }
        function handleAgentOffline(agent) {
          smartAgentLog(
            "handleAgentOffline",
            STRING_CONSTANTS.CALL_AGENT_LOG_OFFLINE,
            agent
          );
        }
        function handleSoftphoneError(agent) {
          smartAgentLog(
            "handleSoftphoneError",
            ERROR_MESSAGES.IN_SOFTPHONE,
            agent,
            LOG_TYPE.ERROR
          );
        }

        function handleWebSocketConnectionLost(agent) {
          smartAgentLog(
            "handleWebSocketConnectionLost",
            ERROR_MESSAGES.IN_WEB_SOCKET,
            agent,
            LOG_TYPE.ERROR
          );
        }

        function handleWebSocketConnectionGained(agent) {
          smartAgentLog(
            "handleWebSocketConnectionGained",
            SUCCESS_MESSAGES.SOCKET_CONNECTED,
            agent
          );
        }

        function handleAfterCallWork(agent) {
          smartAgentLog(
            "handleAfterCallWork",
            STRING_CONSTANTS.CALL_AGENT_LOG_ACW,
            agent
          );
        }
      });
    } catch (err) {
      smartAgentLog(
        "ConnectInitializer.useEffect",
        ERROR_MESSAGES.CCP_INITIATION,
        err,
        LOG_TYPE.ERROR
      );
    }
  };

  useEffect(() => {
    switch (configuredCTI) {
      case CTI_PROVIDER.AMAZON_CONNECT:
        initializeConnectCTI();
        break;
      case CTI_PROVIDER.GENESYS_PURE_CLOUD:
        initializeGenesysCTI();
        break;
      default:
        smartAgentLog(
          "cti.useEffect.ctiInitializer",
          ERROR_MESSAGES.UNKNOWN_CTI_REQUEST,
          configuredCTI,
          LOG_TYPE.ERROR
        );
        break;
    }
  }, [configuredCTI, appUserData.pureCloudAccessToken]);

  // /**Mutation and Queries*/

  const getAgentAHT = async () => {
    dispatch(updateAgentAHT(null));
    const [error, response] = await parseResponse(
      client.query({
        query: GET_AGENT_AHT,
        variables: { userId: appUserData.user?.oktaConfig?.id },
      })
    );
    if (error) {
      console.error(ERROR_MESSAGES.UPDATE_AHT, error);
      return;
    }
    dispatch(updateAgentAHT(response?.data?.getAgentAHT?.avgAHT));
  };

  // SAVE Conversation Call
  const saveConversation = async () => {
    const [error, response] = await parseResponse(
      client.mutate({
        mutation: SAVE_CONVERSATION,
        variables: {
          AddConversationInput: {
            referenceNo: "121658",
            userId: user.user?.oktaConfig.id,
            conversationId: conversationObject.conversationId,
            callChannel: conversationObject.callChannel,
            csat: getRandomArbitrary(71, 97)?.toString()?.slice(0, 2),
            caller: {
              callerName: conversationObject.customerName || "Unknown User",
              memberId: "",
              contactNo: conversationObject.ani,
              durationCallSeconds: 0,
              score: getRandomScore(20, 100),
            },
          },
        },
      })
    );
    if (error || !response) {
      console.error(ERROR_MESSAGES.SAVE_CONVERSATION, error);
    }
  };

  useEffect(() => {
    if (conversationObject.conversationId) {
      dispatch(updateConversationId(conversationObject.conversationId));
      saveConversation();
      getAgentAHT();
    }
  }, [conversationObject.conversationId]);

  useEffect(() => {
    if (
      transcriptArray.length > 0 &&
      requiredTranscriptLength === transcriptArray.length
    ) {
      addTranscriptMutationCall();
      transcriptArray.forEach((element) => {
        dispatch(updateTranscriptData(element));
      });
    }
  }, [transcriptArray]);

  // TODO Add Transcription Call Transfer Flow
  const [addTranscript] = useMutation(ADD_TRANSCRIPT, {
    variables: {
      AddTranscriptInput: {
        conversationId: conversationObject.conversationId,
        source: "EXELIA",
        transcript: transcriptArray,
      },
    },
  });

  // TODO  FIX THIS APPROACH
  const [addLiveTranscript] = useMutation(ADD_TRANSCRIPT, {
    variables: {
      AddTranscriptInput: {
        conversationId: currentLiveTranscriptionId,
        source: USER_TYPE.AGENT,
        transcript: segment,
      },
    },
  });
  // TODO FIX THIS APPROACH
  const [updateTranscript] = useMutation(UPDATE_TRANSCRIPT, {
    variables: {
      UpdateTranscriptInput: {
        conversationId: currentLiveTranscriptionId,
        source: USER_TYPE.AGENT,
        updateTranscriptObject: {
          transcript: segment,
        },
      },
    },
  });

  const addTranscriptMutationCall = async () => {
    const [addTranscriptError, addTranscriptResponse] = await parseResponse(
      addTranscript()
    );
    if (addTranscriptError) {
      console.error("addTranscriptError:  ", { ...addTranscriptError });
      return;
    }
  };

  const addLiveTranscriptMutationCall = async () => {
    const [addLiveTranscriptError, addLiveTranscriptResponse] =
      await parseResponse(addLiveTranscript());
    if (addLiveTranscriptError) {
      console.error("addLiveTranscriptError:  ", { ...addLiveTranscriptError });
      return;
    }
    if (addLiveTranscriptResponse) {
      setIsFirstSegmentAddon(false);
    }
  };

  const updateTranscriptMutationCall = async () => {
    if (isFirstSegmentAddon) {
      addLiveTranscriptMutationCall();
      return;
    }
    const [updateTranscriptError, updateTranscriptResponse] =
      await parseResponse(updateTranscript());
    if (updateTranscriptError) {
      console.error("updateTranscriptError:  ", { ...updateTranscriptError });
      return;
    }
  };

  const parseChatObject = (chatData) => {
    return {
      user: chatData.user || "",
      utterance: chatData.utterance || "",
      session: chatData.session || "",
    };
  };

  // code added on 10/25/23 to change subscription on chat change
  useEffect(() => {
    if (
      appConversationData.selectedConversation &&
      currentLiveTranscriptionId != appConversationData.selectedConversation?.id
    ) {
      setCurrentLiveTranscriptionId(
        appConversationData.selectedConversation?.id
      );
    }
  }, [appConversationData.selectedConversation?.id]);

  useEffect(() => {
    // Live call segment updates
    if (segment) {
      if (interactionChannel === INTERACTION_CHANNEL.CHAT) {
        dispatch(updateTranscriptSentiment(segment));
      } else {
        setTranscriptArray([]);
        updateTranscriptMutationCall();
        if (segment["sentiment"]) {
          // As without sentiment and with sentiment come pretty fast, redux is dispatching the first one but not the second. So keeping some delay before dispatching the second.
          delay(100).then(() => {
            dispatch(updateTranscriptData(segment));
          });
        } else {
          dispatch(updateTranscriptData(segment));
        }
      }
    }
  }, [segment]);

  /* Customer Info extraction*/

  const [searchCustomerByText] = useMutation(SEARCH_CUSTOMER_BY_TEXT, {
    variables: {
      SearchCustomerInput: {
        searchText: memberId,
      },
    },
  });

  const getCustomerData = async () => {
    const [customerSearchError, customerSearchResultData] = await parseResponse(
      searchCustomerByText()
    );
    if (customerSearchError) {
      console.error("Customer Search Error", customerSearchError);
      return;
    }
    if (customerSearchResultData?.data) {
      dispatch(
        updateCustomer(customerSearchResultData.data?.searchCustomerByText[0])
      );
      setMemberId("");
    }
  };

  useEffect(() => {
    if (memberId) {
      getCustomerData();
    }
  }, [memberId]);

  // /* Forwarded call transcript */

  const getTranscript = async (transcriptId) => {
    if (!transcriptId) {
      console.error("Unable to fetch Transcript Data!");
      return;
    }
    const headers = {
      Authorization: process.env.AWS_SERVER_AUTH_CODE,
    };
    const [error, response] = await parseResponse(
      axios.get(
        `${process.env.AWS_SERVER_URL}/Dev/transcript/${transcriptId}`,
        { headers }
      )
    );

    if (error) {
      console.error("getTranscript error", error);
      return;
    }
    if (response) {
      try {
        const transcript = JSON.parse(response.responseData?.transcript);
        const conversation = transcript?.transcript;
        const memberIdentifier = transcript?.contextualInsights?.MemberID;
        setMemberId(memberIdentifier);
        const chatArray = [];
        conversation.forEach((item) => {
          if (item) {
            chatArray.push(parseChatObject(item));
          }
        });
        setRequiredTranscriptLength(chatArray.length);
        setTranscriptArray((prev) => [...prev, ...chatArray]);
      } catch (e) {
        console.error("Unable to parse transcript", e);
      }
    }
  };

  const updateDbCustomSetting = async (callId) => {
    try {
      const updateCustomSettingMutation = await API.graphql({
        query: Update_Call_CustomSetting,
        variables: {
          input: {
            CallId: callId,
            CustomSettings: {
              LexBotId: process.env.LEX_BOT_ID,
              LexAliasId: process.env.LEX_ALIAS_ID,
            },
            UpdatedAt: new Date().toISOString(),
          },
        },
      });

      console.log(
        "Update Custom Setting Mutation Response : ",
        updateCustomSettingMutation
      );
    } catch (error) {
      console.log("ERROR : ", error);
    }
  };

  useEffect(() => {
    let lastSegment = null;
    let subscription;

    if (!currentLiveTranscriptionId) {
      // the component should set the live transcript contact id to null to unsubscribe
      if (subscription?.unsubscribe) {
        subscription.unsubscribe();
      }
      return () => {};
    }

    if (SHOW_INITIAL_RECOMMENDATION) {
      INITIAL_RECOMMEDATION.forEach((recommendationData) => {
        dispatch(
          updateCallRecommendations({
            ...recommendationData,
            createdAt: new Date(),
          })
        );
      });
    }

    subscription = API.graphql(
      graphqlOperation(onAddTranscriptSegment, {
        callId: currentLiveTranscriptionId,
      })
    ).subscribe({
      next: async ({ provider, value }) => {
        const transcriptSegmentValue = value?.data?.onAddTranscriptSegment;
        if (!transcriptSegmentValue) {
          return;
        }
        const transcriptSegment = mapTranscriptSegmentValue(
          transcriptSegmentValue
        );
        const {
          callId,
          transcript,
          sentiment,
          sentimentScore,
          segmentId,
          isPartial,
          channel,
          createdAt,
        } = transcriptSegment;

        if (
          callId !== currentLiveTranscriptionId
          // || (sentiment == null && channel != "AGENT_ASSISTANT")
        ) {
          return;
        }
        if (transcript && segmentId) {
          const transcriptObject = {
            user: channel,
            utterance: transcript,
            session: createdAt,
            sentiment: sentiment,
            segmentId: segmentId,
            sentimentScore: sentimentScore,
          };
          // if (
          //   transcriptObject.user === lastSegment?.user &&
          //   lastSegment?.utterance === transcriptObject?.utterance
          // ) {
          //   return;
          // }

          // For CHAT data should not be updated or written in DB but data should be written on DB
          // for Live Transcript flow
          if (
            //!isChat &&
            transcriptObject.user == "CALLER" ||
            transcriptObject.user == "AGENT"
          ) {
            setSegment(transcriptObject);
            lastSegment = transcriptObject;
          }
          // For CHATS & CALLS, Process Guidance should be added.
          if (transcriptObject.user == "AGENT_ASSISTANT") {
            smartAgentLog(
              "onAddTranscriptSegment.AGENT_ASSISTANT",
              "AGENT_ASSISTANT RESPONDING!!!",
              transcriptObject
            );
            if (transcriptObject?.utterance.includes("NER:")) {
              const utteranceData = transcriptObject?.utterance
                .replace("NER:", "")
                .split(":");
              const value = utteranceData[1] ? utteranceData[1].trim() : "";
              const entity = utteranceData[0] ? utteranceData[0].trim() : "";
              const nerData = {
                value: value,
                type: entity,
                actor: "CALLER",
                entity: entity,
                createdAt: transcriptObject?.session,
              };
              dispatch(updateCallInsights(nerData));
              if (utteranceData.length > 2) {
                const value = utteranceData[3] ? utteranceData[3].trim() : "";
                const entity = utteranceData[2] ? utteranceData[2].trim() : "";
                const nerData = {
                  value: value,
                  type: entity,
                  actor: "CALLER",
                  entity: entity,
                  createdAt: transcriptObject?.session,
                };
                dispatch(updateCallInsights(nerData));
              }
            } else {
              if (checkForJsonInChat(transcriptObject?.utterance)) {
                let agentAssistObj = JSON.parse(
                  transcriptObject?.utterance.replace()
                );
                if (isArray(agentAssistObj)) {
                  agentAssistObj.forEach((data) => {
                    if (data?.Title === "NER") {
                      const utteranceData = data?.message.split(":");
                      const value = utteranceData[1]
                        ? utteranceData[1].trim()
                        : "";
                      const entity = utteranceData[0]
                        ? utteranceData[0].trim()
                        : "";
                      const nerData = {
                        value: value,
                        type: entity,
                        actor: "CALLER",
                        entity: entity,
                        createdAt: transcriptObject?.session,
                      };
                      dispatch(updateCallInsights(nerData));
                    } else {
                      console.log("data from bot", data);
                      if (data.prompt) {
                        dispatch(
                          setAiAssistantData({
                            type: AI_ASSISTANT_TYPE.SPEECH_SUGGESTION,
                            name: "Speech Suggestions",
                            entityValue: data.prompt,
                            createdAt: transcriptObject?.session || new Date(),
                          })
                        );
                      } else if (
                        data.type &&
                        data.type === AI_ASSISTANT_TYPE.KNOWLEDGE_ARTICLE
                      ) {
                        dispatch(
                          setAiAssistantData({
                            type: data.type,
                            name: data.name,
                            entityValue: data.value,
                            createdAt: transcriptObject?.session || new Date(),
                          })
                        );
                      } else if (
                        data.type &&
                        data.type === AI_ASSISTANT_TYPE.ACTION_WORKFLOW
                      ) {
                        dispatch(
                          setAiAssistantData({
                            type: data.type,
                            createdAt: transcriptObject?.session || new Date(),
                          })
                        );
                      } else if (data.botId) {
                        const actionDetail = {
                          component: "ActionWorkFlow",
                          actionType: ACTION_TYPE.DETECTED,
                          name: data?.intentDetected,
                        };
                        userTrackingEvent(
                          COMPONENT_NAME.CTI_COMPONENT,
                          USER_EVENTS.CLICK,
                          actionDetail
                        );
                        dispatch(
                          setDetectedWorkFlow({
                            botId: data?.botId,
                            aliasId: data?.aliasId,
                            name: data?.intentDetected,
                            createdAt: transcriptObject?.session || new Date(),
                          })
                        );
                      } else {
                        dispatch(
                          setAiAssistantData({
                            type: AI_ASSISTANT_TYPE.AI_GUIDANCE,
                            createdAt: transcriptObject?.session || new Date(),
                            entityName: data?.Title,
                            entityValue: data?.message,
                          })
                        );
                      }
                    }
                  });
                } else {
                  if (agentAssistObj.prompt) {
                    dispatch(
                      setAiAssistantData({
                        type: AI_ASSISTANT_TYPE.SPEECH_SUGGESTION,
                        name: "Speech Suggesstions",
                        entityValue: agentAssistObj.prompt,
                        createdAt: transcriptObject?.session || new Date(),
                      })
                    );
                  } else if (
                    agentAssistObj.type &&
                    agentAssistObj.type === AI_ASSISTANT_TYPE.KNOWLEDGE_ARTICLE
                  ) {
                    dispatch(
                      setAiAssistantData({
                        type: agentAssistObj.type,
                        name: agentAssistObj.name,
                        entityValue: agentAssistObj.value,
                        createdAt: transcriptObject?.session || new Date(),
                      })
                    );
                  } else if (
                    agentAssistObj.type &&
                    agentAssistObj.type === AI_ASSISTANT_TYPE.ACTION_WORKFLOW
                  ) {
                    dispatch(
                      setAiAssistantData({
                        type: agentAssistObj.type,
                        createdAt: transcriptObject?.session || new Date(),
                      })
                    );
                  } else if (agentAssistObj.botId) {
                    dispatch(
                      setDetectedWorkFlow({
                        botId: agentAssistObj?.botId,
                        aliasId: agentAssistObj?.aliasId,
                        name: agentAssistObj?.intentDetected,
                        createdAt: transcriptObject?.session || new Date(),
                      })
                    );
                  } else {
                    dispatch(
                      setAiAssistantData({
                        type: AI_ASSISTANT_TYPE.AI_GUIDANCE,
                        createdAt: transcriptObject?.session || new Date(),
                        entityName: agentAssistObj?.Title,
                        entityValue: agentAssistObj?.message,
                      })
                    );
                  }
                }
              }
            }
          }
        }
      },
      error: (err) => {
        console.error(
          "transcript update network subscription failed - please reload the page. Error :  ",
          err
        );
      },
    });

    return () => {
      subscription.unsubscribe();
    };
  }, [currentLiveTranscriptionId]);

  const callUpdateConversationMutation = async (
    conversationId,
    timeDurationCall
  ) => {
    let updateObj = {
      conversationId,
      updateConversationObject: {
        caller: {
          durationCallSeconds: parseInt(timeDurationCall),
        },
      },
    };

    const [error, response] = await parseResponse(
      client.mutate({
        mutation: UPDATE_CONVERSATION,
        variables: {
          UpdateConversationInput: updateObj,
        },
      })
    );
    if (error) {
      console.log("Call Update for Time Failed:  error : ", error);
      return;
    }
    if (response) {
      console.log("Call Update for Time Successful");
      return;
    }
  };

  const onCallDisconnectActivity = (conversationId, timeDuration) => {
    callUpdateConversationMutation(conversationId, timeDuration);
  };

  useEffect(() => {
    const isSummaryViaMutationActivated =
      process.env.CALL_SUMMARY_VIA_MUTATION_ACTIVATED?.toLowerCase() === "true";
    if (isSummaryViaMutationActivated) {
      let subscription;
      if (!currentLiveTranscriptionId) {
        // the component should set the live call update contact id to null to unsubscribe
        if (subscription?.unsubscribe) {
          subscription.unsubscribe();
        }
        return () => {};
      }

      subscription = API.graphql(
        graphqlOperation(onUpdateCall, {
          callId: currentLiveTranscriptionId,
        })
      ).subscribe({
        next: async ({ provider, value }) => {
          const callUpdate = value?.data?.onUpdateCall;
          try {
            if (
              callUpdate.Status === "ENDED" &&
              callUpdate.CallSummaryText &&
              !appCallData.callSummary
            ) {
              dispatch(updateCallSummary(callUpdate.CallSummaryText));
            }
          } catch (e) {
            console.error(
              "Error : subscription : callUpdate:  while Call Summary Parsing!!! ",
              e
            );
          }
        },
        error: (err) => {
          console.error(
            "Call Summary update network subscription failed - please reload the page. Error :  ",
            err
          );
        },
      });

      return () => {
        subscription.unsubscribe();
      };
    }
  }, [currentLiveTranscriptionId]);

  const writeCallEventApi = (message) => {
    let data = JSON.stringify(message);

    let config = {
      method: "post",
      maxBodyLength: Infinity,
      url: process.env.LCA_BACKEND_URL_CALL_EVT,
      headers: {
        "Content-Type": "application/json",
      },
      data: data,
    };

    axios
      .request(config)
      .then((response) => {
        if (response?.data?.status == 200) {
          console.log("Writing KDS event successful");
        } else if (response?.data?.status == 400) {
          console.log("Writing to KDS event failed");
        }
      })
      .catch((error) => {
        console.log(error);
      });
  };

  const handleUserRoutingStatus = (routingStatus) => {
    if (routingStatus === CTI_USER_STATUS.NOT_RESPONDING) {
      const configuredCti = getVoiceCTIInstance();
      const voiceAdaptor = new VoiceCTIAdapter(configuredCti);
      if (!configuredCti || !voiceAdaptor) {
        console.error("[SmartAgent] routingStatus.config error");
        return;
      }
      voiceAdaptor.setUserStatus(
        CTI_USER_STATUS_MAPPED_WITH_CTI.GENESYS_PURE_CLOUD.BUSY
      );
      setTimeout(() => {
        voiceAdaptor.setUserStatus(
          CTI_USER_STATUS_MAPPED_WITH_CTI.GENESYS_PURE_CLOUD.ON_QUEUE
        );
      }, 1500);
    }
  };

  const updateUserActionStatus = (userStatus) => {
    switch (userStatus) {
      case CTI_USER_STATUS_MAPPED_WITH_CTI.AMAZON_CONNECT.AVAILABLE:
      case CTI_USER_STATUS_MAPPED_WITH_CTI.GENESYS_PURE_CLOUD.ON_QUEUE:
        if (CTI_USER_STATUS.ON_QUEUE === appUserData.currentCTIStatus) {
          // smartAgentLog('updateUserActionStatus', "No change in User State!", { previousState: CTI_USER_STATUS.ON_QUEUE, newState: userStatus });
          return;
        }
        dispatch(updateCurrentCTIStatus(CTI_USER_STATUS.ON_QUEUE));
        break;
      case CTI_USER_STATUS_MAPPED_WITH_CTI.AMAZON_CONNECT.BUSY:
      case CTI_USER_STATUS_MAPPED_WITH_CTI.GENESYS_PURE_CLOUD.BUSY:
      case CTI_USER_STATUS_MAPPED_WITH_CTI.GENESYS_PURE_CLOUD.AWAY:
      case CTI_USER_STATUS_MAPPED_WITH_CTI.GENESYS_PURE_CLOUD.NOT_RESPONDING:
      case CTI_USER_STATUS_MAPPED_WITH_CTI.GENESYS_PURE_CLOUD.AVAILABLE:
      case CTI_USER_STATUS_MAPPED_WITH_CTI.GENESYS_PURE_CLOUD.OFF_QUEUE:
        if (CTI_USER_STATUS.BUSY === appUserData.currentCTIStatus) {
          // smartAgentLog('updateUserActionStatus', "No change in User State!", { previousState: CTI_USER_STATUS.BUSY, newState: userStatus });
          return;
        }
        dispatch(updateCurrentCTIStatus(CTI_USER_STATUS.BUSY));
        break;
      default:
        break;
    }
  };

  const handleUpdateCallTimer = (connectedTime, startTime) => {
    console.log(connectedTime, startTime,"test444")
    dispatch(
      updateCallTime({
        connectedTime,
        startTime,
      })
    );
  };

  const handleOnCTICallConnectionAppTimerAction = (
    connectedConversationId,
    callConnectTime,
    callStartTime
  ) => {
    setAppInteractionId(connectedConversationId);
    dispatch(setIsCallConnected(true));
    if (configuredCTI === CTI_PROVIDER.GENESYS_PURE_CLOUD) {
      handleUpdateCallTimer(callConnectTime, callStartTime);
    }
    if (configuredCTI === CTI_PROVIDER.AMAZON_CONNECT) {
      handleUpdateCallTimer(
        new Date(Date.now() + callConnectTime).toString(),
        callStartTime
      );
    }
    dispatch(setTriggerDataUpdateForConversationId(connectedConversationId));
    router.push("/agent/conversation-details");
  };

  const handleOnCTICallConnectionInitialAction = (connectedConversationId) => {
    if (connectedConversationId != currentLiveTranscriptionId) {
      setIsFirstSegmentAddon(true);
      setCurrentLiveTranscriptionId(connectedConversationId);
      dispatch(updateLiveTranscriptId(connectedConversationId));
      updateDbCustomSetting(connectedConversationId);
      setConversationObject({
        ...conversationObject,
        callChannel: appConversationData.selectedConversation?.id
          ? INTERACTION_CHANNEL.TEXT
          : INTERACTION_CHANNEL.VOICE,
        conversationId: connectedConversationId,
      });
    }
  };

  const handleCTICallConnected = (
    connectedConversationId,
    callConnectedTime,
    callStartTime = new Date()
  ) => {
    setAppInteractionId(connectedConversationId);
    dispatch(setIsCallConnected(true));
    handleOnCTICallConnectionInitialAction(connectedConversationId);
    handleOnCTICallConnectionAppTimerAction(
      connectedConversationId,
      callConnectedTime,
      callStartTime
    );
    // // TODO
    // dispatch(setChatConversations(
    //   handleChatRequestData(connectedConversationId, true, null, false, CALL_WRAP_UP_STATE.UNKNOWN)
    // ))
  };

  const handleMuteToggle = (isMuted) => {
    dispatch(setIsCallMuted(isMuted));
  };

  const handleCTICallEnd = (connectedConversationId, callDuration, disconnectTime) => {
    smartAgentLog('handleCTICallEnd', STRING_CONSTANTS.CALL_ENDED, {connectedConversationId, callDuration, disconnectTime});
    dispatch(setIsCallConnected(false));
    dispatch(
      updateCallDisconnectTime({
        disconnectTime: disconnectTime || new Date(),
      })
    );
    dispatch(updateCallDuration(callDuration));
    dispatch(updateCallDurationString(`${callDuration}`));
    onCallDisconnectActivity(connectedConversationId, callDuration);
    dispatch(
      setChatConversations(
        handleChatRequestData(
          connectedConversationId,
          false,
          disconnectTime,
          false,
          CALL_WRAP_UP_STATE.NOT_SUBMITTED
        )
      )
    );
  };

  const handleCTICallRingingForConnect = (contact) => {
    setInteractionIdForSubscription(contact?.getContactId());
    dispatch(setIsCallRinging(true));
    dispatch(setIncomingAlertData(null));
    console.log("[ SmartAgent ] contact", contact);
    console.log(
      "[ SmartAgent ] contact getDescription",
      contact.getConnections()
    );
    dispatch(
      setIncomingAlertData({
        customerName: "",
        conversationId: contact.getContactId(),
        queueName: "",
        isChat: contact?.getType() === connect.ContactType.CHAT,
      })
    );
  };

  const handleChatConnected = (interactionId) => {
    // Add After Chat Connected event methods...
    setIsChat(true);
    dispatch(setConversationIdToSelect(interactionId)); // Update Conversation List
    dispatch(setIsRefetchTriggered(true)); // Update Conversation List
    dispatch(updateInteractionChannel(INTERACTION_CHANNEL.CHAT));
    dispatch(
      setChatConversations(
        handleChatRequestData(
          interactionId,
          true,
          null,
          false,
          CALL_WRAP_UP_STATE.UNKNOWN
        )
      )
    );
    if (configuredCTI === CTI_PROVIDER.AMAZON_CONNECT) {
      // Trigger Add listener for getting ongoing messages
      dispatch(setAttachChatListenerForId(interactionId));
    }
  };
  const handleChatDisconnected = () => {
    dispatch(setIsCallRinging(false));
    dispatch(setIncomingAlertData(null));
  };

  return (
    <>
      {configuredCTI === CTI_PROVIDER.AMAZON_CONNECT && (
        <div ref={cRef} className="softphone" style={{ display: "none" }} />
      )}
      {configuredCTI === CTI_PROVIDER.GENESYS_PURE_CLOUD &&
        appUserData.pureCloudAccessToken && (
          <>
            <div className="softphone">
              <iframe
                hidden
                id="softphone"
                allow="camera *; microphone *; autoplay *; hid *"
                src="https://apps.usw2.pure.cloud/crm/embeddableFramework.html?dedicatedLoginWindow=false&orgName=exl-cx-coe&provider=okta"
              ></iframe>
            </div>
          </>
        )}
      <ChatProcessor />
      <IncomingCallModal
        interactionIdForSubscription={interactionIdForSubscription}
        appInteractionId={appInteractionId}
      />
    </>
  );
};

export default CTIComponent;



import { useEffect, useRef, useState } from "react";
import { useDispatch, useSelector } from "react-redux";
import { useRouter } from "next/router";
import Image from "next/future/image";
import CallSummarySubmitModal from "../../../components/agent/modals/CallSummarySubmitModal";
import {
  epochToTimeFormat,
  handleChatRequestData,
  parseResponse,
  smartAgentLog,
} from "../../../utility";
import client from "../../../apollo-client";
import { UPDATE_CONVERSATION } from "../../../graphql/userMicroservice/mutations";
import Loading from "../../../components/common/Loading";
import CustomerDetails from "../../../assets/resource/static.json";
import PopupController from "../../../components/common/popup";
import CALL_SUMMARY_ICON from "../../../assets/img/agent/call-summary-logo.svg";
import { updateCallSummary, updateCallSummaryNotes } from "../../../store/callSlice";
import {
  SUMMARY_INTENT,
  CALL_SUMMARY_RESOLUTION_QUESTIONS,
  CALL_SUMMARY_BUTTONS,
  CALL_SUMMARY_CALL_INFO,
  CALL_SUMMARY_LABELS,
  CALL_SUMMARY_SUB_HEADINGS,
  ACTION_TYPE,
  COMPONENT_NAME,
  LOG_TYPE,
  CTI_PROVIDER,
  SUCCESS_MESSAGES,
  ERROR_MESSAGES,
  KEY_STROKE,
  CALL_WRAP_UP_STATE,
  INTERACTION_CHANNEL,
} from "../../../utility/constants";
import { useAppContext } from "../../common/AppContext";
import config from "../../../scripts/purecloud/config";

const platformClient = require("purecloud-platform-client-v2/dist/node/purecloud-platform-client-v2.js");
import {
  setChatConversations,
  setConversationIdCommunicationIdMap,
  updateConversationIdCommunicationIdObject,
} from "../../../store/conversationSlice";
import currentInstance from "../../cti/connectCTISession";

interface Data {
  description: string;
}

const NewCallSummary = ({ summaryText, loader, isCallWrappingFlow }) => {
  const router = useRouter();
  const [openSubmitModal, setOpenSubmitModal] = useState(false);
  const [intents, setIntents] = useState([]);
  const appUser = useSelector((state: any) => state.user);
  const callerCustomer = useSelector((state: any) => state.customer);
  const appCallData = useSelector((state: any) => state.call);
  const appConversationData = useSelector((state: any) => state.conversation);
  const [initialNotes, setInitialNotes] = useState([summaryText]);
  const [loader2, setLoader2] = useState(false);
  // const [isLoading, setIsLoading] = useState(true);
  const [customerData, setCustomerData] = useState<any>({});
  const dispatch = useDispatch();
  const [focusedInput, setFocusedInput] = useState(null);
  const [isAccountNumberUpdated, setIsAccountNumberUpdated] = useState(false);
  const [showPopUp, setShowPopUp] = useState(false);
  const { userTrackingEvent } = useAppContext();
  const interactionChannel = appCallData.interactionChannel;
  const configuredCTI = process.env.NEXT_PUBLIC_CONFIGURED_CTI;
  const interactionId = ((interactionChannel === INTERACTION_CHANNEL.CHAT) ? 
    appConversationData.selectedConversation?.id : appCallData.liveTranscriptionId);
  const appWorkFlowData = appCallData.workFlowData;
  const lastIntent = Object.values(appWorkFlowData)?.[Object.values(appWorkFlowData).length -1]?.intentDetected || "";
  const callSummaryNotes = appCallData?.callSummaryNotes?.[interactionId]?.historyNotes;
  const [historyNotes, setHistoryNotes] = useState<Data[]>(JSON.parse(JSON.stringify(callSummaryNotes || [])));
  const [newContent, setNewContent] = useState("");
  const [selectedValues, setSelectedValues] = useState({
    isFirstIntentCorrect: true,
    isIssueResolved: true,
    isCallWrapped: true,
  });

  const [answers, setAnswers] = useState(
    CALL_SUMMARY_RESOLUTION_QUESTIONS?.map((q) => q.DEFAULT_VALUE)
  );
    
  useEffect(() => {
    if(appWorkFlowData){
      Object.keys(appWorkFlowData).map((workflow) => {
        const detectedIntent = appWorkFlowData[workflow]?.intentDetected
        setIntents(prev => [...prev, detectedIntent]);
      });
    }
  }, [appWorkFlowData])
  

  const [notesSelection, setNotesSelection] = useState<{
    selected: string[];
    unSelected: string[];
  }>({
    selected: [],
    unSelected: [],
  });

  useEffect(() => {
    if (!appUser.isAuthenticated) {
      router.push("/login");
    }
  }, []);

  const wrapCallForGenesys = (mutationParameters) => {
    const callId = mutationParameters.conversationId;
    const client = platformClient.ApiClient.instance;
    client.setEnvironment(config.genesysCloud.region); // Genesys Cloud region
    client.setAccessToken(appUser.pureCloudAccessToken);
    let conversationsApi = new platformClient.ConversationsApi();
    conversationsApi
      .getConversation(callId)
      .then((getConversationResponse) => {
        const participant = getConversationResponse?.participants?.filter(
          (participantData) => participantData.purpose === "agent"
        );
        if (participant.length && participant[0]?.id) {
          let conversationId = callId;
          let participantId = participant[0]?.id;
          const notesInput =
            mutationParameters.agentNotes?.toString() || "Default Summary";
          let requestData = {
            wrapup: {
              code: "dc8bfca7-8961-4a06-b502-73727c57cb9c", // SmartAgentWrapUp WrapUp Code
              notes: notesInput,
            },
          };

          // Update conversation participant on PureCloud
          conversationsApi
            .patchConversationsCallParticipant(
              conversationId,
              participantId,
              requestData
            )
            .then(() => {
              setLoader2(false);
              console.log(SUCCESS_MESSAGES.SUBMIT_CALL_SUMMARY);
              // Update conversation in DB
              callUpdateConversationMutation(mutationParameters).catch(
                (err) => console.error(ERROR_MESSAGES.SUBMIT_CALL_SUMMARY, err)
                // smartAgentLog("callSummaryUpdate.useEffect", "Error while submitting Call Summary", err, LOG_TYPE.ERROR);
              );
            })
            .catch((err) => {
              setLoader2(false);
              console.error(ERROR_MESSAGES.SUBMIT_CALL_WRAP, err);
            });
        }
      })
      .catch((error) => {
        setLoader2(false);
        console.error(ERROR_MESSAGES.SUBMIT_CALL_WRAP, error);
        return;
      });
  };

  const callUpdateConversationMutation = async (mutationParameters) => {
    setLoader2(true);
    const {
      conversationId,
      agentNotes,
      firstIntent,
      isFirstIntentCorrect,
      isIssueResolved,
      isCallWrapped,
    } = mutationParameters;
    let wrapObj = {
      conversationId,
      updateConversationObject: {
        wrapup: {
          agentNotes,
          firstIntent,
          isFirstIntentCorrect,
          isIssueResolved,
          isCallWrapped,
        },
      },
    };

    let normalObj = {
      conversationId,
      updateConversationObject: {
        aht: parseInt(appCallData.totalCallDuration),
        wrapup: {
          agentNotes,
          firstIntent,
          isFirstIntentCorrect,
          isIssueResolved,
          isCallWrapped,
        },
        caller: {
          durationCallSeconds: parseInt(appCallData.totalCallDuration),
        },
      },
    };

    let updateConvObject;

    if (isCallWrappingFlow) {
      updateConvObject = wrapObj;
    } else {
      updateConvObject = normalObj;
    }
    const [error, response] = await parseResponse(
      client.mutate({
        mutation: UPDATE_CONVERSATION,
        variables: {
          UpdateConversationInput: updateConvObject,
        },
      })
    );

    if (error) {
      console.error(ERROR_MESSAGES.UPDATE_CONVERSATION, error);
      setLoader2(false);
      return;
    }
    if (response) {
      if (appCallData.interactionChannel === INTERACTION_CHANNEL.CHAT) {
        dispatch(
          setChatConversations(
            handleChatRequestData(
              interactionId,
              false,
              true,
              false,
              CALL_WRAP_UP_STATE.SUBMITTED
            )
          )
        );
        dispatch(
          setChatConversations({
            conversationId: interactionId,
            deleteConversationData: true,
          })
        );
        // INCASE of CONNECT: Remove Contact object as well!
        if (configuredCTI === CTI_PROVIDER.AMAZON_CONNECT) {
          delete currentInstance?.contactDetails?.[interactionId];
          const communicationMap = Object.assign(
            {},
            appConversationData.conversationIdCommunicationIdMap
          );
          delete communicationMap?.[interactionId];
          dispatch(updateConversationIdCommunicationIdObject(communicationMap));
        }
      }
      setLoader2(false);
      router.push("/agent");
    }
  };

  const handleCallSubmit = async () => {
    if (isAccountNumberUpdated) {
      setShowPopUp(true);
    } else {
      pushSummaryData();
      const actionDetail = {
        component: COMPONENT_NAME.CALL_SUMMARY_SUBMIT,
        actionType: ACTION_TYPE.SUBMIT,
        answers: answers,
      };
      userTrackingEvent(
        COMPONENT_NAME.CALL_SUMMARY,
        ACTION_TYPE.CLICK,
        actionDetail
      );
    }
  };

  const handleCallNotes = (historyNotes) => {
    let updatedCallNotes = [];
    historyNotes.map((item) => {
      updatedCallNotes.push({ note: item?.description });
    });
    return updatedCallNotes;
  };

  useEffect(() => {
    if (
      appCallData.callSummary &&
      Object.keys(appCallData.callSummary).length
    ) {
      const mutationParameters = {
        conversationId: interactionId,
        agentNotes: appCallData.callSummary?.agentNotes,
        firstIntent: appCallData.callSummary?.firstIntent,
        isFirstIntentCorrect: appCallData.callSummary?.isFirstIntentCorrect,
        isIssueResolved: appCallData.callSummary?.isIssueResolved,
        isCallWrapped: appCallData.callSummary?.isCallWrapped,
      };
      // TRIGGER CTI CALL WRAP flow
      if (configuredCTI === CTI_PROVIDER.GENESYS_PURE_CLOUD) {
        try {
          wrapCallForGenesys(mutationParameters);
        } catch (err) {
          console.error(ERROR_MESSAGES.SUBMIT_CALL_WRAP_ERROR_IN_METHOD, err);
        }
      } else {
        callUpdateConversationMutation(mutationParameters).catch((err) => {
          setLoader2(false);
          console.error(ERROR_MESSAGES.SUBMIT_CALL_SUMMARY, err);
        });
      }
    }
  }, [appCallData.callSummary]);

  const pushSummaryData = () => {
    const notes = handleCallNotes(historyNotes);
    const summaryData = {
      conversationId: appCallData?.conversationId,
      agentNotes: notes,
      firstIntent: Object.values(appWorkFlowData)?.[0]?.intentDetected || "",
      isFirstIntentCorrect: selectedValues.isFirstIntentCorrect,
      isIssueResolved: selectedValues.isIssueResolved,
      isCallWrapped: selectedValues.isCallWrapped,
    };
    dispatch(updateCallSummary(summaryData));
    dispatch(
      updateCallSummaryNotes({
        interactionId,
        deleteCallSummaryNotes: true,
      })
    );
  };

  useEffect(() => {
    if (summaryText) {
      setInitialNotes([summaryText]);
    }
  }, [summaryText]);

  useEffect(() => {
    if (callerCustomer) setCustomerData(callerCustomer.customer);
  }, [callerCustomer]);

  useEffect(() => {
    if (notesSelection && summaryText && summaryText.length) {
      setNotesSelection({
        selected: [],
        unSelected: JSON.parse(JSON.stringify(summaryText)),
      });
    }
  }, [summaryText.length]);

  const handleRadioChange = (index, value) => {
    const newAnswers = [...answers];
    newAnswers[index] = value;
    setAnswers(newAnswers);

    const updatedSelectedValues = { ...selectedValues };
    if (index === 0) {
      updatedSelectedValues.isFirstIntentCorrect = value;
    } else if (index === 1) {
      updatedSelectedValues.isIssueResolved = value;
    } else if (index === 2) {
      updatedSelectedValues.isIssueResolved = value;
    }
    setSelectedValues(updatedSelectedValues);
    const actionDetail = {
      component: COMPONENT_NAME.CALL_SUMMARY_RESOLUTION,
      actionType: ACTION_TYPE.CLICK,
      callResolution: newAnswers,
      callNotes: historyNotes,
    };
    userTrackingEvent(
      COMPONENT_NAME.CALL_SUMMARY,
      ACTION_TYPE.CLICK,
      actionDetail
    );
  };
  const handleChange = (index, e) => {
    const newNotes = [...JSON.parse(JSON.stringify(historyNotes))];
    newNotes[index].description = e.target.value;
    setHistoryNotes(newNotes);
  };

  useEffect(() => {
    setHistoryNotes(JSON.parse(JSON.stringify(callSummaryNotes || [])));
  }, [interactionId])

  useEffect(() => {
    historyNotes.length &&
      dispatch(updateCallSummaryNotes({ interactionId, data: {historyNotes} }))
  }, [historyNotes]);

  const handleEnterKeyPress = (e) => {
    if (e.key === KEY_STROKE.ENTER && newContent.trim() !== "") {
      e.preventDefault();
      if (
        historyNotes &&
        historyNotes[historyNotes.length - 1]?.description?.trim() === ""
      ) {
        return;
      }
      setHistoryNotes([...historyNotes, { description: newContent }]);
      setNewContent("");
      const actionDetail = {
        component: COMPONENT_NAME.CALL_NOTES,
        actionType: ACTION_TYPE.CREATED,
        content: newContent,
      };
      userTrackingEvent(
        COMPONENT_NAME.CALL_SUMMARY,
        ACTION_TYPE.ENTER,
        actionDetail
      );
    }
  };

  return (
    <>
      <div
        className={`new-summary-container ${
          interactionChannel === INTERACTION_CHANNEL.CHAT
            ? "new-summary-container-flex-chat"
            : "new-summary-container-flex-voice"
        }`}
      >
        <div className="summary-header-layout">
          <Image
            className="call-summary-header-icon"
            src={CALL_SUMMARY_ICON}
            alt="Summary"
          />
          Call Summary
        </div>
        <div className="new-summary-body-container">
          <div className="new-summary_caller-info_container">
            <div className="call-summary-sub-heading">
              {CALL_SUMMARY_SUB_HEADINGS.CALL_INFO}
            </div>
            <div className="new-summary_call-info_wrapper">
              <div className="new-summary-call-info-div">
                <div className="summary-grayed-title">
                  {CALL_SUMMARY_LABELS.INTENT}
                </div>
                <p>{lastIntent}</p>
              </div>

              <div className="new-summary-call-info-div">
                <div className="summary-grayed-title">
                  {CALL_SUMMARY_LABELS.REPEAT_CALL}
                </div>
                <p>{CALL_SUMMARY_CALL_INFO.REPEAT_CALL_VALUE}</p>
              </div>

              <div className="new-summary-call-info-div">
                <div className="summary-grayed-title">
                  {CALL_SUMMARY_LABELS.INCOMING_TIME}
                </div>
                <p>{epochToTimeFormat(appCallData.callConnectedTime)} IST</p>
              </div>
              {appCallData.totalCallDuration && <div className="new-summary-call-info-div">
                <div className="summary-grayed-title">
                  {CALL_SUMMARY_LABELS.CALL_DURATION}
                </div>
                <p>{appCallData.totalCallDuration} Sec</p>
              </div>}
            </div>
          </div>

          <div className="new-summary_call-notes-container">
            <div className="call-summary-sub-heading">
              {CALL_SUMMARY_SUB_HEADINGS.CALL_NOTES}
            </div>
            <div className="call-notes-wrapper">
              {historyNotes && historyNotes?.map((item, index) => (
                <div className="notes-input-text-container" key={index}>
                  <textarea
                    className="call-notes-input-box"
                    value={item?.description}
                    rows={1}
                    onFocus={(e) => (e.target.rows = 3)}
                    onBlur={(e) => (e.target.rows = 1)}
                    onChange={(e) => handleChange(index, e)}
                  />
                </div>
              ))}
              <div>
                <textarea
                  className="call-notes-input-box"
                  value={newContent}
                  rows={1}
                  onFocus={(e) => (e.target.rows = 3)}
                  onBlur={(e) => (e.target.rows = 1)}
                  onChange={(e) => setNewContent(e.target.value)}
                  onKeyDown={handleEnterKeyPress}
                  placeholder="Create New Note..."
                />
              </div>
            </div>

            {loader && <Loading isLoading={loader} />}
          </div>

          <div className="new-summary_resolution_container">
            <div className="call-summary-sub-heading">
              {CALL_SUMMARY_SUB_HEADINGS.RESOLUTION}
            </div>
            <div className="call-summary-resolution-wrapper">
              <div className="call-summary-resolution-content">
                {CALL_SUMMARY_RESOLUTION_QUESTIONS?.map((item, index) => (
                  <span key={index}>
                    <div className="call-Summary-resolution-questions">
                      {item?.QUESTION}
                    </div>
                    <span className="new-summary-radio-item-div">
                      <>
                        <label>
                          <input
                            type="radio"
                            name={`question${index}`}
                            value="true"
                            checked={answers && answers[index] === true}
                            onChange={() => handleRadioChange(index, true)}
                          />
                          Yes
                        </label>
                      </>
                      <>
                        <label>
                          <input
                            type="radio"
                            name={`question${index}`}
                            value="false"
                            checked={answers && answers[index] === false}
                            onChange={() => handleRadioChange(index, false)}
                          />
                          No
                        </label>
                      </>
                    </span>
                  </span>
                ))}
              </div>
            </div>
          </div>
          <div className="new-summary_footer">
            <button onClick={handleCallSubmit}>
              {CALL_SUMMARY_BUTTONS.SUBMIT}
            </button>
          </div>
          {loader2 && <Loading isLoading={loader} />}

          <CallSummarySubmitModal
            open={openSubmitModal}
            onClose={() => setOpenSubmitModal(false)}
          />
        </div>
      </div>
    </>
  );
};

export default NewCallSummary;


import { createSlice } from "@reduxjs/toolkit";
import { HYDRATE } from "next-redux-wrapper";
import {
  CALL_WRAP_UP_STATE,
  INPUT_ATTRIBUTES_FOR_UPDATING_TRANSCRIPTDATA,
  INTERACTION_CHANNEL,
} from "../utility/constants";

const callInitialState = {
  isLoading: false,
  isIdle: true,
  isCallRinging: false,
  isCallConnected: false,
  isCallOnHold: false,
  isCallMuted: false,
  isCallWrapped: CALL_WRAP_UP_STATE.UNKNOWN,
  conversationId: "",
  transcriptionId: "",
  /**
   * Need to handle for chat & voice
   */
  // transcriptData: [],
  transcriptData: {},
  transcriptDataWithSentiment: [],
  liveTranscriptionId: "",
  callConnectedTime: null,
  callStartTime: null,
  callDisconnectTime: null,
  totalCallDuration: null,
  totalCallDurationString: "",
  agentAHT: null,
  callInsights: [],
  callRecommendations: [],
  callSummary: "",
  callSummaryNotes: {},
  hasRequestedShowCallAudit: false,
  showCallTranscript: true,
  showCallTagsAndInsights: false,
  showCallChecklist: false,
  callConversationAudio: { play: false, startTime: 0, endTime: 0 },
  searchCustomerInfo: false,
  incomingAlertData: null,
  aiWikiConversation: [],
  detectedWorkFlow: [],
  completedWorkFlow: [],
  workFlowData: {},
  aiAssistantData: [],
  runningWorkFlow: null,
  showAllCases: false,
  getCustomerResponse: false,
  sendCustomerResponse: false,
  lastSendCustomerSegment: "",
  createNewCase: false,
  liveAuditData: [],
  showTour: {
    tourEnded: false,
    tourProps: {
      run: false,
      steps: [],
      section: "",
    },
    showPreviousCallPageTour: false,
    showIncomingCallTour: false,
    showDummyDataForTour: false,
    showCallDetailsPageTour: {
      showPageTour: false,
      showAutoAuditPopUpTour: false,
      showAllCasesTour: false,
      showCasesPreviewTour: false,
      showNewCasePageTour: false,
      showAiWikiPageTour: false,
      showCallSummaryPageTour: false,
    },
  },
  interactionChannel: null,
};

const callSlice = createSlice({
  name: "call",
  initialState: callInitialState,
  reducers: {
    setIsLoading: (state, action) => {
      state.isLoading = action.payload;
    },
    setIsIdle: (state) => {
      state = { ...state, ...callInitialState };
    },
    resetCallState: (state) => ({ ...state, ...callInitialState, callSummaryNotes: state.callSummaryNotes }),
    setIsCallRinging: (state, action) => {
      state.isCallRinging = action.payload;
    },
    setIsCallConnected: (state, action) => {
      state.isCallConnected = action.payload;
      state.isIdle = !action.payload;
      state.isCallOnHold = false;
      state.isCallRinging = false;
      state.isCallWrapped = CALL_WRAP_UP_STATE.NOT_SUBMITTED;
    },
    setIsCallOnHold: (state, action) => {
      state.isCallOnHold = action.payload;
    },
    setIsCallMuted: (state, action) => {
      state.isCallMuted = action.payload;
    },
    setIsCallWrapped: (state, action) => {
      state.isCallWrapped = action.payload;
    },
    setShowTour: (state, action) => {
      action.payload &&
        Object.keys(action.payload).map((key, index) => {
          state.showTour[key] = action.payload[key];
        });
    },
    updateLiveAuditData: (state, action) => {
      state.liveAuditData = action.payload;
    },
    updateLiveTranscriptId: (state, action) => {
      state.liveTranscriptionId = action.payload;
      state.isLoading = false;
    },
    updateTranscriptId: (state, action) => {
      state.transcriptionId = action.payload;
      state.isLoading = false;
    },
    updateConversationId: (state, action) => {
      state.conversationId = action.payload;
      state.isLoading = false;
    },
    updateAIWikiConversation: (state, action) => {
      state.aiWikiConversation = [...state.aiWikiConversation, action.payload];
    },
    updateAIWikiFeedback: (state, action) => {
      state.aiWikiConversation[action.payload.index].feedback =
        action.payload.feedback;
    },
    /**
     * This call slice is responsible for voice call
     * @param {*} state 
     * @param {*} action 
     * @returns 
     */
    updateTranscriptData: (state, action) => {
      let transcriptObj = {};
      if (action.payload?.segmentId &&
        state.transcriptData[action.payload?.segmentId] &&
        !state.transcriptData[action.payload?.segmentId]["isPartial"] &&
	      state.transcriptData[action.payload?.segmentId]["sentiment"]
      )
        return;
      transcriptObj[action.payload?.segmentId] = action.payload;
      state.transcriptData = { ...state.transcriptData, ...transcriptObj };
    },
    /**
     * This call slice is responsible for updating sentiments for chat
     * @param {*} state 
     * @param {*} action 
     */
    updateTranscriptSentiment: (state, action) => {
      let transcriptData = state.transcriptData;
      const withSentimetData = action.payload;
      Object.keys(transcriptData).forEach((key) => {
        if (
          withSentimetData &&
          withSentimetData?.sentiment &&
          withSentimetData?.sentimentScore &&
          transcriptData[key].utterance === withSentimetData.utterance
        ) {
          transcriptData[key][
            INPUT_ATTRIBUTES_FOR_UPDATING_TRANSCRIPTDATA?.SENTIMENT_SCORE
          ] = withSentimetData.sentimentScore;
          transcriptData[key][
            INPUT_ATTRIBUTES_FOR_UPDATING_TRANSCRIPTDATA?.SENTIMENT
          ] = withSentimetData.sentiment;
        }
      });
    },
    /**
     * Not being used now previously used for chat sentiment data
     * @param {*} state 
     * @param {*} action 
     */
    updateTranscriptDataWithSentiment: (state, action) => {
      state.transcriptDataWithSentiment = [
        ...state.transcriptDataWithSentiment,
        action.payload,
      ];
    },
    /**
     * Used for chat
     * @param {*} state 
     * @param {*} action 
     */
    updateTranscriptArray: (state, action) => {
      // Logic for blank
      if(!action.payload || Object.keys(action.payload).length === 0){
        state.transcriptData = {};
        return;
      }
      const responseFromCti = action.payload.responseFromCti;
      let transcriptDataWithSentiment = [];
      let transcriptData = [];
      if(responseFromCti){
        // It means data is coming from cti transcript api
        transcriptData = Object.values(action.payload.transcriptData);
        transcriptDataWithSentiment = Object.values(state.transcriptData);
      }else{
        // It means data is coming from dynamo db
        transcriptData = Object.values(state.transcriptData);
        transcriptDataWithSentiment = Object.values(action.payload.transcriptData);
      }
      let updatedTranscriptData = [];
      transcriptData.forEach((data) => {
        let updatedTranscript = Object.assign({ selected: false }, data);
        const matchedItems = transcriptDataWithSentiment?.filter(
          (withSentiment) => withSentiment.utterance === data.utterance
        );
        if (
          matchedItems[0] &&
          matchedItems[0]?.sentiment &&
          matchedItems[0]?.sentimentScore
        ) {
          updatedTranscript[
            INPUT_ATTRIBUTES_FOR_UPDATING_TRANSCRIPTDATA?.SENTIMENT_SCORE
          ] = matchedItems[0].sentimentScore;
          updatedTranscript[
            INPUT_ATTRIBUTES_FOR_UPDATING_TRANSCRIPTDATA?.SENTIMENT
          ] = matchedItems[0].sentiment;
        }
        updatedTranscriptData.push(updatedTranscript);
      });
      if(!transcriptData.length){
        updatedTranscriptData = transcriptDataWithSentiment // Handle Case of CTI conversation API failure [Connect Customer session End]
      }
      state.transcriptData = { ...updatedTranscriptData };
    },
    updateTranscriptArrayWithSentiment: (state, action) => {
      state.transcriptDataWithSentiment = action.payload;
    },
    updateCallTime: (state, action) => {
      state.callConnectedTime = action.payload.connectedTime;
      state.callStartTime = action.payload.startTime;
    },
    updateCallDisconnectTime: (state, action) => {
      state.callDisconnectTime = action.payload.disconnectTime;
    },
    updateCallDuration: (state, action) => {
      state.totalCallDuration = action.payload;
    },
    updateCallDurationString: (state, action) => {
      state.totalCallDurationString = action.payload;
    },
    updateAgentAHT: (state, action) => {
      state.agentAHT = action.payload;
    },
    updateCallInsights: (state, action) => {
      // state.callInsights = action.payload;
      //For subscription based insights
      state.callInsights = [...state.callInsights, action.payload];
    },
    setCallRecommendations: (state, action) => {
      state.callRecommendations = action.payload;
      //For subscription based insights
      // state.callInsights = [...state.callInsights, action.payload];
    },
    updateCallRecommendations: (state, action) => {
      // if(!state.callRecommendations.includes(action.payload)){
      //   state.callRecommendations = [...state.callRecommendations, action.payload];
      // }
      let matched = false;
      state.callRecommendations.forEach((element) => {
        if (element.message == action.payload.message) {
          matched = true;
        }
      });

      if (!matched) {
        state.callRecommendations = [
          ...state.callRecommendations,
          action.payload,
        ];
      }
    },
    updateCallSummary: (state, action) => {
      state.callSummary = action.payload;
    },
    updateCallSummaryNotes: (state, action) => {
      action?.payload?.data && (
      state.callSummaryNotes = {...state.callSummaryNotes, 
        [action.payload?.interactionId]: action.payload?.data
    })
      action.payload?.deleteCallSummaryNotes && (delete state.callSummaryNotes[action.payload?.interactionId])
    },
    setShowCallTranscript: (state, action) => {
      state.showCallTranscript = action.payload;
    },
    setShowCallTagsAndInsights: (state, action) => {
      state.showCallTagsAndInsights = action.payload;
    },
    setShowCallChecklist: (state, action) => {
      state.showCallChecklist = action.payload;
    },
    setCallConverationAudio: (state, action) => {
      state.callConversationAudio = action.payload;
    },
    resetCallConversationAudio: (state, action) => {
      state.callConversationAudio = { play: false, startTime: 0, endTime: 0 };
    },
    setSearchCustomerInfo: (state, action) => {
      state.searchCustomerInfo = action.payload;
    },
    setHasRequestedShowCallAudit: (state, action) => {
      state.hasRequestedShowCallAudit = action.payload;
    },
    setIncomingAlertData: (state, action) => {
      state.incomingAlertData = action.payload;
    },
    setActionWorkFlowData: (state, action) => {
      let workflowObj = {};
      workflowObj[action.payload.name] = action.payload;
      state.workFlowData = { ...state.workFlowData, ...workflowObj };
    },
    setAiAssistantData: (state, action) => {
      if (Array.isArray(action.payload)) {
        state.aiAssistantData = action.payload;
      } else {
        state.aiAssistantData = [...state.aiAssistantData, action.payload];
      }
    },
    setRunningWorkFlow: (state, action) => {
      state.runningWorkFlow = action.payload;
    },
    setCasesButtonClicked: (state, action) => {
      state.showAllCases = action.payload;
    },
    setGetCustomerResponse: (state, action) => {
      state.getCustomerResponse = action.payload;
    },
    setSendCustomerResponse: (state, action) => {
      state.sendCustomerResponse = action.payload;
    },
    setLastSendCustomerSegment: (state, action) => {
      state.lastSendCustomerSegment = action.payload;
    },
    setDetectedWorkFlow: (state, action) => {
      if (Array.isArray(action.payload)) {
        state.detectedWorkFlow = action.payload;
      } else {
        state.detectedWorkFlow = [...state.detectedWorkFlow, action.payload];
      }
    },
    setCompletedWorkFlow: (state, action) => {
      if (Array.isArray(action.payload)) {
        state.completedWorkFlow = action.payload;
      } else {
        state.completedWorkFlow = [...state.completedWorkFlow, action.payload];
      }
    },
    setCreateCaseButtonClicked: (state, action) => {
      state.createNewCase = action.payload;
    },
    updateInteractionChannel: (state, action) => {
      state.interactionChannel = action.payload;
    },
    resetConversationState: (state, action) => {
      state.callRecommendations = [];
      state.aiAssistantData = [];
      state.detectedWorkFlow = [];
      state.updateTranscriptArray = {};
    },

    extraReducers: {
      [HYDRATE]: (state, action) => {
        return {
          ...state,
          ...action.payload.conversationId,
          ...action.payload.transcriptionId,
          ...action.payload.isCallRinging,
          ...action.payload.setIsCallOnHold,
          ...action.payload.setIsCallConnected,
          ...action.payload.setIsCallMuted,
          ...action.payload.isCallWrapped,
        };
      },
    },
  },
});

export const getConversationId = (state) => state.conversationId;
export const getTranscriptionId = (state) => state.transcriptionId;
export const getLiveTranscriptionId = (state) => state.transcriptionId;
export const getShowCallTranscript = (state) => state.call.showCallTranscript;
export const getShowCallTagsAndInsights = (state) =>
  state.call.showCallTagsAndInsights;
export const getShowCallChecklist = (state) => state.call.showCallChecklist;
export const getCallConversationAudio = (state) =>
  state.call.callConversationAudio;
export const getSearchCustomerInfo = (state) => state.call.searchCustomerInfo;

export const {
  setIsIdle,
  updateTranscriptData,
  updateTranscriptDataWithSentiment,
  resetCallState,
  setIsCallWrapped,
  setIsCallRinging,
  setIsCallConnected,
  setIsCallOnHold,
  setIsCallMuted,
  updateConversationId,
  updateTranscriptId,
  updateLiveTranscriptId,
  setIsLoading,
  updateCallTime,
  updateCallDisconnectTime,
  updateCallDuration,
  updateCallDurationString,
  updateAgentAHT,
  updateCallInsights,
  updateCallRecommendations,
  updateCallSummary,
  updateCallSummaryNotes,
  setShowCallTranscript,
  setShowCallTagsAndInsights,
  setShowCallChecklist,
  setCallConverationAudio,
  resetCallConversationAudio,
  setSearchCustomerInfo,
  setHasRequestedShowCallAudit,
  updateLiveAuditData,
  updateTranscriptArray, // Added for Direct Messages array update
  updateTranscriptArrayWithSentiment, // Added for Direct Messages array update
  setCallRecommendations, // Added for Direct Messages array update,
  setIncomingAlertData, // add Alert related Details // minified customer related data
  updateAIWikiConversation, // Agent Wiki Inclusion
  updateAIWikiFeedback,
  setAiAssistantData,
  setActionWorkFlowData,
  setRunningWorkFlow,
  setCasesButtonClicked,
  setGetCustomerResponse,
  setSendCustomerResponse,
  setLastSendCustomerSegment,
  setDetectedWorkFlow,
  setCompletedWorkFlow,
  setCreateCaseButtonClicked,
  setShowTour,
  updateInteractionChannel,
  updateTranscriptSentiment,
  resetConversationState
} = callSlice.actions;

export default callSlice;
