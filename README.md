import { useEffect, useRef, useState } from "react";
import Image from "next/future/image";
import EXL_LOGO from "../assets/img/agent/exl_big.png";
import AGENT_LOGO from "../assets/img/agent/profile.svg";
import { useOktaAuth } from "@okta/okta-react";
import { useDispatch, useSelector } from "react-redux";
import { signOut } from "../store/userSlice";
import { parseResponse, smartAgentLog } from "../utility";
import Router, { useRouter } from "next/router";
import { resetCallState } from "../store/callSlice";
import { MdOutlineKeyboardArrowDown } from "react-icons/md";
import { BiChevronDown } from "react-icons/bi";
import { RiRadioButtonLine } from "react-icons/ri";
import { CALL_AVAILABLE_STATE, CTI_USER_STATUS, BYPASS_OKTA_LOGIN } from "../utility/constants";
import useClickOutside from "../hooks/useClickOutside";
import CTIComponent from "./cti";
import currentInstance from '../components/cti/session'
import { resetConversationMessages } from "../store/conversationSlice";
import CallSimulationPlayer from './common/audioPlayer'
import LanguageSwitcher from "./languageSwitcher"
import { useIntl } from "react-intl";

export default function Navbar(props) {
  const intl = useIntl()
  const { oktaAuth, authState } = useOktaAuth();
  const availableStateRef = useRef(null);
  const dispatch = useDispatch();
  const router = useRouter();
  const appUser = useSelector((state) => state.user);
  const userName = authState?.idToken?.claims?.name;
  const appCallData = useSelector((state) => state.call);
  const [isAvailable, setIsAvailable] = useState(false);
  const [availableState, setAvailableState] = useState(
    CALL_AVAILABLE_STATE.READY
  );
  const [showAvailableState, setShowAvailableState] = useState(false);

  const handleSignOut = async () => {
    // const [signOutError, signOutResponse] = await parseResponse(
    //   // oktaAuth.signOut()
    // );
    // if (signOutError) {
    //   console.error("signOutError", signOutError);
    //   // TODO Pop Up "Unable to perform SignOut User!"
    //   return;
    // }
    dispatch(signOut());
    dispatch(resetCallState());
    dispatch(resetConversationMessages()); //TODO remove
  };

  const pushToHome = () => {
    Router.push("/agent");
  };

  function updateUserStatus(status) {
    let temp;
    if (status == CTI_USER_STATUS.BUSY) { temp = 'offline' };
    if (status == CTI_USER_STATUS.ON_QUEUE) { temp = 'routable' };
    if (status == CTI_USER_STATUS.AWAY) { temp = 'offline' };
    if (!currentInstance?.agent?.getAgentStates) {
      return;
    }
    var routableState = currentInstance?.agent?.getAgentStates()?.filter(function (state) {
      return state.type === temp;
    })[0];
    currentInstance?.agent?.setState(routableState, {
      success: function () {
        console.log("Set agent status to Available (routable) via Streams")
      },
      failure: function (e) {
        console.log("Failed to set agent status to Available (routable) via Streams" + e)
      }
    });
  }

  const onAvailableStateClick = (state) => {
    setAvailableState(state);
    setShowAvailableState(false);
  };

  // useEffect(() => {

  // });

  // useEffect(() => {
  //   if ((oktaAuth && authState && !authState.isAuthenticated) || !BYPASS_OKTA_LOGIN) {
  //     router.push("/login");
  //     dispatch(resetCallState());
  //     dispatch(signOut());
  //   }
  // }, [authState?.isAuthenticated]);

  useClickOutside(
    availableStateRef,
    () => availableState && setShowAvailableState(false)
  );

  useEffect(() => {
    if (
      appUser.currentCTIStatus === CTI_USER_STATUS.BUSY ||
      appUser.currentCTIStatus === CTI_USER_STATUS.AWAY
    ) {
      onAvailableStateClick(CALL_AVAILABLE_STATE.NOT_READY);
      if (appUser.currentCTIStatus === CTI_USER_STATUS.AWAY) {
        setIsAvailable(false);
      }
    } else if (
      appUser.currentCTIStatus === CTI_USER_STATUS.ON_QUEUE ||
      appUser.currentCTIStatus === CTI_USER_STATUS.AVAILABLE
    ) {
      setIsAvailable(true);
      if (appUser.currentCTIStatus === CTI_USER_STATUS.AVAILABLE) {
        onAvailableStateClick(CALL_AVAILABLE_STATE.NOT_READY);
      } else {
        onAvailableStateClick(CALL_AVAILABLE_STATE.READY);
      }
    }
  }, [appUser.currentCTIStatus]);


  return (
    <>
      {((oktaAuth && authState && authState.isAuthenticated) || BYPASS_OKTA_LOGIN) && (
        <>
          <div className="smart-navbar component-box-shadow">
            <div className="logo-wrapper">
              <Image
                className="nav-logo"
                alt="green-icon"
                width={"100"}
                height={"60"}
                src={EXL_LOGO}
              />
              <div className="logo-divider" />
              <div className="nav-agent-logo" onClick={() => pushToHome()}>
               {intl.formatMessage({ id:'Agent Assist' })}
              </div>
            </div>
            {appCallData.isCallConnected && <CallSimulationPlayer />}
            <div className="user-info-container">
              <div className="user-availablity">
                <div>
                  <LanguageSwitcher />
                </div>
                <label className="switch">
                  <input
                    type="checkbox"
                    checked={isAvailable}
                    onChange={(e) => {
                      setIsAvailable(e.target.checked);
                      updateUserStatus(
                        e.target.checked
                          ? CTI_USER_STATUS.ON_QUEUE
                          : CTI_USER_STATUS.AWAY
                      );
                      onAvailableStateClick(CALL_AVAILABLE_STATE.READY);
                    }}
                  />
                  <span className="slider round"></span>
                </label>
                <div
                  className={
                    isAvailable ? "user-available" : "user-unavailable"
                  }
                >
                  <p>{isAvailable ? intl.formatMessage({ id: 'Available' }) : intl.formatMessage({ id: 'Unavailable' })}</p>
                </div>
              </div>

              {/* {isAvailable && (
                <div className="user-available-state">
                  <div onClick={() => setShowAvailableState((prev) => !prev)}>
                    <RiRadioButtonLine
                      color={
                        availableState === CALL_AVAILABLE_STATE.READY
                          ? "6ac5ab"
                          : availableState === CALL_AVAILABLE_STATE.NOT_READY
                            ? "#dbdbdb"
                            : "#c56a6a"
                      }
                    />
                    <p>{availableState}</p>
                    {availableState !== CALL_AVAILABLE_STATE.IN_A_CALL && (
                      <BiChevronDown />
                    )}
                  </div>
                  {showAvailableState && (
                    <ul className="user-available-options">
                      <li
                        onClick={(event) => {
                          onAvailableStateClick(CALL_AVAILABLE_STATE.READY);
                          updateUserStatus(CTI_USER_STATUS.ON_QUEUE);
                        }}
                      >
                        {CALL_AVAILABLE_STATE.READY}
                      </li>
                      <li
                        onClick={() => {
                          onAvailableStateClick(CALL_AVAILABLE_STATE.NOT_READY);
                          updateUserStatus(CTI_USER_STATUS.BUSY);
                        }}
                      >
                        {CALL_AVAILABLE_STATE.NOT_READY}
                      </li>
                    </ul>
                  )}
                </div>
              )} */}

              <div>
                <Image
                  className="navbar-avatar"
                  src={AGENT_LOGO}
                  alt="Profile"
                />
              </div>
              <div>
                <div>
                  <span>{userName} </span>
                  <RiRadioButtonLine
                    color={
                      availableState === CALL_AVAILABLE_STATE.READY
                        ? "6ac5ab"
                        : availableState === CALL_AVAILABLE_STATE.NOT_READY
                          ? "#dbdbdb"
                          : "#c56a6a"
                    }
                  />
                </div>
                <div className="agent-subtitle">Agent</div>
              </div>
              <div>
                <button type="button" id="menu1" data-bs-toggle="dropdown">
                  <MdOutlineKeyboardArrowDown size={20} color="#ffffff" />
                </button>
                <ul
                  className="dropdown-menu"
                  role="menu"
                  aria-labelledby="menu1"
                >
                  <li role="presentation">
                    <button
                      className="dropdown-item-group"
                      onClick={() => handleSignOut()}
                    >
                      Logout
                    </button>
                  </li>
                </ul>
              </div>
            </div>
          </div>
          <div className="cti-view-container">
            <CTIComponent />
          </div>

          {/* <div className="softphone">
             <iframe
              hidden
              id="softphone"
              allow="camera *; microphone *"
              src="https://apps.usw2.pure.cloud/crm/embeddableFramework.html?dedicatedLoginWindow=false&orgName=exl-cx-coe&provider=okta"
            ></iframe> 
          </div> */}
        </>
      )}
    </>
  );
}

import { useSelector } from "react-redux"
import { IntlProvider } from "react-intl"
import mainEn from "../locales/en/en.json"
import mainHi from "../locales/hi/hi.json"
import { useIntl } from 'react-intl'
import React, { createContext, useContext } from "react"

const messages = {
    en: mainEn, hi: mainHi
}
const TranslationContext = createContext();

export const IntlProviderWrapper = ({ children }) => {
    const intl = useIntl()
    const locale = useSelector((state) => state.user.locales)
    const localeMessages = messages[locale] || messages['en']
    const translateLabel = (label, values) => intl.formatMessage({ id: label, ...values })
    return (
        <TranslationContext.Provider value={{ translateLabel}}>
            <IntlProvider locale={locale} messages={localeMessages}>
                {children}
            </IntlProvider>
        </TranslationContext.Provider>
    )
}

export const useTranslation = () => {
    return useContext(TranslationContext);
}

import "bootstrap/dist/css/bootstrap.css";
import "../assets/css/custom.css";
import "bootstrap/dist/css/bootstrap.min.css";
import { Amplify } from "aws-amplify";
import "../assets/css/globals.css";
import "../assets/css/common.css";
import "../assets/css/callmodal.css";
// import "../assets/css/calldetails.css";
import "../assets/css/chatDetails.css";
import "../assets/css/reacttable.css";
import "../assets/css/base-layout.css";
import "../assets/css/ai-assistant.css";
import "../assets/css/action-workflow.css";
import "../assets/css/callWidget.css";
import "../assets/css/sentiment.css";
import "../assets/css/transcript.css";
import "../assets/css/contactInformation.css";
import "../assets/css/cases.css";
import "../assets/css/editCase.css";
import "../assets/css/interactionHistory.css";
import "../assets/css/createCase.css";
import "../assets/css/auto-audit.css";
import "../assets/css/callsummary.css";
import "../assets/css/ai-wiki.css";
import "../assets/css/dashboard.css";
import "../assets/css/customer-360.css";
import "../assets/css/demoPage.css"
import "../assets/css/languages.css"
import Head from "next/head";
import { IntlProviderWrapper } from '../components/IntlProviderWrapper'
import { useEffect } from "react";
import { SessionProvider } from "next-auth/react";
import { Provider } from "react-redux";
import { OktaAuth, toRelativeUrl } from "@okta/okta-auth-js";
import { Security } from "@okta/okta-react";
import Router from "next/router";
import { store, persistor } from "../store";
import ApplicationLayoutHOC from "../components/hoc/applicationLayoutHOC";

if (typeof window !== "undefined") {
  require("amazon-connect-streams");
}

import { ApolloProvider } from "@apollo/client";

import client from "../apollo-client";
import { PersistGate } from "redux-persist/integration/react";

// setupAxiosInterceptors(); // Custom Backend Response interceptor deactivated for Generic response!

const myAppConfig = {
  aws_appsync_graphqlEndpoint: process.env.AWS_APP_SYNC_ENDPOINT || "",
  aws_appsync_region: process.env.AWS_APP_SYNC_REGION || "",
  aws_appsync_authenticationType: process.env.AWS_APP_SYNC_AUTH_TYPE || "",
  aws_appsync_apiKey: process.env.AWS_APP_SYNC_API_KEY || "",
};

Amplify.configure(myAppConfig);

const oktaAuthClient = new OktaAuth({
  issuer: process.env.NEXT_PUBLIC_OKTA_ISSUER || "",
  clientId: process.env.OKTA_CLIENT_ID || "",
  redirectUri: "/",
});

function MyApp({ Component, pageProps }) {
  useEffect(() => {
    typeof document !== undefined
      ? require("bootstrap/dist/js/bootstrap")
      : null;

    typeof document !== undefined ? require("moment/moment.js") : null;
  }, []);

  const restoreOriginalUri = async (_oktaAuth, originalUri) => {
    // Router.replace(toRelativeUrl(originalUri || "/", window.location.origin));
    Router.replace("/");
  };

  return (
    <Provider store={store}>
      <PersistGate
        loading={<div className="loading-view">Loading...</div>}
        persistor={persistor}
      >
        <Security
          oktaAuth={oktaAuthClient}
          restoreOriginalUri={restoreOriginalUri}
        >
          <SessionProvider session={pageProps.session}>
            <ApolloProvider client={client}>
              <IntlProviderWrapper>
                  <Head>
                    <title>Agent Assist</title>
                    <meta
                      name="viewport"
                      content="width=device-width, initial-scale=1"
                    />
                  </Head>
                  <ApplicationLayoutHOC>
                    <Component {...pageProps} />
                  </ApplicationLayoutHOC>
              </IntlProviderWrapper>
            </ApolloProvider>
          </SessionProvider>
        </Security>
      </PersistGate>
    </Provider >
  );
}

export default MyApp;# docs
