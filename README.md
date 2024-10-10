import { useState } from 'react';
import Image from "next/future/image";
import ThumbsUp from "../../../assets/img/agent/thumbsUp.svg";
import ThumbsUpFill from "../../../assets/img/agent/thumbsUpFill.svg";
import ThumbsDown from "../../../assets/img/agent/thumbsDown.svg";
import ThumbsDownFill from "../../../assets/img/agent/thumbsDownFill.svg";
import { useAppContext } from "../../common/AppContext";
import { USER_EVENTS, LIKED_BUTTON_TYPE, ACTION_TYPE, COMPONENT_NAME } from "../../../utility/constants";

const AIGuidance = (props) => {
    const [activeBtn, setActiveBtn] = useState(null);
    const { userTrackingEvent } = useAppContext();
    let actionDetail = {
        component: COMPONENT_NAME.AI_GUIDANCE,
        status: "",
        entityName: props?.guidanceData?.entityName,
        index: props?.dataIndex
    }
    const handleBtnClick = (btnName) => {
        if(btnName === LIKED_BUTTON_TYPE.THUMB_DOWN){
            actionDetail.status = ACTION_TYPE.DISLIKED;
            userTrackingEvent(COMPONENT_NAME.AI_ASSISTANT, ACTION_TYPE.CLICK, actionDetail);
        } else {
            actionDetail.status = ACTION_TYPE.LIKED;
            userTrackingEvent(COMPONENT_NAME.AI_ASSISTANT, ACTION_TYPE.CLICK, actionDetail);
        }
        { activeBtn === btnName ? setActiveBtn(null) : setActiveBtn(btnName) }
    };
    let isRenderGuidanceComponent;
    { (props.guidanceData.entityName && props.guidanceData.entityValue) ? isRenderGuidanceComponent = true : isRenderGuidanceComponent = false 
    return (
        isRenderGuidanceComponent &&
        (<div className="ai-assist-guidance-layout">
            <div className="ai-assist-heading-container">
                <div className='heading-2'> {props.guidanceData?.entityName} </div>
                <div className='ai-guidance-feedback-button-container'>
                    <button className="thumb-down" onClick={() => handleBtnClick(LIKED_BUTTON_TYPE.THUMB_DOWN)} >
                        {activeBtn === LIKED_BUTTON_TYPE.THUMB_DOWN ?
                            <Image
                                src={ThumbsDownFill}
                                alt="ThumbsDownFillIcon"
                            />
                            :
                            <Image
                                src={ThumbsDown}
                                alt="ThumbsDownIcon"
                            />
                        }
                    </button>
                    <button className="thumb-up" onClick={() => handleBtnClick(LIKED_BUTTON_TYPE.THUMB_UP)}>
                        {activeBtn === LIKED_BUTTON_TYPE.THUMB_UP ?
                            <Image
                                src={ThumbsUpFill}
                                alt="ThumbsUpFillIcon"
                            />
                            :
                            <Image
                                src={ThumbsUp}
                                alt="ThumbsUpIcon"
                            />
                        }
                    </button>
                </div>
            </div>
            <div className='ai-guidance-content' dangerouslySetInnerHTML={{ __html: props.guidanceData?.entityValue }}>
            </div>
        </div>)
        )
    };
}

export default AIGuidance;
