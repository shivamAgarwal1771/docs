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
  const [isListening, setIsListening] = useState(false);
  const recognitionRef = useRef(null); // SpeechRecognition instance reference

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
        setValue((prevValue) => prevValue + " " + currentTranscript.trim());
        console.log("Spoken Text: ", currentTranscript);
      };

      recognitionRef.current.onerror = (event) => {
        console.error("Speech recognition error:", event.error);
      };

      recognitionRef.current.onend = () => {
        setIsListening(false);
        console.log("Speech recognition service disconnected");
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

  // Other unchanged methods here (like handleSubmit, handleFileChange, etc.)

  return (
    <>
      {/* Conditional rendering for upload input */}
      {inputChanger === CHAT_INPUT_TYPE.UPLOAD && (
        <div className={styles.upload_container}>
          <input type="file" className={styles.upload_btn} onChange={handleFileChange} />
          <button
            onClick={() => filePreview && onSend(filePreview)}
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
              placeholder={placeholder}
              className={styles.input}
              value={value}
              onChange={(e) => setValue(e.target.value)}
              onKeyPress={(e) => handleSubmit(e)}
              disabled={inputDisabled}
            />
          </div>
          <button className={styles.mic_btn} onClick={handleAudioToggle}>
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
