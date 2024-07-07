import { useCallback, useState, useEffect } from "react";
import { RecognizeTextCommand } from "@aws-sdk/client-lex-runtime-v2";
import { v4 as uuidv4 } from "uuid";
import moment from "moment";
 
import ChatboxContent from "./chatboxContent/chatboxContent";
import ChatboxHeader from "./chatboxHeader/chatboxHeader";
import ChatboxInput from "./chatboxInput/chatboxInput";
import styles from "./chatbox.module.css";
import { getLexParams, lexV2Client } from "../../lex/config";
// import { botMessages } from "../../App";
import CreditCard from "../creditCard/CreditCard";
import Carousel from "../carousel/carousel";
 
const Chatbox = ({ chats, setChats, hasEnded, setHasEnded }) => {
  // const [botMessageIndex, setBotMessageIndex] = useState(0);
  const [sessionId, setSessionId] = useState("");
  const [showCreditForm, setShowCreditForm] = useState(false);
  const [showCarousel, setShowCarousel] = useState(false);    // To handle Carousel
  const [carouselData, setCarouselData] = ([]);
  const [inputType, setInputType] = useState("");    // Store the intent captured from LEX
  const lastChat = chats[chats.length - 1];
 
  // const onOptionClick = useCallback(
  //   (option, type) => {
  //     let selected = option;
  //     if (type === "button") {
  //       selected = option.ButtonName;
  //     }
  //     if (
  //       lastChat.type === "confirmation" &&
  //       lastChat?.input?.inputText &&
  //       lastChat?.input?.masked
  //     ) {
  //       selected = option
  //         .split("")
  //         .map((el) => "•")
  //         .join("");
  //     }
  //     let newChats = [
  //       ...chats,
  //       { messages: [{ text: selected }], sender: "user" },
  //       { isLoading: true, sender: "bot" },
  //     ];
  //     setChats(newChats);
  //   },
  //   [chats, botMessageIndex]
  // );
 
  // useEffect(() => {
  //   if (chats.length && lastChat.isLoading) {
  //     setTimeout(() => {
  //       let newChats = [...chats];
  //       const nextBotChat = botMessages[botMessageIndex + 1];
  //       if (nextBotChat.type) {
  //         newChats[newChats.length - 1] = { ...nextBotChat, sender: "bot" };
  //       } else {
  //         newChats[newChats.length - 1] = {
  //           ...nextBotChat,
  //           endedTime: moment(new Date()).format("h:mm A"),
  //           hasEnded: true,
  //           sender: "bot",
  //         };
  //         setHasEnded(true);
  //       }
  //       setChats(newChats);
  //       setBotMessageIndex((prev) => prev + 1);
  //     }, 1000);
  //   }
  // }, [chats, lastChat]);
 
  // const startNewChat = useCallback(() => {
  //   setChats((prev) => [
  //     ...prev,
  //     { hasStarted: true, startedTime: moment(new Date()).format("h:mm A") },
  //     { ...botMessages[0], sender: "bot" },
  //   ]);
  //   setBotMessageIndex(0);
  //   setHasEnded(false);
  // }, []);
 
  //  **** API CALLS ****** //
  const getInitialIntent = useCallback(async () => {
    const id = uuidv4();
    const params = getLexParams("Intial intent", id);
    try {
      const res = await lexV2Client.send(new RecognizeTextCommand(params));
      const content = JSON.parse(res?.messages?.[0]?.content || "{}");
      setChats([{ ...content, sender: "bot" }]);
      setSessionId(id);
 
      console.log("Response: ", res, "Content: ", content, "SessionID: ", sessionId);
    } catch (error) {
      console.error(`got an error :${error.message}`);
      throw error;
    }
  }, []);
 
  useEffect(() => {
    getInitialIntent();
  }, [getInitialIntent]);
 
  const onOptionClick = async (option, type, inputMessages) => {
    inputMessages = inputMessages || null;
 
    // console.log("Option: ", option, "Type: ", type, "Input Message: ", inputMessages);
 
    // console.log("Before: ", inputType);
 
    // if(
    //   lastChat.type === "confirmation" &&
    //   lastChat?.input?.inputText &&
    //   lastChat?.input?.inputType === "date"
    // ) {
    //   setInputType("date");
    // }
 
    // console.log("After: ", inputType);
 
    let selected = option,
      value = option;
    if (type === "button") {
      selected = option.ButtonValue;
      value = option.ButtonValue;
    }
    if (
      lastChat.type === "confirmation" &&
      lastChat?.input?.inputText &&
      lastChat?.input?.masked
    ) {
      selected = option
        .split("")
        .map(() => "•")
        .join("");
    }
    let newChats = [
      ...chats
    ];
 
    if (inputMessages) {
      inputMessages.forEach(msg => {
        newChats.push({ messages: [msg], sender: "bot" })
      })
    }
 
    newChats.push({ messages: [{ text: selected }], sender: "user" })
    newChats.push({ isLoading: true, value, sender: "bot" })
 
    setChats(newChats);
  };
 
  useEffect(()=>{
    console.log(carouselData,"aman")
  },[carouselData]);
 
  useEffect(() => {
    if (chats.length && lastChat?.isLoading) {
      (async function () {
        const params = getLexParams(lastChat?.value, sessionId);
 
        try {
          const res = await lexV2Client.send(new RecognizeTextCommand(params));
          const content = JSON.parse(res?.messages?.[0]?.content || "{}");
          let newChats = [...chats];
 
          console.log("Content: ", content);
          console.log("Chats: ", newChats);
 
          console.log("Before: ", inputType);
 
          if(
            content.type === "confirmation" &&
            content?.input?.inputText &&
            content?.input?.inputType === "date"
          ) {
            setInputType("date");
          } else if(
            content.type === "confirmation" &&
            content?.input?.inputText &&
            content?.input?.masked
          ) {
            setInputType("password");
          } else if(
            content.type === "confirmation" &&
            content?.input?.inputText &&
            content?.input?.inputType === "upload"
          ) {
            setInputType("upload");
          } else if(
            content.type === "carousel" &&
            content?.input?.carousel
          ) {
            setShowCarousel("true");
            // console.log("Carousel Data: ", content.input.carousel);
            content.input.carousel.map((items, index) => {
              setCarouselData([...carouselData, items]);
            })
          }
 
          console.log("After: ", inputType);
          console.log("Shivam: ", carouselData);
 
          if (content.type) {
            newChats[newChats.length - 1] = { ...content, sender: "bot" };
          } else {
            newChats[newChats.length - 1] = {
              ...content,
              endedTime: moment(new Date()).format("h:mm A"),
              hasEnded: true,
              sender: "bot",
            };
            setHasEnded(true);
          }
          setChats(newChats);
        } catch (error) {
          console.error(`got an error :${error.message}`);
          throw error;
        }
      })();
    }
  }, [chats, lastChat]);
 
  const startNewChat = useCallback(async () => {
    const params = getLexParams("Intial intent", sessionId);
    try {
      const res = await lexV2Client.send(new RecognizeTextCommand(params));
      const content = JSON.parse(res?.messages?.[0]?.content || "{}");
      setChats((prev) => [
        ...prev,
        { hasStarted: true, startedTime: moment(new Date()).format("h:mm A") },
        { ...content, sender: "bot" },
      ]);
      setHasEnded(false);
    } catch (error) {
      console.error(`got an error :${error.message}`);
      throw error;
    }
  }, [sessionId]);
 
  const onCreditCardClose = () => {
    let newChats = [
      ...chats,
      { messages: [{ text: "close" }], sender: "user" },
      { isLoading: true, sender: "bot", value: 'close' },
    ].filter((chat) => chat.type !== "creditCard");
    setChats(newChats);
    setShowCreditForm(false);
  };
 
  const onCarouselClose = () => {
    let newChats = [
      ...chats,
      { messages: [{ text: "close" }], sender: "user" },
      { isLoading: true, sender: "bot", value: 'close' },
    ].filter((chat) => chat.type !== "creditCard");
    setChats(newChats);
    setShowCarousel(false);
  };
 
  const displayChange = () => {
    if (showCreditForm) {
      return <CreditCard onClose={onCreditCardClose} />;
    } else if (showCarousel) {
      // return <Carousel />;
      // return <Carousel onClose={onCarouselClose} data={carouselData} />;
    } else {
      return <>
                <ChatboxContent
                  chats={chats}
                  onOptionClick={onOptionClick}
                  hasEnded={hasEnded}
                  startNewChat={startNewChat}
                  setShowCreditForm={setShowCreditForm}
                />
 
                <ChatboxInput lastChat={lastChat} onOptionClick={onOptionClick} type={inputType}/>
              </>;
    }
  }
 
  return (
    <main
      className={styles.wrapper}
      style={{ top: chats.length > 1 || showCreditForm || showCarousel ? "50px" : "25%" }}
    >
      <>
        <ChatboxHeader
          size={chats.length > 1 || showCreditForm || showCarousel ? "sm" : "lg"}
          showCredit={showCreditForm}
        />
 
        {displayChange()}
 
      </>
    </main>
  );
};
 
export default Chatbox;
 
has context menu
