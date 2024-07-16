import { useEffect, useState, useRef } from "react";
import { IoSendSharp } from "react-icons/io5";
// import { FiEyeOff, FiEye } from "react-icons/fi";
import styles from "./input.module.css";
import { CHAT_INPUT_TYPE } from "../../utility/constants";

const Input = ({ secure, onSend, inputDisabled, input }) => {
  const inputRef = useRef(null);
  const [inputChanger, setInputChanger] = useState(input);
  const [showInputText, setShowInputText] = useState(false);
  const [value, setValue] = useState(false);
  const [placeholder, setPlaceholder] = useState("");
  const [file, setFile] = useState(null);
  const today = new Date();
  const maxDate = today.toJSON().slice(0, 10);

  const minDate = () => {
    let date = today.getDate();
    let month = today.getMonth() - 2;
    let year = today.getFullYear();

    if (date < 10) {
        date = String(date).padStart(2, '0');
    }
    if (month < 10) {
        month = String(month).padStart(2, '0');
    }

    return (year + '-' + month + '-' + date);
  }

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
  }
  
  const handleFileChange = (e) => {
    if (e?.target?.files) {
      setFile(e?.target?.files[0]);
    }
  };

  const inputType = () => {
    if (input === CHAT_INPUT_TYPE.TEXT && showInputText) {
      return CHAT_INPUT_TYPE.TEXT;
    }
    if (input === CHAT_INPUT_TYPE.PASSWORD) {
      return CHAT_INPUT_TYPE.PASSWORD;
    }
    if (input === CHAT_INPUT_TYPE.DATE) {
      return CHAT_INPUT_TYPE.DATE;
    }
  }

  // useEffect(() => {
  //   if (inputRef.current) inputRef.current.focus();
  // }, []);

  return (
    <>
      {inputChanger === CHAT_INPUT_TYPE.UPLOAD &&
        (<div className={styles.upload_container}>
          <input type="file" className={styles.upload_btn} onChange={handleFileChange}/>
          <button 
            onClick={() => onSend("file uploaded")} 
            className={styles.submit_btn}
            disabled={!file}
          >Submit</button>
        </div>)
      }
      {inputChanger !== CHAT_INPUT_TYPE.UPLOAD && 
        (<div className={styles.wrapper}>
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
              min={minDate()}
              max={maxDate}
            />
            
          </div>
          <button
            className={styles.send_btn}
            disabled={!value}
            onClick={() => onSend(value)}
          >
            <IoSendSharp size={21} />
          </button>
        </div>)
      }
    </>
  );
};

export default Input;
