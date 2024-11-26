import React, { useState, useRef, useEffect } from 'react';
import { useDispatch, useSelector } from 'react-redux';
import { uploadToS3 } from '../../../utility/text2speech/uploadAudioToS3';
import { FILE_UPLOADED, AUDIO_FILE_SPEAKER } from '../../../utility/constants';
import { setProgressBarAudio } from "../../../store/callSlice";
import { removeStatus } from '../../../store/speech-slice';
import { updateActivePage } from '../../../store/userSlice';
import TranscriptionStatus from '../text2json/Status';
import Download from '../audio2json/Download';
import VideotoJson from '../video2json/videotoJson';
import TexttoSpeech from '../text2json/text2Speech';

const MergedMediaSelection = ({audio, setAudio}) => {
  // State hooks for tab functionality
  const [activeTab, setActiveTab] = useState('audio'); // Default tab is 'audio'

  // State hooks for file upload and trimming
  const [notification, setNotification] = useState('');
  const [file, setFile] = useState(undefined);
  const [selectedFile, setSelectedFile] = useState(false);
  const [isFileUploaded, setIsFileUploaded] = useState(false);
  const [firstSpeaker, setFirstSpeaker] = useState(AUDIO_FILE_SPEAKER.FIRST_SPEAKER);
  const [audioFile, setAudioFile] = useState(null);
  const [trimmedAudioFile, setTrimmedAudioFile] = useState(null);
  const [startTime, setStartTime] = useState(0);
  const [endTime, setEndTime] = useState(0);

  const audioRef = useRef(null);
  const trimmedAudioRef = useRef(null);

  const dispatch = useDispatch();
  const status = useSelector(state => state?.speechstate?.status || '');
  const appCallData = useSelector((state) => state?.call);

  // Handle file upload
  const handleFileUpload = (event) => {
    const uploadedFile = event.target.files[0];
    console.log(uploadedFile,"test008")

    dispatch(setProgressBarAudio(URL.createObjectURL(event.target.files[0])))
    setAudio(event.target.files[0]?event.target.files[0]:appCallData.progressBarAudio)
    setFile(uploadedFile);
    if (uploadedFile && uploadedFile.type === 'audio/mpeg') {
      setAudioFile(URL.createObjectURL(uploadedFile));
      setTrimmedAudioFile(null); // Reset trimmed audio on new upload
      setSelectedFile(true);
    } else {
      setNotification(FILE_UPLOADED.NOT_VALID);
      setTimeout(() => setNotification(''), 8000);
      setFile(undefined);
      setSelectedFile(false);
    }
  };

  useEffect(()=>{
    console.log(appCallData.progressBarAudio,"test008")
    setAudioFile(appCallData.progressBarAudio)
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

  // Convert AudioBuffer to WAV Blob
  const bufferToWave = (abuffer, len) => {
    const numOfChan = abuffer.numberOfChannels;
    const length = len * numOfChan * 2 + 44;
    const buffer = new ArrayBuffer(length);
    const view = new DataView(buffer);
    const channels = [];
    let pos = 0;

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

  // Handle file submission to S3 (same as in AudiotoJson)
  const handleFileSubmit = async () => {
    // if (!file) return;

    try {
      const buffer = await file.arrayBuffer();
      uploadToS3(buffer, dispatch, firstSpeaker);
      setIsFileUploaded(true);
      setSelectedFile(false);
    } catch (err) {
      console.error("File type is not valid", err);
    }
  };

  return (
    <div className="container">
      <div className="main-body-container">
        <div className="tabs">
          <button
            className={activeTab === 'audio' ? 'active' : ''}
            onClick={() => setActiveTab('audio')}
          >
            Audio to JSON
          </button>
          <button
            className={activeTab === 'video' ? 'active' : ''}
            onClick={() => setActiveTab('video')}
          >
            Video to JSON
          </button>
          <button
            className={activeTab === 'text' ? 'active' : ''}
            onClick={() => setActiveTab('text')}
          >
            Text to JSON
          </button>
        </div>

        {/* Conditional rendering of content based on active tab */}
        {activeTab === 'audio' && (
          <div className="audio-tab-content">
            {/* File Upload and Speaker Selection */}
            <form className="upload-body-container">
              {!isFileUploaded && (
                <>
                  <div className="speaker-selection-container1">
                    <label>Who is Speaking first?</label>
                    <select
                      className="speaker-selection"
                      value={firstSpeaker}
                      onChange={(e) => setFirstSpeaker(e.target.value)}
                    >
                      <option value="Agent">Agent</option>
                      <option value="Customer">Customer</option>
                    </select>
                  </div>
                  <label htmlFor="audio-file" className="upload-btn1 tab btn-active">
                    + Upload
                  </label>
                  <input
                    className="file-upload"
                    id="audio-file"
                    type="file"
                    accept=".mp3"
                    onChange={handleFileUpload}
                  />
                </>
              )}

              {notification && <p className="file-notification">{notification}</p>}
            </form>

            {/* Uploaded File Preview and Trimming */}
            { appCallData.progressBarAudio && (
              <div className="media-selection-audio-trimmer">
                <h3>Uploaded Audio Preview</h3>
                <audio controls src={appCallData.progressBarAudio.name?URL.createObjectURL(appCallData.progressBarAudio):appCallData.progressBarAudio} ref={audioRef} />
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
                  <button className="trim-btn" onClick={handleTrim}>
                    Trim Audio
                  </button>
                </div>
              </div>
            )}

            {/* Trimmed Audio Preview */}
            {trimmedAudioFile && (
              <div>
                <h3>Trimmed Audio Preview</h3>
                <audio controls src={trimmedAudioFile} ref={trimmedAudioRef} />
              </div>
            )}

            {/* Submit Button */}
            {appCallData.progressBarAudio && (
              <button className="submit-btn" onClick={handleFileSubmit}>
                Submit
              </button>
            )}

            {/* Transcription Status */}
            {status.length !== 0 && (
              <div className="upload-body-child">
                <TranscriptionStatus />
              </div>
            )}

            {/* Download Component */}
            <Download/>
          </div>
        )}
        {activeTab === 'video' && <VideotoJson/>}
        {activeTab === 'text' && <TexttoSpeech/>}
        {/* Add similar logic for 'video' and 'text' tabs if needed */}
      </div>
    </div>
  );
};

export default MergedMediaSelection;





.media-selection-container {
    font-family: Arial, sans-serif;
    padding: 20px;
    max-width: 600px;
    margin: auto;
    background-color: #ffffff;
    border-radius: 8px;
    box-shadow: 0 4px 8px rgba(0, 0, 0, 0.1);
}

.button-group {
    display: flex;
    gap: 10px;
    align-items: center;
    margin-bottom: 20px;
}

.button-group h2 {
    font-weight: 600;
    font-size: 16px;
    margin-right: 10px;
}

.button-group button {
    padding: 10px 15px;
    border: 1px solid #ddd;
    background-color: #f4f4f4;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s, transform 0.2s;
}

.button-group button.active {
    background-color: #af8cf6;
    color: white;
    border: none;
}

.button-group button:hover {
    background-color: #af8cf6;
    color: white;
    transform: translateY(-2px);
}

.media-selection-content {
    border: 1px solid #ddd;
    padding: 20px;
    margin-bottom: 20px;
    background-color: #fafafa;
    border-radius: 8px;
    box-shadow: 0 2px 4px rgba(0, 0, 0, 0.1);
}

.media-selection-content h3 {
    font-weight: 600;
    font-size: 16px;
    margin-bottom: 10px;
}

.dropdown-group {
    display: flex;
    flex-direction: column;
    gap: 15px;
    margin-bottom: 20px;
}

.dropdown-group label {
    display: flex;
    flex-direction: column;
    flex-basis: 100%;
}

.dropdown-group select {
    padding: 10px;
    margin-top: 5px;
    border-radius: 4px;
    border: 1px solid #ddd;
}

.media-selection-audio-trimmer h3 {
    margin: 10px 0;
}

.media-selection-audio-trimmer {
    background-color: #f8f8f8;
    padding: 15px;
    border-radius: 4px;
    box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
}

.media-trimmer {
    margin-bottom: 20px;
    padding: 10px;
    background-color: #f2f2f2;
    border-radius: 4px;
    box-shadow: 0 1px 2px rgba(0, 0, 0, 0.1);
}

.script-upload {
    margin-top: 10px;
}

.script-upload textarea {
    width: 100%;
    height: 100px;
    padding: 10px;
    border-radius: 4px;
    border: 1px solid #ddd;
    font-size: 14px;
}

.script-upload h4 {
    margin-bottom: 8px;
    font-weight: 600;
    font-size: 16px;
}

.create-resource-btn, .upload-media-btn {
    padding: 10px 20px;
    background-color: #28a745;
    color: white;
    border: none;
    border-radius: 4px;
    cursor: pointer;
    transition: background-color 0.3s, transform 0.2s;
}

.create-resource-btn:hover, .upload-media-btn:hover {
    background-color: #218838;
    transform: translateY(-2px);
}

/* Responsive Styles */
@media (max-width: 768px) {
    .button-group {
        flex-direction: column;
    }

    .dropdown-group {
        flex-direction: column;
    }

    .dropdown-group label {
        width: 100%;
    }
}
