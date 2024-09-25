import { useEffect, useState, useRef } from "react";
import { IoSendSharp } from "react-icons/io5";
import { FaMicrophone, FaMicrophoneSlash } from "react-icons/fa"; // For microphone icons
import styles from "./input.module.css";
import { CHAT_INPUT_TYPE } from "../../utility/constants";

// Check for SpeechRecognition API support
const SpeechRecognition = window.SpeechRecognition || window.webkitSpeechRecognition;

const Input = ({ secure, onSend, inputDisabled, input }) => {
  const inputRef = useRef(null);
  const [inputChanger, setInputChanger] = useState(input);
  const [showInputText, setShowInputText] = useState(false);
  const [value, setValue] = useState("");
  const [placeholder, setPlaceholder] = useState("");
  const [file, setFile] = useState(null);
  const [filePreview, setFilePreview] = useState(null);
  const [isListening, setIsListening] = useState(false); // For microphone state
  const recognitionRef = useRef(null); // SpeechRecognition instance reference

  const today = new Date();
  const thisDate = today.toJSON().slice(0, 10);

  const minDate = () => {
    let date = today.getDate();
    let month = today.getMonth();
    let year = today.getFullYear() - 100;

    if (date < 10) date = String(date).padStart(2, "0");
    if (month < 10) month = String(month).padStart(2, "0");

    return `${year}-${month}-${date}`;
  };

  const thisDateLocal = () => {
    let date = today.getDate();
    let month = today.getMonth();
    let year = today.getFullYear();
    let hour = today.getHours();

    if (date < 10) date = String(date).padStart(2, "0");
    if (month < 10) month is String(month).padStart(2, "0");
    if (hour < 10) hour = String(hour).padStart(2, "0");

    return `${year}-${month}-${date}T${hour}:00`;
  };

  const nxtDate = () => {
    let date = today.getDate();
    let month = today.getMonth() + 3;
    let year = today.getFullYear();
    let hour = today.getHours();

    if (date < 10) date = String(date).padStart(2, "0");
    if (month < 10) month is String(month).padStart(2, "0");
    if (hour < 10) hour is String(hour).padStart(2, "0");

    return `${year}-${month}-${date}T${hour}:00`;
  };

  useEffect(() => {
    if (!secure) {
      setShowInputText(true);
      setPlaceholder(inputDisabled ? "" : "Type message");
    }
    if (secure) setPlaceholder("Type your secure message");
    setValue("");
    if (inputRef.current) inputRef.current.focus();
  }, [secure]);

  const handleSubmit = (event) => {
    if (event.key === "Enter") {
      setShowInputText(true);
      onSend(value);
    }
  };

  const handleFileChange = (e) => {
    if (e?.target?.files) {
      setFile(e?.target?.files[0]);
    }
  };

  useEffect(() => {
    if (file) {
      const reader = new FileReader();
      reader.onload = (e) => {
        const url = e.target.result;
        setFilePreview(url);
      };
      reader.readAsDataURL(file);
    }
  }, [file]);

  const inputType = () => {
    if (input === CHAT_INPUT_TYPE.TEXT && showInputText) return CHAT_INPUT_TYPE.TEXT;
    if (input === CHAT_INPUT_TYPE.PASSWORD) return CHAT_INPUT_TYPE.PASSWORD;
    if (input === CHAT_INPUT_TYPE.DATE) return CHAT_INPUT_TYPE.DATE;
    if (input === CHAT_INPUT_TYPE.DATE_TIME) return CHAT_INPUT_TYPE.DATE_TIME;
  };

  useEffect(() => {
    // Initialize SpeechRecognition once
    if (SpeechRecognition) {
      recognitionRef.current = new SpeechRecognition();
      recognitionRef.current.continuous = true;
      recognitionRef.current.interimResults = true;
      recognitionRef.current.lang = "en-US";

      recognitionRef.current.onresult = (event) => {
        const currentTranscript = Array.from(event.results)
          .map((result) => result[0].transcript)
          .join("");
        
        // Accumulate the transcript in the value state
        setValue((prevValue) => prevValue + " " + currentTranscript);

        // Log spoken text to the console
        console.log("Spoken Text: ", currentTranscript);
      };

      recognitionRef.current.onerror = (event) => {
        console.error("Speech recognition error:", event.error);
        recognitionRef.current.stop();
        setIsListening(false);
      };

      recognitionRef.current.onend = () => {
        setIsListening(false);
      };
    } else {
      alert("Speech recognition is not supported in this browser.");
    }
  }, []);

  const handleAudioToggle = () => {
    if (!recognitionRef.current) return;

    if (isListening) {
      recognitionRef.current.stop();
      setIsListening(false);
    } else {
      recognitionRef.current.start();
      setIsListening(true);
    }
  };

  return (
    <>
      {inputChanger === CHAT_INPUT_TYPE.UPLOAD && (
        <div className={styles.upload_container}>
          <input type="file" className={styles.upload_btn} onChange={handleFileChange} />
          <button
            onClick={() => {
              filePreview && onSend(filePreview);
            }}
            className={styles.submit_btn}
            disabled={!file}
          >
            Submit
          </button>
        </div>
      )}
      {inputChanger !== CHAT_INPUT_TYPE.UPLOAD && (
        <div className={styles.wrapper}>
          <div className={styles.input_cont}>
            <input
              ref={inputRef}
              type={inputType()}
              placeholder={input === "date" ? "" : placeholder}
              className={styles.input}
              value={value}
              onChange={(e) => setValue(e.target.value)}
              onKeyPress={(e) => handleSubmit(e)}
              disabled={inputDisabled}
              min={input === "date" ? minDate() : thisDateLocal()}
              max={input === "date" ? thisDate : nxtDate()}
            />
          </div>

          {/* Audio-to-Text Button */}
          <button
            className={styles.mic_btn}
            onClick={handleAudioToggle}
          >
            {isListening ? <FaMicrophoneSlash size={21} /> : <FaMicrophone size={21} />}
          </button>

          <button
            className={styles.send_btn}
            disabled={!value}
            onClick={() => onSend(value, input)}
          >
            <IoSendSharp size={21} />
          </button>
        </div>
      )}
    </>
  );
};

export default Input;
