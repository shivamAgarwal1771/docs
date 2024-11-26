import React, { useEffect, useState } from 'react'
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

// Importing ffmpeg.js
import { createFFmpeg, fetchFile } from '@ffmpeg/ffmpeg';

const ffmpeg = createFFmpeg({ log: true });

const AudiotoJson = () => {
  const [notification, setNotification] = useState('');
  const [message, setMessage] = useState('');
  const status = useSelector(state => state?.speechstate?.status || '')
  const [file, setFile] = useState(undefined)
  const [selectedFile, setSelectedFile] = useState(false);
  const [isFileUploaded, setIsFileUploaded] = useState(false);
  const [firstSpeaker, setFirstSpeaker] = useState(AUDIO_FILE_SPEAKER.FIRST_SPEAKER);
  const [trimmedAudio, setTrimmedAudio] = useState(null);
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

  const trimAudio = async () => {
    if (!file) return;

    // Load the ffmpeg library
    if (!ffmpeg.isLoaded()) {
      await ffmpeg.load();
    }

    // Load the uploaded audio file into ffmpeg
    ffmpeg.FS('writeFile', file.name, await fetchFile(file));

    // Define trim start and end time (in seconds)
    const startTime = 5; // start at 5 seconds
    const duration = 10; // trim the next 10 seconds

    // Run ffmpeg command to trim the audio
    await ffmpeg.run(
      '-i', file.name, 
      '-ss', `${startTime}`, // start time
      '-t', `${duration}`, // duration
      'output.mp3'
    );

    // Fetch the trimmed audio file from ffmpeg's file system
    const trimmedAudioData = ffmpeg.FS('readFile', 'output.mp3');

    // Create a blob URL for the trimmed audio
    const audioBlob = new Blob([trimmedAudioData.buffer], { type: 'audio/mpeg' });
    const audioUrl = URL.createObjectURL(audioBlob);

    // Set the trimmed audio file in state
    setTrimmedAudio(audioUrl);
  }

  const handleFileUpload = async () => {
    if (!trimmedAudio) {
      return;
    }

    try {
      const buffer = await fetch(trimmedAudio).then(res => res.arrayBuffer());
      uploadToS3(buffer, dispatch, firstSpeaker);
    } catch (err) {
      console.error("File upload failed", err);
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
                <input className='file-upload' id='audio-file' type='file' accept='.mp3' onChange={e => { setFile(e.target.files[0]); dispatch(setProgressBarAudio(e.target.files[0])) }} />
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
            {selectedFile && !trimmedAudio && (
              <button className='submit-btn' onClick={trimAudio}>Trim Audio</button>
            )}
            {trimmedAudio && (
              <div>
                <audio controls src={trimmedAudio}></audio>
                <button className='submit-btn' onClick={handleFileUpload}>Submit Trimmed Audio</button>
              </div>
            )}
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
