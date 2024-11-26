import React, { useEffect, useState, useRef } from 'react'
import { useDispatch, useSelector } from 'react-redux'
import { updateActivePage } from '../../../store/userSlice'
import Header from '../Header';
import TranscriptionStatus from '../text2json/Status';
import Download from './Download';
import { uploadToS3 } from '../../../utility/text2speech/uploadAudioToS3';
import { FILE_UPLOADED } from '../../../utility/constants';
import { AUDIO_FILE_SPEAKER } from '../../../utility/constants';
import { removeStatus } from '../../../store/speech-slice'
import { setProgressBarAudio } from "../../../store/callSlice"

const AudiotoJson = () => {
  const [notification, setNotification] = useState('');
  const [message, setMessage] = useState('');
  const [audioFile, setAudioFile] = useState(null);
  const status = useSelector(state => state?.speechstate?.status || '')
  const [trimmedAudioFile, setTrimmedAudioFile] = useState(null); // For storing trimmed audio
  const [startTime, setStartTime] = useState(0);
  const [endTime, setEndTime] = useState(0);
  const audioRef = useRef(null);
  const trimmedAudioRef = useRef(null);
  const [file, setFile] = useState(undefined)
  const [selectedFile, setSelectedFile] = useState(false);
  const [isFileUploaded, setIsFileUploaded] = useState(false);
  const [firstSpeaker, setFirstSpeaker] = useState(AUDIO_FILE_SPEAKER.FIRST_SPEAKER);
  const dispatch = useDispatch()

  useEffect(() => {
    dispatch(removeStatus())
  }, [])

  const handleTrim = () => {
    if (!audioFile) {
      alert('Please upload an audio file first.');
      return;
    }

    const audio = audioRef.current;
    if (audio) {
      const start = parseFloat(startTime);
      const end = parseFloat(endTime);

      if (start >= 0 && end > start && end <= audio.duration) {
        const audioContext = new AudioContext();
        fetch(audioFile)
          .then((response) => response.arrayBuffer())
          .then((arrayBuffer) => audioContext.decodeAudioData(arrayBuffer))
          .then((audioBuffer) => {
            const trimmedBuffer = audioContext.createBuffer(
              audioBuffer.numberOfChannels,
              (end - start) * audioBuffer.sampleRate,
              audioBuffer.sampleRate
            );

            for (let channel = 0; channel < audioBuffer.numberOfChannels; channel++) {
              const oldData = audioBuffer.getChannelData(channel);
              const newData = trimmedBuffer.getChannelData(channel);
              newData.set(oldData.slice(
                start * audioBuffer.sampleRate,
                end * audioBuffer.sampleRate
              ));
            }

            const audioBlob = bufferToWave(trimmedBuffer, trimmedBuffer.length);
            const trimmedAudioUrl = URL.createObjectURL(audioBlob);
            setTrimmedAudioFile(trimmedAudioUrl);
          })
          .catch((error) => {
            console.error('Error decoding audio', error);
          });
      } else {
        alert('Invalid trim times. Please ensure they are within the audio duration.');
      }
    }
  };

  const bufferToWave = (abuffer, len) => {
    const numOfChan = abuffer.numberOfChannels;
    const length = len * numOfChan * 2 + 44;
    const buffer = new ArrayBuffer(length);
    const view = new DataView(buffer);
    const channels = [];
    let pos = 0;

    // Write WAV container metadata
    function setUint16(data) {
      view.setUint16(pos, data, true);
      pos += 2;
    }

    function setUint32(data) {
      view.setUint32(pos, data, true);
      pos += 4;
    }

    setUint32(0x46464952); // "RIFF" marker
    setUint32(length - 8); // RIFF chunk length
    setUint32(0x45564157); // "WAVE" marker
    setUint32(0x20746d66); // "fmt " chunk
    setUint32(16); // Length of format chunk
    setUint16(1); // Type of format (1 is PCM)
    setUint16(numOfChan);
    setUint32(abuffer.sampleRate);
    setUint32(abuffer.sampleRate * 2 * numOfChan);
    setUint16(numOfChan * 2);
    setUint16(16); // 16-bit samples
    setUint32(0x61746164); // "data" marker
    setUint32(length - pos - 4); // Data chunk length

    for (let i = 0; i < abuffer.numberOfChannels; i++) {
      channels.push(abuffer.getChannelData(i));
    }

    for (let i = 0; i < len; i++) {
      for (let c = 0; c < numOfChan; c++) {
        const sample = Math.max(-1, Math.min(1, channels[c][i]));
        view.setInt16(pos, sample < 0 ? sample * 0x8000 : sample * 0x7fff, true);
        pos += 2;
      }
    }

    return new Blob([buffer], { type: 'audio/wav' });
  };

  useEffect(() => {
    if (!file) {
      setNotification(FILE_UPLOADED.NOT_UPLOADED)
      setTimeout(() => {
        setNotification('')
      }, [5000]);
      return;
    }

    if (file) {
      if (file?.type !== 'audio/mpeg' && !file?.name?.endsWith('.mp3')) {
        setNotification(FILE_UPLOADED.NOT_VALID)
        setTimeout(() => {
          setNotification('')
        }, [8000]);
        setFile(undefined)
        setSelectedFile(false);
        return;
      }
      setSelectedFile(true);
    }
  }, [file])

  const handleFileUpload = async () => {
    if (!file) {
      return;
    }

    try {
      const buffer = await file.arrayBuffer()
      uploadToS3(buffer, dispatch, firstSpeaker)
    } catch (err) {
      console.error("File type is not valid", err)
    }
    setIsFileUploaded(true);
    setSelectedFile(false);
  }

  return (
    <>
      <div className="container">
        <div className='main-body-container'>
          <form className='upload-body-container'>
            {!isFileUploaded && (
              <>
                <div className="speaker-selection-container1">
                  <label htmlFor="">Who is Speaking first?</label>
                  <select className='speaker-selection'
                    value={firstSpeaker}
                    onChange={(e) => setFirstSpeaker(e.target.value)}>
                    <option value='Agent'>Agent</option>
                    <option value='Customer'>Customer</option>
                  </select>
                </div>
                <label htmlFor='audio-file' className='upload-btn1 tab btn-active'>+ Upload</label>
                <input className='file-upload' id='audio-file' type='file' accept='.mp3' onChange={e => { setAudioFile(URL.createObjectURL(e.target.files[0])); setFile(e.target.files[0]); dispatch(setProgressBarAudio(e.target.files[0])) }} />
                {audioFile && (
                  <div className='media-selection-audio-trimmer'>
                    <h3>Uploaded Audio Preview</h3>
                    <audio controls src={audioFile} ref={audioRef}>
                      Your browser does not support the audio element.
                    </audio>

                    <h3>Trim Audio</h3>
                    <div className="media-trimmer">
                      <label>
                        Start Time (seconds):
                        <input
                          type="number"
                          min="0"
                          step="0.1"
                          value={startTime}
                          onChange={(e) => setStartTime(e.target.value)}
                        />
                      </label>
                      <label>
                        End Time (seconds):
                        <input
                          type="number"
                          min="0"
                          step="0.1"
                          value={endTime}
                          onChange={(e) => setEndTime(e.target.value)}
                        />
                      </label>
                    </div>

                    <button style={{ background: "#af8cf6", color: "white", padding: "4px" }} onClick={handleTrim}>Trim Audio</button>
                  </div>
                )}
                {trimmedAudioFile && (
                  <div>
                    <h3>Trimmed Audio Preview</h3>
                    <audio controls src={trimmedAudioFile} ref={trimmedAudioRef}>
                      Your browser does not support the audio element.
                    </audio>
                  </div>
                )}
              </>
            )}
            {notification && <p className='file-notification'>{notification}</p>}
          </form>
          <div className="uploading-files-container">
            {file && (
              <div className="uploaded-file-details">
                <span><strong>{file.name}</strong></span>
              </div>
            )}
            {selectedFile && <button className='submit-btn' onClick={handleFileUpload}>Submit</button>}
          </div>
          {status.length !== 0 && <div className='upload-body-child'>
            <TranscriptionStatus message={message} />
          </div>}
        </div>
      </div>
      <Download setMessage={(e) => (setMessage(e))} />
    </>
  )
}

export default AudiotoJson;



