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
import { useAppContext } from "../common/appContext"

const SAABaseLayout = () => {
  const { intlTranslator } = useAppContext()
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
      if (!router.isReady) return;
      try {
        const customerInfo = router.query
        const data = `${customerInfo.clientName}_${customerInfo.version}`
        finalData = data.toLowerCase()
        dispatch(selectClient(finalData))
        getStaticResource("customerInfo.json", finalData).then((data) => {
          dispatch(setCustomerName(intlTranslator(data?.customerName)))
          customerName = (intlTranslator(data?.customerName))
          agentName = (intlTranslator(data?.agentName))
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
              <AiAssistant formData={transcript} summaryData={summary} tabs="AI Assistant" />
            </>}
            {(callDisconnectTime && iscallWrappedUp === CALL_WRAP_UP_STATE.NOT_SUBMITTED) && <AiAssistant formData={transcript} summaryData={summary} tabs="Call Summary" />}

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
