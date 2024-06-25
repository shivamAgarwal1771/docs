components/common/AppContext.tsx

 import React, { createContext, useContext } from 'react';
 
 interface AppContextProps {
   userTrackingEvent: any;
   children?: React.ReactNode;
 }
 
 const AppContext = createContext<AppContextProps | undefined>(undefined);
 
 export const AppProvider: React.FC<AppContextProps> = ({ children, userTrackingEvent }) => {
   return <AppContext.Provider value={{ userTrackingEvent }}>{children}</AppContext.Provider>;
 };
 
 export const useAppContext = () => {
   const context = useContext(AppContext);
   if (!context) {
     throw new Error('useAppContext must be used within an AppProvider');
   }
   return context;
 };

 components/hoc/applicationLayoutHOC.js

 import React from "react";
 import Navbar from "../Navbar";
 import { useTracking } from "react-tracking";
 import { useSelector } from "react-redux";
 import { AppProvider } from '../../components/common/AppContext';
 import { CALL_WRAP_UP_STATE } from "../../utility/constants";
  
 const ApplicationLayoutHOC = ({ children }) => {
   const appCallData = useSelector((state) => state.call);
   const tracking = useTracking();
   const dispatchTrackingEvent = (className, userAction, detail) => {
     const eventData = {
       className, 
       userAction, 
       conversationId: appCallData?.conversationId, 
       callDuration: appCallData.totalCallDuration, 
       currentTime: (new Date()).toISOString(), 
       detail,
       isDisconnected: appCallData.callDisconnectTime && appCallData.isCallWrapped === CALL_WRAP_UP_STATE.NOT_SUBMITTED || false
     };
     tracking.trackEvent(eventData);
   }
   return (
     <AppProvider userTrackingEvent={dispatchTrackingEvent}>
       <div className="smart-app-wrapper">
         <Navbar />
         <div className="smart-app-container">
           </div>
         </div>
       </div>
     </AppProvider>
   );
 };
 export default ApplicationLayoutHOC;

 components/cti/index.js

 import client from "../../apollo-client";
 import { useRouter } from "next/router";
 import IncomingCallModal from "./IncomingCallModal";
 import { useAppContext } from "../../components/common/AppContext";
  
 import {
   CALL_INTERACTION_WIDGET_TEXT,
   CTI_USER_STATUS,
   SHOW_INITIAL_RECOMMENDATION,
   INITIAL_RECOMMEDATION,
   AI_ASSISTANT_TYPE
   AI_ASSISTANT_TYPE,
   USER_EVENTS,
   ACTION_TYPE,
   COMPONENT_NAME
 } from "../../utility/constants";
 import { checkForJsonInChat } from "../../utility"

     conversationId: "",
     callChannel: "",
   });
   const { userTrackingEvent } = useAppContext();
  
   const getRandomArbitrary = (min, max) => {
     return Math.random() * (max - min) + min;
                         }))
                       } 
                       else if (data.botId) {
                         const actionDetail = {
                             component: "ActionWorkFlow",
                             actionType: ACTION_TYPE.DETECTED,
                             name: data?.intentDetected
                         }
                         userTrackingEvent(COMPONENT_NAME.CTI_COMPONENT, USER_EVENTS.CLICK, actionDetail);
                         dispatch(setDetectedWorkFlow({
                           "botId": data?.botId,
                           "aliasId": data?.aliasId,
