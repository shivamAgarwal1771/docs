import React, { useState, useRef, useEffect } from 'react';
import AudiotoJson from '../audio2json/audiotoJson';
import VideotoJson from '../video2json/videotoJson';
import TexttoSpeech from '../text2json/text2Speech';
import { useDispatch, useSelector } from 'react-redux';


const MediaSelection = ({ setAudio, audio }) => {
  const [selectedMedia, setSelectedMedia] = useState('demoRecording'); // Default selection
  const [audioFile, setAudioFile] = useState(null);
  const [trimmedAudioFile, setTrimmedAudioFile] = useState(null); // For storing trimmed audio
  const [startTime, setStartTime] = useState(0);
  const [endTime, setEndTime] = useState(0);
  const audioRef = useRef(null);
  const trimmedAudioRef = useRef(null);
  const appCallData = useSelector((state) => state?.call);

  // Handle file upload for audio
  const handleFileUpload = (event) => {
    setAudio(event.target.files[0]);
    const file = event.target.files[0];
    if (file && file.type.includes('audio')) {
      setAudioFile(URL.createObjectURL(file));
      setTrimmedAudioFile(null); // Reset trimmed audio on new upload
    } else {
      alert('Please upload a valid audio file.');
    }
  };

  
  useEffect(()=>{
    setTrimmedAudioFile(null);
    if(appCallData?.progressBarAudio?.name){
      setAudioFile(URL.createObjectURL(appCallData.progressBarAudio));
      setAudio(URL.createObjectURL(appCallData.progressBarAudio))
    }else{
      setAudioFile(appCallData.progressBarAudio);
      setAudio(appCallData.progressBarAudio)
    }

  },[appCallData.progressBarAudio])

  // Handle trimming and playing trimmed audio
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

  return (
    <div className="media-selection-container">
      <div className="button-group">
        <h2>Select Media:</h2>
        {/* <button
          className={selectedMedia === 'demoScript' ? 'active' : ''}
          onClick={() => setSelectedMedia('demoScript')}
        > */}
          {/* Demo Script
        </button> */}
        <button
          className={selectedMedia === 'demoRecording' ? 'active' : ''}
          onClick={() => setSelectedMedia('demoRecording')}
        >
          Demo Recording
        </button>
        <button
          className={selectedMedia === 'audio' ? 'active' : ''}
          onClick={() => setSelectedMedia('audio')}
        >
          Audio
        </button>
        <button
          className={selectedMedia === 'video' ? 'active' : ''}
          onClick={() => setSelectedMedia('video')}
        >
          Video
        </button>
        <button
          className={selectedMedia === 'text' ? 'active' : ''}
          onClick={() => setSelectedMedia('text')}
        >
          Text
        </button>
      </div>

      {/* Conditional Content Rendering */}
      {selectedMedia === 'audio' && (
        <AudiotoJson/>
      )}
         {selectedMedia === 'demoRecording' && (
        <div className="media-selection-content">
          <h3>Upload Audio Recording</h3>
          <input type="file" accept="audio/*" onChange={e=>handleFileUpload(e)} />

          {/* Preview uploaded audio */}
          {appCallData?.progressBarAudio && (
            <div className='media-selection-audio-trimmer'>
              <h3>Uploaded Audio Preview</h3>
              <audio controls src={URL.createObjectURL(appCallData?.progressBarAudio)} ref={audioRef}>
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

          {/* Preview trimmed audio */}
          {trimmedAudioFile && (
            <div>
              <h3>Trimmed Audio Preview</h3>
              <audio controls src={trimmedAudioFile} ref={trimmedAudioRef}>
                Your browser does not support the audio element.
              </audio>
            </div>
          )}
        </div>
      )}

      {selectedMedia === 'video' && (
        <div className="media-selection-content">
        <VideotoJson/>
        </div>
      )}

      {selectedMedia === 'text' && (
        <div className="media-selection-content">
    <TexttoSpeech/>
        </div>
      )}

      {selectedMedia === 'demoScript' && (
        <div className="media-selection-content">
          <h3>Select Voice</h3>
          <div className="dropdown-group">
            <label>
              Customer:
              <select>
                <option>Salli (Female)</option>
                <option>Matthew (Male)</option>
                <option>Kimberly (Female)</option>
                <option>Kendra (Female)</option>
                <option>Justin (Male)</option>
                <option>Joey (Male)</option>
                <option>Lvy (Female)</option>
              </select>
            </label>
            <label>
              Agent:
              <select>
                <option>Salli (Female)</option>
                <option>Matthew (Male)</option>
                <option>Kimberly (Female)</option>
                <option>Kendra (Female)</option>
                <option>Justin (Male)</option>
                <option>Joey (Male)</option>
                <option>Lvy (Female)</option>
              </select>
            </label>
          </div>
          <h3>Upload Script File</h3>
          <div className="script-upload">
            <a href="../../../public/sample/sample.pdf" download>
              Sample script
            </a>
            <textarea placeholder="Write your text here"></textarea>
            <input type="file" accept=".txt" />
          </div>
          <button className="create-resource-btn">Create Resource</button>
        </div>
      )}
    </div>
  );
};

export default MediaSelection;




import React, { useEffect, useState } from 'react'
import { useDispatch, useSelector } from 'react-redux'
import { updateActivePage } from '../../../store/userSlice'
import Header from '../Header';
import TranscriptionStatus from '../text2json/Status';
import Download from './Download';
import { uploadToS3 } from '../../../utility/text2speech/uploadAudioToS3';
import { FILE_UPLOADED } from '../../../utility/constants';
import {AUDIO_FILE_SPEAKER} from '../../../utility/constants';
import { removeStatus } from '../../../store/speech-slice'
import {setProgressBarAudio} from "../../../store/callSlice"
import MediaSelection from '../upload-demo/MediaSelection';

const AudiotoJson = () => {
  const [notification, setNotification] = useState('');
  const appCallData = useSelector((state) => state?.call);
  const [message, setMessage] = useState('');
  const status = useSelector(state => state?.speechstate?.status || '')
  const [file, setFile] = useState(undefined)
  const [selectedFile, setSelectedFile] = useState(false);
  const [isFileUploaded, setIsFileUploaded] = useState(false);
  const [firstSpeaker, setFirstSpeaker] = useState(AUDIO_FILE_SPEAKER.FIRST_SPEAKER);
  const dispatch = useDispatch()

  useEffect(() => {
    dispatch(removeStatus())
  }, [])

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

    try{
    const buffer = await file.arrayBuffer()
    uploadToS3(buffer, dispatch, firstSpeaker)
    }catch(err){
      console.error("File type is not valid",err)
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
                <input className='file-upload' id='audio-file' type='file' accept='.mp3' onChange={e => {setFile(e.target.files[0]); dispatch(setProgressBarAudio(e.target.files[0]))}} />
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



