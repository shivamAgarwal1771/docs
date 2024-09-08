import React, { useState, useEffect } from "react";
import Image from "next/future/image";
import NewCallWidget from "./call-widget/callWidget";
import AiAssistant from "./ai-assistant";
import SentimentGraph from "./sentiment/SentimentGraph";
import Transcript from "./transcript";
import InteractionHistory from "./interaction-history";
import ContactInformation from "./contact";
import Cases from "./cases";
// import contactData from '../../assets/resource/dummyContactInfo.json';
import { useSelector } from "react-redux";
import AutoAudit from "./auto-audit";
import {
  CALL_WRAP_UP_STATE,
  HEADERS,
  ASSISTANT,
  LANGUAGE,
} from "../../utility/constants";
import { setIsCallWrapped } from "../../store/callSlice";
import CallSummaryDashboard from "../../pages/agentOldDesign/conversation-details/CallSummaryDashboard";
import AIWiki from "./ai-wiki";
import AIWikiSymbol from "../../assets/img/wiki_symbol.svg";
import AutoAuditPopup from "./auto-audit/auto-audit-popup";
import { fetchData } from "next-auth/client/_utils";
import { useRouter } from "next/router";
import { useDispatch } from "react-redux";
import styles from "../../assets/css/agent.module.scss";
import { selectClient, setCustomerName, setFileName } from "../../store/userSlice";
import { setIsCallRinging, setSimulationUseCase, updateHeader } from "../../store/callSlice";
import CustomerInfoPageOne from "./customer-info-360/customer-info-page";
import CustomerInfo360Page from "./customer-info-360/customer-info-360-page";
import { getStaticResource } from "../../utility/automationBotHandler";
import { useAppContext } from "../common/appContext";
import getCredentials from "../../utility/authentication/get-credentials";
import { getFolderFromS3 } from "../../utility/upload-demo/getfolderfroms3";
import { concat } from "@apollo/client";
import CallSimulatonPlayer from "../common/audioPlayer";
import { selectedLanguage } from "../../store/userSlice";
import { LANGUAGES } from "../../utility/constants";

const SAABaseLayout = () => {
  const { intlTranslator } = useAppContext();
  const locale = useSelector((state: any) => state.user.locales);
  const appCallData = useSelector((state: any) => state.call);
  const showAllCases = appCallData.showAllCases;
  const callDisconnectTime = appCallData.callDisconnectTime;
  const iscallWrappedUp = appCallData.isCallWrapped;
  const isOnCall = appCallData.isCallConnected || appCallData.isCallOnHold;
  const [hide, setHide] = useState(false);
  const [showWiki, setShowWiki] = useState(false);
  const [showAutoAuditPopUp, setShowAutoAuditPopup] = useState(false);
  const [selectedQuestion, setSelectedQuestion] = useState({});
  const [render, setRender] = useState([]);
  const [history, setHistory] = useState([]);
  const [contact, setContact] = useState([]);
  const [customer, setCustomer] = useState([]);
  const [summary, setSummary] = useState([]);
  const [transcript, setTranscript] = useState([]);
  const setSelectedClient = useSelector((state: any) => state.user);
  const [activePage, setActivePage] = useState(true);
  const [tabs, setTabs] = useState("");
  const [demoAgent, setDemoAgent] = useState("");
  const [demoCustomer, setDemoCustomer] = useState("");
  const [customerInfo360, setCustomerInfo360] = useState({})

  const router = useRouter();
  const dispatch = useDispatch();
  const clientJson = setSelectedClient.selectedClient;
  let s3folderName;

  function caseSensitiveKey(obj, targetkey){
    const lowerkey = targetkey.toLowerCase();
    for (const key in obj){
      if(key.toLowerCase() === lowerkey){
        return key;
      }
    }
  }

  useEffect(() => {
    s3folderName = router.query.fileName;
    dispatch(setFileName(s3folderName))
    const getData = async () => {
      try {
        const credentials = await getCredentials();
        const customerInfo = router.query;

        const fetchedData = await getFolderFromS3(credentials, s3folderName);
        const metadata = fetchedData.metadata;
        const dialect = metadata.selectedLanguage;

        if (dialect === LANGUAGES.ENGLISH.name) {
          dispatch(selectedLanguage(LANGUAGES.ENGLISH.languageCode));
        } else if (dialect === LANGUAGES.HINDI.name) {
          dispatch(selectedLanguage(LANGUAGES.HINDI.languageCode));
        } else if (dialect === LANGUAGES.SPANISH.name) {
          dispatch(selectedLanguage(LANGUAGES.SPANISH.languageCode));
        } else {
          dispatch(selectedLanguage(LANGUAGES.MANDARIN.languageCode));
        }

        const customer360data = metadata?.customer360
        console.log("CUSTOMER",customer360data)
        const agentName = metadata?.agent;
        let customerName = "";
        let personalInfo = metadata?.personalInformation

        if (personalInfo){
          if(personalInfo["Customer Name"] || personalInfo["Name"] || personalInfo["Member Name"] || personalInfo["Policy Holder Name"]  ){
            customerName = (personalInfo["Customer Name"] || personalInfo["Name"] || personalInfo["Member Name"] || personalInfo["Policy Holder Name"] )
          } else{
            const firstNameKey = caseSensitiveKey(personalInfo, 'First Name')
            const lastNamekey = caseSensitiveKey(personalInfo, 'Last Name')
            if(firstNameKey && lastNamekey){
              customerName = personalInfo[firstNameKey] + " " + personalInfo[lastNamekey]
            }else if(firstNameKey){
              customerName = personalInfo[firstNameKey]
            }
          }
        }

        const transcriptionData = fetchedData.transcript;
        setTranscript(transcriptionData);
        setCustomerInfo360(customer360data);
        setDemoAgent(agentName);
        setDemoCustomer(customerName);
        setSummary(metadata?.summary);
        setRender(metadata?.cases);
        setHistory(metadata?.interactionHistory);
        setContact(metadata?.personalInformation);
        dispatch(setIsCallRinging(true));
        dispatch(updateHeader(metadata?.header));
      } catch (err) {
        console.error("Error fetching data from s3", err);
      }
    };
    getData();
  }, [router.isReady]);

  // useEffect(()=>{
  //   dispatch(selectedLanguage(LANGUAGES.ENGLISH.languageCode));
  // },[dialect])

  useEffect(() => {
    setCustomer([demoCustomer, demoAgent]);
    dispatch(setCustomerName(demoCustomer));
  }, [demoCustomer, demoAgent]);

  useEffect(() => {
    if (Object.values(appCallData.contextDetails).length === 0) {
      setTabs(ASSISTANT.AI_ASSISTANT);
    } else {
      setTabs(ASSISTANT.CALL_CONTEXT);
      const timer = setTimeout(() => {
        setTabs(ASSISTANT.AI_ASSISTANT);
      }, 10000);
      return () => clearTimeout(timer);
    }
  }, [appCallData.contextDetails]);

  return (
    <>
      <div className="platform-container">
        <div className="distant-start-section">
          <div className="sentiment-section component-box-shadow">
            <SentimentGraph />
          </div>
          <div className="transcript-section component-box-shadow">
            <Transcript customerData={customer} />
          </div>
          <AutoAudit
            setShowAutoAuditPopup={setShowAutoAuditPopup}
            selectedQuestion={selectedQuestion}
            setSelectedQuestion={setSelectedQuestion}
          />
        </div>
        {activePage ? (
          <CustomerInfoPageOne
            casesData={render}
            historyData={history}
            contactData={contact}
            hide={hide}
            setHide={setHide}
            activePage
            setActivePage={setActivePage}
          />
        ) : (
          <CustomerInfo360Page
            casesData={render}
            historyData={history}
            contactData={contact}
            hide={hide}
            customerinfo360={customerInfo360}
            setHide={setHide}
            activePage
            setActivePage={setActivePage}
          />
        )}
        <div className="distant-end-section">
          <>
            {isOnCall && (
              <>
                <div className="call-widget-section component-box-shadow">
                  <NewCallWidget />
                </div>
                <AiAssistant
                  formData={transcript}
                  summaryData={summary}
                  tabs={tabs}
                />
              </>
            )}
            {callDisconnectTime &&
              iscallWrappedUp === CALL_WRAP_UP_STATE.NOT_SUBMITTED && (
                <AiAssistant
                  formData={transcript}
                  summaryData={summary}
                  tabs="Call Summary"
                />
              )}
          </>
        </div>
        <div
          className="aiwiki-symbol"
          onClick={() => {
            setShowWiki(true);
          }}
        >
          <Image src={AIWikiSymbol} width={60} height={60} alt="AI WIki" />
        </div>
        <AIWiki show={showWiki} setShow={setShowWiki} />
        {showAutoAuditPopUp && (
          <AutoAuditPopup
            setShowAutoAuditPopup={setShowAutoAuditPopup}
            selectedQuestion={selectedQuestion}
            setSelectedQuestion={setSelectedQuestion}
          />
        )}
      </div>
      <div>
        <footer className={styles.footer}>
          <p>Powered by</p>
          <Image
            src={require("../../assets/img/agent/exl_small.png").default}
            alt="exl-small"
            height={14}
            width={30}
          />
        </footer>
      </div>
    </>
  );
};

export default SAABaseLayout;
