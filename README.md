import { useEffect, useRef } from "react";
import styles from "./message.module.css";

export const BubbleTail = ({ position }) => {
  return (
    <svg
      className={
        position === "left" ? styles.bubble_tail_left : styles.bubble_tail_right
      }
      xmlns="http://www.w3.org/2000/svg"
      xmlnsXlink="http://www.w3.org/1999/xlink"
      width="13px"
      height="13px"
      viewBox="0 0 13 10"
      fill="#FF671F"
      aria-hidden="true"
    >
      <defs>
        <filter id="s7yyhd1d1a">
          <feColorMatrix in="SourceGraphic"></feColorMatrix>
        </filter>
      </defs>
      <g
        filter="url(#s7yyhd1d1a)"
        transform={
          position === "right"
            ? "translate(-1402 -548) translate(1310 508)"
            : "translate(-964 -488) translate(964 408)"
        }
      >
        <g>
          <path
            d="M13.032 0l-9.64 9.35c-.792.768-2.059.749-2.828-.044C.202 8.933 0 8.433 0 7.914V0h13.032z"
            transform={
              position === "right"
                ? "matrix(-1 0 0 1 105 40)"
                : "translate(0 80)"
            }
          ></path>
        </g>
      </g>
    </svg>
  );
};

const Message = ({ text, width = "75%", showRightTail, showLeftTail }) => {
  const messageRef = useRef(null);
  useEffect(() => {
    messageRef.current?.scrollIntoView({ behavior: "smooth" });
  }, [text]);
  text = text === "Thank you let me go ahead and check the policy and Provider info." ? "Thank you! Let me go ahead and check the policy information" : text;
  console.log(text,"shivam")
  return (
    <>
      <div
        className={styles.message}
        ref={messageRef}
        style={{ maxWidth: width, borderRadius: showLeftTail ? "8px 8px 8px 0" : "8px 8px 0 8px" }}
      >
        {text?.includes("video/")&&<video style={{width:"300px", height:"170px"}}><source src={text}/></video>}
        {text?.includes("audio/")&&<audio controls><source src={text}/></audio>}
        {text?.includes("application/pdf")&&<iframe src={text}/>} 
        {/* {text?.includes("openxmlformats")&&<iframe src={text}/>} */}
        {text?.includes("image/")&&<img src={text} style={{width:"300px", height:"170px"}}/>}
        {!text?.includes("image/")&&!text?.includes("audio/")&&!text?.includes("video/")&&!text?.includes("application/pdf")&&<div dangerouslySetInnerHTML={{ __html: text }} />}
        {(showLeftTail || showRightTail) && (
          <BubbleTail position={showLeftTail ? "left" : "right"} />
        )}
      </div>
    </>
  );
};

export default Message;
//{text?.includes("audio")?<audio controls><source src={text}/></audio>:<div dangerouslySetInnerHTML={{ __html: text }} />}

{/* {text?.includes("image")||text?.includes("audio")?<img src={text} style={{width:"300px", height:"170px"}}/>:<div dangerouslySetInnerHTML={{ __html: text }} />} */}
