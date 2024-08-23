import { useEffect, useState } from "react";
import { FaAngleDown, FaAngleUp } from "react-icons/fa";
import Image from "next/future/image";
import AUTOAUDIT_ICON from "../../../assets/img/agent/auto-audit-icon.svg";
import AUTOAUDIT_LIST_ICON from "../../../assets/img/agent/auto-audit-list-icon.svg";
import { AUTO_AUDIT_DETAILS, HEADERS } from "../../../utility/constants";
// import {useAppContext} from "../../common/appContext"
import AutoAudit from "../../base-layout/auto-audit";
import ReactDOM from 'react-dom';
import React from 'react';
import AutoAuditPopup from "./auto-audit-popup";
import { AppProvider, useAppContext } from '../../../components/common/appContext';
import { Provider, useSelector } from "react-redux";
import { store } from "../../../store";
import { Modal } from "react-bootstrap";
import RenderIcon from "./auto-audit-renderIcon";
// import auto-audit from "../../../assets/css/auto-audit.css"
import { AUTO_AUDIT } from "../../../utility/constants";
 
 
 
const AutoAuditHeader = ({ setShowAuditSection, questions, section, handlePercentage, selectedQuestion, setSelectedQuestion }) => {
 
    const { intlTranslator } = useAppContext()
    const [dropdown, setDropdown] = useState(false);
    // const {intlTranslator} = useAppContext()
    const [sectionHide, setSectionHide] = useState(false);
 
    // const [selectedQuestion, setSelectedQuestion] = useState(null);
    const [showAutoAuditPopup, setShowAutoAuditPopup] = useState(false);
    const [showAutoAuditWindow, setShowAutoAuditWindow] = useState(false);
 
    const [setHandlePercentage] = useState(0);
    const [newWindow, setNewWindow] = useState<Window | null>(null);
 
 
    const [show, setShow] = useState(true);
    const appCallData = useSelector((state: any) => state?.call)
 
 
    useEffect(() => {
        questions?.map((item, index) => {
            setSelectedQuestion((prevAnswers) => ({ ...prevAnswers, [item?.question]: item?.auditResponse }))
        });
    }, [questions]);
 
    const handleCloseSection = () => {
        setDropdown(!dropdown)
    };
 
    const handleCheckedOption = (question, answer) => {
        setSelectedQuestion((prevAnswers) => ({
            ...prevAnswers,
            [question]: answer
        }));
    };
 
    const [getBtnName, setGetBtnName] = useState("");
 
 
 
    const handlePopupHide = () => {
        setShow(false);
        setShowAutoAuditPopup(false);
    };
 
 
    // const handlePopupHide = () => {
    //     setShow(false);
    //     setShowAutoAuditPopup(false);
    // };
 
    // const handleCheckedOption = (question, answer) => {
    //     setSelectedQuestion((prevAnswers) => ({
    //         ...prevAnswers,
    //         [question]: answer
    //     }));
    // };
 
   
    const handleGetBtnName = (e) => {
        console.log('Button clicked:', e.target.textContent);
        // Perform any additional logic
        setGetBtnName(e.target.textContent); // Example function call
    };
 
    useEffect(() => {
        setShowAuditSection(true);
        setSectionHide(true);
    }, [])
 
    const handleAutoAuditSection = () => {
        setSectionHide(!sectionHide);
        setShowAuditSection(!sectionHide);
    };
    const handlePopup = () => {
        setShowAutoAuditPopup(true);
    };
    const urls = process.env.NEXT_PUBLIC_NEXTAUTH_URL;
   
    const
        css =
            `
    @import url("https://fonts.googleapis.com/css2?family=Roboto:ital,wght@0,100;0,300;0,400;0,500;0,700;0,900;1,100;1,300;1,400;1,500;1,700;1,900&display=swap");
 
*,
*::before,
*::after {
  -webkit-box-sizing: border-box;
  box-sizing: border-box;
}
 
body {
  margin: 0;
  font-family: "Roboto";
  color: #2A2A2A;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
}
 
code {
  font-family: source-code-pro, Menlo, Monaco, Consolas, "Courier New",
    monospace;
}
 
 
    .auto-audit-header-container {
    display: flex;
    flex: 0.15;
    justify-content: space-between;
    align-items: center;
    background-color: #fff;
    box-shadow: 0px 1px 1px 1px rgba(222, 222, 222, 0.65);
    border-radius: 6px 6px 0 0;
    margin-bottom: 8px;
    padding: 8px 1rem 6px;
    border: 1px solid #ebf1fb;
    gap: 10px;
    position: sticky;
}
 
.auto-audit-header-container-inactive {
    flex: 0.15;
    margin-bottom: 0px;
}
 
.auto-audit-details-container {
    display: flex;
    flex-direction: column;
    flex: 2.85;
    padding: 5px 30px;
    gap: 10px;
    overflow-y: auto;
}
 
.auto-audit-header-leftside {
    display: flex;
    align-items: center;
    gap: 10px;
    font-size: 15px;
    font-weight: 700;
}
 
.auto-audit-header-rightside {
    display: flex;
    gap: 10px;
    color: #A09FA3;
    align-items: center;
}
 
.auto-audit-note-icon {
    color: #A09FA3;
    width: 13.5px;
    height: 17.71px;
    top: 3px;
    left: 5px;
    gap: 0px;
    opacity: 0px;
}
 
.auto-audit-list-icon {
    color: #946ECB;
    width: 18.67px;
    height: 24.89px;
    top: 1.56px;
    left: 4.67px;
    gap: 0px;
    opacity: 0px;
}
 
.auto-audit-dropdown-button {
    color: #946ECB;
}
 
.auto-audit-details-section-header {
    display: flex;
    flex: 1;
    justify-content: space-between;
    /* font-size: 16px; */
    /* font-weight: 600px; */
    font-weight: bolder;
    color: #272727;
}
 
.auto-audit-icon-section {
    display: flex;
    justify-content: space-between;
    align-items: center;
    gap: 10px;
}
 
.auto-audit-icon-style {
    color: #A09FA3;
    scale: 0.75;
}
 
.auto-audit-icon-style-check {
    color: #946ECB;
    scale: 0.75;
}
 
.auto-audit-icon-style-fail {
    color: #FF8A65;
    scale: 0.75;
}
 
.auto-audit-details-section-details-question {
    color: #A09FA3;
    font-size: 12px;
    font-weight: 700;
    line-height: 16.8px;
}
 
.auto-audit-radio-item-div {
    display: flex;
    flex: 1;
    gap: 8px;
    align-items: center;
    margin-bottom: 10px;
}
 
.auto-audit-radio-popup-item-div {
    display: flex;
    flex: 1;
    gap: 15px;
    align-items: center;
    margin-bottom: 10px;
}
 
.auto-audit-form-check {
    accent-color: #946ECB;
}
 
.auto-audit-popup-container {
    display: flex;
    flex: 1;
    flex-direction: column;
    transform: translate(-125px, 75px);
    margin-left: 20%;
        border: 5px solid #A09FA3;
}
 
.auto-audit-popup-container .modal-content {
    width: 750px;
    height: 420px;
 
}
 
.auto-audit-popup-header-container {
    display: flex;
    align-items: center;
    background-color: #fff;
    justify-content: space-between;
    box-shadow: 0px 1px 1px 1px rgba(222, 222, 222, 0.65);
    border-radius: 6px 6px 0 0;
    padding: 8px 1rem 6px;
    border-bottom: 5px solid #A09FA3;
}
 
.auto-audit-popup-header-container .btn-close {
    scale: 0.8;
}
 
.auto-audit-popup-details-container {
    display: flex;
    flex: 7;
    padding: 0px;
}
 
.auto-audit-popup-details-container {
    display: flex;
    flex: 1;
    height: 50vh;
}
 
.auto-audit-popup-header-leftside {
    display: flex;
    align-items: center;
    gap: 10px;
    font-size: 16px;
    font-weight: 700;
}
 
.auto-audit-popup-button {
    display: flex;
    justify-content: space-between;
    padding: 10px 20px 10px 20px;
    font-size: 13px;
    font-weight: 700;
    margin-right: -10px;
}
 
.auto-audit-popup-details-leftside {
    display: flex;
    flex: 3;
    flex-direction: column;
    margin: 1rem 0;
}
 
.auto-audit-popup-details-rightside {
    flex: 4;
    padding: 1.75rem 2rem 1rem;
    border-left: 5px solid #A09FA3;
    margin-left: 10px;
}
 
.button-background {
    background-color: #e8e7e8;
}`
        ;
 
        useEffect(() => {
           console.log(getBtnName+"nandan");
        }, [getBtnName])
 
 
    const handleNewWindow = () => {
        // Open a new window
        const newWindow = window.open(`${urls}/auto-audit`, '_blank', 'width=800,height=600');
        if (newWindow) {
            // Write a basic structure to the new window
            newWindow.document.write(` <html>
                <head>
                    <style>${css}</style>
                </head>
                <body>
                    <div id="auto-audit-root"></div>
                    <script>
                        // function handleGetBtnName(e) {
                        //     window.opener.handleGetBtnName(e);
 
                        // }
 
                        const handleGetBtnName = (e) => {
        console.log('Button clicked:', e.target.textContent);
        // Perform any additional logic
        setGetBtnName(e.target.textContent); // Example function call
    };
 
                    </script>
                </body>
            </html>`);
            newWindow.document.title = "Auto Audit";
 
            // Wait until the DOM is ready in the new window
            const rootElement = newWindow.document.getElementById('auto-audit-root');
            if (rootElement) {
                ReactDOM.createRoot(rootElement).render(
 
                    <Provider store={store}>
                        <div
                            className="auto-audit-popup-container"
                        >
                            <div className="auto-audit-popup-header-container" >
                                <div className="auto-audit-popup-header-leftside">
                                    <Image
                                        className="auto-audit-note-icon"
                                        src={AUTOAUDIT_ICON}
                                        alt="AutoAuditIcon"
                                    />
                                    {AUTO_AUDIT_DETAILS?.HEADING}
                                </div>
                            </div>
                            <div className="auto-audit-popup-details-container">
                                <div className="auto-audit-popup-details-leftside">
                                    {appCallData?.callAudits?.map((item, index) => (
                                        <button
                                        key={index}
                                        className={
                                          getBtnName === item.section
                                            ? 'auto-audit-popup-button button-background'
                                            : 'auto-audit-popup-button'
                                        }
                                        value={item?.section}
                                        onClick={(e) => {
                                            // Define the click handler in the new window's context
                                            handleGetBtnName(e);
                                            console.log('Button clicked:', e.target);
                                        }}
                                    >
                                        {item?.section}
                                        <RenderIcon status={item?.auditStatus} />
                                      </button>
                                    ))}
                                </div>
                                <div className="auto-audit-popup-details-rightside">
 
                                    {appCallData?.callAudits?.find((item, index) => (item.section) === getBtnName)?.questions.map((element, index) => {
                                        console.log(element + "element");
                                        return (<>
                                            <div className="auto-audit-details-section-details-question" key={index}> {element?.question} </div>
                                            <div className="auto-audit-radio-popup-item-div">
                                                {element?.options.map((item, index) => (
                                                    <>
                                                        <input
                                                            className="auto-audit-form-check"
                                                            type="radio"
                                                            key={index}
                                                            name={`${element?.question}`}
                                                            value={item}
                                                            checked={item === element?.auditResponse}
                                                            onChange={(e) => handleCheckedOption(element?.question, e.target.value)}
                                                        />
                                                        <label className="auto-audit-form-check" htmlFor={element?.question}>
                                                            {item}
                                                       </label>
                                                    </>
                                                ))}
                                            </div>
                                        </>)
 
                                        // <>
                                        //     <div className="auto-audit-details-section-details-question" key={index}> {element?.question} </div>
                                        //     <div className="auto-audit-radio-popup-item-div">
                                        //         {element?.options.map((item, index) => (
                                        //             <>
                                        //                 <input
                                        //                     className="auto-audit-form-check"
                                        //                     type="radio"
                                        //                     key={index}
                                        //                     name={`${element?.question}`}
                                        //                     value={item}
                                        //                     checked={item === element?.auditResponse}
                                        //                     onChange={(e) => handleCheckedOption(element?.question, e.target.value)}
                                        //                 />
                                        //                 <label className="auto-audit-form-check" htmlFor={element?.question}>
                                        //                     {item}
                                        //                 </label>
                                        //             </>
                                        //         ))}
                                        //     </div>
                                        // </>
                                    })}
                                </div>
                            </div>
                        </div>
 
                        {/* <div className="auto-audit-details-section">
            <div className="auto-audit-details-section-header">
                {intlTranslator(section)}
                <span className="auto-audit-icon-section">
                    <RenderIcon status={status} />
                    <button onClick={handleCloseSection}> {!dropdown ? <FaAngleDown size={20} /> : <FaAngleUp size={20} />} </button>
                </span>
            </div>
            {dropdown &&
                <div className="auto-audit-details-section-details">
                    {questions?.map((element, index) => (
                        <span >
                            <span className="auto-audit-details-section-details-question">{intlTranslator(element?.question)}</span>
                            <div key={index} className="auto-audit-radio-item-div">
                                {element?.options.map((item, index) => (
                                    <>
                                        <input
                                            className="auto-audit-form-check"
                                            type="radio"
                                            name={`${element?.question}`}
                                            value={item}
                                            defaultChecked={selectedQuestion[element?.question] === item}
                                            onChange={(e) => handleCheckedOption(element?.question, e.target.value)}
                                        />
                                        <label className="auto-audit-form-check" htmlFor={element?.question}>
                                            {intlTranslator(item)}
                                        </label>
                                    </>
                                ))}
                            </div>
                        </span>
                    ))}
                </div>
            }
        </div> */}
                    </Provider>
 
 
                );
            }
        }
 
    };
 
 
    return (
        <div className={!sectionHide ? "auto-audit-header-container component-box-shadow" : "auto-audit-header-container auto-audit-header-container-inactive component-box-shadow"}>
            <div className="auto-audit-header-leftside">
                <Image
                    className="auto-audit-note-icon"
                    src={AUTOAUDIT_ICON}
                    alt="AutoAuditIcon"
                />
                {HEADERS.AUTO_AUDIT}
            </div>
            <div className="auto-audit-header-rightside">
                {handlePercentage}%
                <button onClick={handleNewWindow}>
                    {/* <button onClick={handlePopup}>  */}
                    {/* <button> */}
                    {<Image
                        className="auto-audit-list-icon"
                        src={AUTOAUDIT_LIST_ICON}
                        alt="AutoAuditListIcon"
                    />}
                </button>
                <button className="auto-audit-dropdown-button"> {!sectionHide ? <FaAngleDown size={20} onClick={handleAutoAuditSection} /> : <FaAngleUp size={20} onClick={handleAutoAuditSection} />} </button>
            </div>
        </div>
    )
};
 
export default AutoAuditHeader;
 
function handleCheckedOption(question: any, value: string): void {
    throw new Error("Function not implemented.");
}
 
