import { memo, useMemo } from "react";
import { AiOutlineArrowDown } from "react-icons/ai";
import styles from "./chatboxContent.module.css";
import moment from "moment";

import Image from "../../image/Image";
import Loader from "../../message/Loader";
import Message from "../../message/Message";
import Video from "../../video/Video";
import HorizontalButtons from "../../horizontalButtons/HorizontalButtons";
import CreditCard from "../../creditCard/CreditCard";
import TableFormat from "../../table/table";

const ChatboxContent = ({
  chats,
  onOptionClick,
  hasEnded,
  startNewChat,
  setShowCreditForm,
}) => {
  var imgURL;
  // const endedTime = useMemo(
  //   () => (hasEnded ? moment(new Date()).format("h:mm A") : ""),
  //   [hasEnded]
  // );
chats?.map(data=>
  {if(data.value)
    {
      if(data.value.includes("http"))
        {
          imgURL=data.value
        }
    }})
  const downloadTranscript = () => {
    const transcript = chats?.map(entry => entry?.messages?.map(message => `${entry.sender}: ${message.text}`).join('\n')).join('\n');
    const blob = new Blob([transcript], { type: 'text/plain' });
    const url = URL.createObjectURL(blob);
    const anchor = document.createElement('a');
    anchor.href = url;
    anchor.download = "conversation-transcript.txt"
    document.body.appendChild(anchor);
    anchor.click();
    URL.revokeObjectURL(url);
    document.body.removeChild(anchor);
  }

  return (
    <div
      className={styles.wrapper}
      style={{
        justifyContent: chats.length > 1 ? "start" : "end",
        height: hasEnded ? "535px" : chats.length > 1 ? "445px" : "275px",
        overflowY: "scroll",
      }}
    >
      {chats?.map((chat, chatInd) => {
        const { type, messages, input, sender, Table } = chat;
        if (type === "creditCard" && chats.length === chatInd + 1)
          setShowCreditForm(true);

        return (
          <div key={`chatbox-${chatInd}`}>
            <div>
              {chat?.hasStarted && (
                <p className={styles.started_time}>
                  Today at {chat?.startedTime}
                </p>
              )}

              <div
                className={styles.message_container}
                style={{
                  alignItems: chat.sender !== "bot" ? "end" : "start",
                  // marginBottom:
                  //   !input.inputText && input.align === "Vertical"
                  //     ? "1rem"
                  //     : "0px",
                }}
              >
                {chat?.isLoading && <Loader />}
                {messages?.map((message, ind) => (
                  <Message
                    key={ind}
                    {...message}
                    showRightTail={
                      messages.length === ind + 1 && sender !== "bot"
                    }
                    showLeftTail={
                      messages.length === ind + 1 && sender === "bot"
                    }
                  />
                ))}
              </div>
              <div>
                {Table && Table.length && <TableFormat table={Table} />}
                {type === "confirmation" &&
                  chats.length === chatInd + 1 &&
                  input.align === "Horizontal" && (
                    <>
                      {input.messages &&
                        input.messages?.map((message, ind) => (
                          <Message
                            key={ind}
                            {...message}
                            showRightTail={
                              messages.length === ind + 1 && sender !== "bot"
                            }
                            showLeftTail={
                              messages.length === ind + 1 && sender === "bot"
                            }
                          />
                        ))}
                      <HorizontalButtons
                        buttons={input.buttons}
                        onClick={onOptionClick}
                        inputMessages={"messages" in input ? input.messages : undefined}
                      />
                    </>
                  )}

                {/* {type === "card" && (
                  <>
                    {card?.images?.length && (
                      <Image
                        images={card.images}
                        buttons={card.buttons}
                        onClick={(value) => onOptionClick(value, "button")}
                      />
                    )}
                    {card?.videos?.length && (
                      <Video
                        videos={card.videos}
                        buttons={card.buttons}
                        onClick={(value) => onOptionClick(value, "button")}
                      />
                    )}
                  </>
                )} */}
              </div>

              {(chat?.hasEnded || chat?.endchat) && (
                <div style={{ color:"#696969" }}>
                  <div className={styles.ended_cont}>
                    <p className={styles.ended_time}>
                      Today at {chat?.endedTime}
                    </p>
                    <p>This conversation has ended.</p>
                  </div>
                  {chats.length === chatInd + 1 && (
                    <div className={styles.ended_buttons}>
                      <div className={styles.btn_cont}>           
                        <button className={styles.start_btn} onClick={downloadTranscript}>  
                          <AiOutlineArrowDown size={18} />
                          Download Transcript
                        </button>
                      </div>
                      <div className={styles.btn_cont}>
                        <button
                          className={styles.start_btn}
                          onClick={startNewChat}
                        >
                          Start New Conversation
                        </button>
                      </div>
                    </div>
                  )}
                </div>
              )}
            </div>
          </div>
        );
      })}
    </div>
  );
};

export default memo(ChatboxContent);
