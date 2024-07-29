import { useState } from "react";
import AutoAuditDetails from "./auto-audit-details";
import AutoAuditHeader from "./auto-audit-header";

const AutoAudit = ({setShowAutoAuditPopup, selectedQuestion, setSelectedQuestion}) => {
    const [showAuditSection, setShowAuditSection] = useState(false);
    const [handlePercentage, setHandlePercentage] = useState(0);
    return (
        <div className={showAuditSection ? 'audit-section audit-section-hidden component-box-shadow' : 'audit-section component-box-shadow'}>
            <AutoAuditHeader setShowAuditSection={setShowAuditSection} setShowAutoAuditPopup={setShowAutoAuditPopup} handlePercentage={handlePercentage}/>
            {!showAuditSection && <AutoAuditDetails setHandlePercentage={setHandlePercentage} selectedQuestion={selectedQuestion} setSelectedQuestion={setSelectedQuestion}/>}
        </div>
    )
};

export default AutoAudit;import { useEffect } from "react";
import { AUTO_AUDIT } from "../../../utility/constants";
import AutoAuditSection from "./auto-audit-section";
import { useSelector } from "react-redux";

const AutoAuditDetails = ({ setHandlePercentage, selectedQuestion, setSelectedQuestion }) => {
    const appCallData = useSelector((state:any) => state?.call)

    const handleStatus = () => {
        const NoofQ = appCallData?.callAudits?.length;
        let passedQ = 0;
        {
            appCallData?.callAudits.map((item, index) => {
                if (item.auditStatus === "PASS") {
                    passedQ += 1
                };
            })
        }
        setHandlePercentage(NoofQ !== 0 ? ((passedQ / NoofQ) * 100).toFixed(2) : 0);
    };
    
    useEffect(() => {
        handleStatus()
    }, [appCallData?.callAudits]);

    return (
        <div className="auto-audit-details-container">
            {appCallData?.callAudits?.map((item, index) => (
                <AutoAuditSection section={item.section} questions={item.questions} status={item.auditStatus} selectedQuestion={selectedQuestion} setSelectedQuestion={setSelectedQuestion} key={index} />
            ))}
        </div>
    )
};

export default AutoAuditDetails;

import { useEffect, useState } from "react";
import { FaAngleDown, FaAngleUp } from "react-icons/fa";
import Image from "next/future/image";
import AUTOAUDIT_ICON from "../../../assets/img/agent/auto-audit-icon.svg";
import AUTOAUDIT_LIST_ICON from "../../../assets/img/agent/auto-audit-list-icon.svg";
import { AUTO_AUDIT_DETAILS, HEADERS } from "../../../utility/constants";
import {useAppContext} from "../../common/appContext"

const AutoAuditHeader = ({ setShowAuditSection, setShowAutoAuditPopup, handlePercentage }) => {
    const {intlTranslator} = useAppContext()
    const [sectionHide, setSectionHide] = useState(false);

    useEffect(()=>{
        setShowAuditSection(true);
        setSectionHide(true);
    },[])

    const handleAutoAuditSection = () => {
        setSectionHide(!sectionHide);
        setShowAuditSection(!sectionHide);
    };
    const handlePopup = () => {
        setShowAutoAuditPopup(true);
    };
    return (
        <div className={!sectionHide ? "auto-audit-header-container component-box-shadow" : "auto-audit-header-container auto-audit-header-container-inactive component-box-shadow"}>
            <div className="auto-audit-header-leftside">
                <Image
                    className="auto-audit-note-icon"
                    src={AUTOAUDIT_ICON}
                    alt="AutoAuditIcon"
                />
                {intlTranslator(HEADERS.AUTO_AUDIT)}
            </div>
            <div className="auto-audit-header-rightside">
                {handlePercentage}%
                <button onClick={() => handlePopup()}>
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

my index.tsx

