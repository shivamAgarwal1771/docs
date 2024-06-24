import React, { useState, useEffect } from 'react'
import Image from "next/future/image";
import NewCallWidget from './call-widget/callWidget';
import AiAssistant from './ai-assistant';
import SentimentGraph from './sentiment/SentimentGraph';
import Transcript from './transcript';
import InteractionHistory from './interaction-history';
import ContactInformation from './contact';
import Cases from './cases';
// import contactData from '../../assets/resource/dummyContactInfo.json';
import { useSelector } from 'react-redux';
import AutoAudit from './auto-audit';
import { CALL_WRAP_UP_STATE, HEADERS } from '../../utility/constants';
import { setIsCallWrapped } from '../../store/callSlice';
import CallSummaryDashboard from '../../pages/agentOldDesign/conversation-details/CallSummaryDashboard';
import AIWiki from './ai-wiki';
import AIWikiSymbol from '../../assets/img/wiki_symbol.svg'
import AutoAuditPopup from './auto-audit/auto-audit-popup';
import { fetchData } from 'next-auth/client/_utils';
import { useRouter } from 'next/router';
import { useDispatch } from 'react-redux';
import styles from "../../assets/css/agent.module.scss";
import { selectClient, setCustomerName } from "../../store/userSlice"
import { setIsCallRinging, setSimulationUseCase } from '../../store/callSlice';
import CustomerInfoPageOne from './customer-info-360/customer-info-page';
import CustomerInfo360Page from './customer-info-360/customer-info-360-page';
import { getStaticResource } from '../../utility/automationBotHandler'
import { useIntl } from "react-intl";

const SAABaseLayout = () => {
  const intl = useIntl()
  const locale = useSelector((state: any) => state.user.locales)
  const appCallData = useSelector((state: any) => state.call);
  const showAllCases = appCallData.showAllCases;
  const callDisconnectTime = appCallData.callDisconnectTime;
  const iscallWrappedUp = appCallData.isCallWrapped;
  // const  callDisconnectTime = true;
  // const iscallWrappedUp = CALL_WRAP_UP_STATE.NOT_SUBMITTED;
  const isOnCall = appCallData.isCallConnected || appCallData.isCallOnHold;
  // appCallData.callDisconnectTime && appCallData.isCallWrappedUP === CALL_WRAP_UP_STATE.NOT_SUBMITTED
  const [hide, setHide] = useState(false);
  const [showWiki, setShowWiki] = useState(false)
  const [showAutoAuditPopUp, setShowAutoAuditPopup] = useState(false);
  const [selectedQuestion, setSelectedQuestion] = useState({});
  const [render, setRender] = useState([])
  const [history, setHistory] = useState([])
  const [contact, setContact] = useState([])
  const [customer, setCustomer] = useState([])
  const [summary, setSummary] = useState([])
  const [transcript, setTranscript] = useState([])
  const setSelectedClient = useSelector((state: any) => state.user)
  const [activePage, setActivePage] = useState(true);
  const router = useRouter();
  const dispatch = useDispatch()
  const clientJson = setSelectedClient.selectedClient
  let finalData
  useEffect(() => {
    const fetchQueryData = async () => {
      let customerName;
      let agentName;
      let TranscriptInfo = "transcript.json";
      while (!router.isReady) {
        await new Promise((resolve) => setTimeout(resolve, 50))
      }
      try {
        const customerInfo = router.query
        const data = `${customerInfo.clientName}_${customerInfo.version}`
        finalData = data.toLowerCase()
        dispatch(selectClient(finalData))
        getStaticResource("customerInfo.json", finalData).then((data) => {
          dispatch(setCustomerName(intl.formatMessage({ id: data?.customerName })))
          customerName = (intl.formatMessage({ id: data?.customerName }))
          agentName = (intl.formatMessage({ id: data?.agentName }))
          setCustomer([customerName, agentName])
          setContact(data?.personalInformation)
        })
        getStaticResource("summary.json", finalData).then((data) => setSummary(data))
        getStaticResource("cases.json", finalData).then((data) => setRender(data))
        getStaticResource("history.json", finalData).then((data) => setHistory(data))
        finalData === HEADERS.DEMO ? locale === "en" ? TranscriptInfo = "transcript_en.json" : TranscriptInfo = "transcript_hi.json" : TranscriptInfo = "transcript.json";
        getStaticResource(TranscriptInfo, finalData).then((data) => setTranscript(data))
        dispatch(setIsCallRinging(true))
      } catch (error) { console.log(error) }
    }
    fetchQueryData()
  }, [router.isReady, clientJson])


  return (
    <>
      <div className='platform-container'>
        <div className='distant-start-section'>
          <div className='sentiment-section component-box-shadow'><SentimentGraph /></div>
          <div className='transcript-section component-box-shadow'><Transcript customerData={customer} /></div>
          <AutoAudit setShowAutoAuditPopup={setShowAutoAuditPopup} selectedQuestion={selectedQuestion} setSelectedQuestion={setSelectedQuestion} />
        </div>
        {activePage ?
          <CustomerInfoPageOne
            casesData={render}
            historyData={history}
            contactData={contact}
            hide={hide}
            setHide={setHide}
            activePage setActivePage={setActivePage} />
          :
          <CustomerInfo360Page
            casesData={render}
            historyData={history}
            contactData={contact}
            hide={hide}
            setHide={setHide}
            activePage setActivePage={setActivePage} />
        }
        <div className='distant-end-section'>
          <>{isOnCall &&
            <>
              <div className="call-widget-section component-box-shadow"><NewCallWidget /></div>
              <AiAssistant formData={transcript} />
            </>}
            {(callDisconnectTime && iscallWrappedUp === CALL_WRAP_UP_STATE.NOT_SUBMITTED) && <CallSummaryDashboard summaryInfo={summary} />}

          </>
        </div>
        <div className='aiwiki-symbol' onClick={() => { setShowWiki(true) }}>
          <Image src={AIWikiSymbol} width={60} height={60} alt='AI WIki' />
        </div>
        <AIWiki show={showWiki} setShow={setShowWiki} />
        {showAutoAuditPopUp && <AutoAuditPopup setShowAutoAuditPopup={setShowAutoAuditPopup} selectedQuestion={selectedQuestion} setSelectedQuestion={setSelectedQuestion} />}

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
  )
}

export default SAABaseLayout;



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
    const locale = useSelector((state) => state.user.locales)
    const localeMessages = messages[locale] || messages['en']

    return (
        <IntlProvider locale={locale} messages={localeMessages}>
            <TranslationProvider>
                {children}
            </TranslationProvider>
        </IntlProvider>
    )
}
const TranslationProvider = ({ children }) => {
    const intl = useIntl();
    const translateLabel = (label, values) => intl.formatMessage({ id: label, ...values });
    return (
        <TranslationContext.Provider value={{ translateLabel }}>
            {children}
        </TranslationContext.Provider>
    )
}
export const useTranslation = () => {
    return useContext(TranslationContext);
}



import React from 'react';
import { useTranslation } from './IntlProviderWrapper';

const withTranslation = (WrappedComponent) => (props) => {
    const { translateLabel: t } = useTranslation()
    return <WrappedComponent{...props} t={t} />
}

export default withTranslation;

import withTranslation from "./withTranslation";
import MyApSAABaseLayoutp from "./base-layout/index"

const wrappedMyApp = withTranslation(MyApSAABaseLayoutp)
export default wrappedMyApp
