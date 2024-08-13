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
