import React, { useState, useRef } from 'react';
const MediaSelection = () => {
  const [selectedMedia, setSelectedMedia] = useState('demoScript'); // Default selection
  const [audioFile, setAudioFile] = useState(null);
  const [trimmedAudioFile, setTrimmedAudioFile] = useState(null); // For storing trimmed audio
  const [startTime, setStartTime] = useState(0);
  const [endTime, setEndTime] = useState(0);
  const audioRef = useRef(null);
  const trimmedAudioRef = useRef(null);

  // Handle file upload for audio
  const handleFileUpload = (event) => {
    const file = event.target.files[0];
    if (file && file.type.includes('audio')) {
      setAudioFile(URL.createObjectURL(file));
      setTrimmedAudioFile(null); // Reset trimmed audio on new upload
    } else {
      alert('Please upload a valid audio file.');
    }
  };

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
        <button
          className={selectedMedia === 'demoScript' ? 'active' : ''}
          onClick={() => setSelectedMedia('demoScript')}
        >
          Demo Script Radio Button
        </button>
        <button
          className={selectedMedia === 'demoRecording' ? 'active' : ''}
          onClick={() => setSelectedMedia('demoRecording')}
        >
          Demo Recording Radio Button
        </button>
      </div>

      {selectedMedia === 'demoRecording' && (
        <div className="content">
          <h3>Upload Audio Recording</h3>
          <input type="file" accept="audio/*" onChange={handleFileUpload} />
          
          {/* Preview uploaded audio */}
          {audioFile && (
            <div className='audio-trimmer'>
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

              <button style={{background:"#af8cf6", color:"white", padding:"4px"}} onClick={handleTrim}>Trim Audio</button>
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

      {selectedMedia === 'demoScript' && (
        <div className="content">
          <h3>Select Voice</h3>
          <div className="dropdown-group">
            <label>
              Customer:
              <select>
                <option>Salli (Female)</option>
                <option>Matthew (Male)</option>
                <option>Kimberly (Female)</option>
                <option>Kendra (Female)</option>
                <option>Justin (male)</option>
                <option>Joey(male)</option>
                <option>Lvy(Female)</option>
              </select>
            </label>
            <label>
              Agent:
              <select>
                <option>Salli (Female)</option>
                <option>Matthew (Male)</option>
                <option>Kimberly (Female)</option>
                <option>Kendra (Female)</option>
                <option>Justin (male)</option>
                <option>Joey(male)</option>
                <option>Lvy(Female)</option>
              </select>
            </label>
          </div>
          <h3>Upload Script File</h3>
          <div className="script-upload">
          <a href ='../../../public/sample/sample.pdf' download='../../../public/sample/sample.pdf'> Sample script</a>
            <textarea placeholder='Write your text area'></textarea>
            <input style={{paddingBottom:"6px", paddingTop:"6px"}} type="file" accept=".txt" />
          </div>
          <button className="create-resource-btn">Create Resource</button>
        </div>
      )}
    </div>
  );
};

export default MediaSelection;


.media-selection-container {
    font-family: Arial, sans-serif;
    padding: 20px;
    max-width: 600px;
    margin: auto;
  }
   
  /* Button group styling */
  .button-group {
    display: flex;
    gap: 10px;
    align-items: center;
    margin-bottom: 20px;
  }
  
  .button-group h2{
    font-weight: 60px;
    font-size: 15px;
  }

  .button-group button {
    padding: 10px 20px;
    border: 1px solid #333;
    background-color: #f4f4f4;
    cursor: pointer;
    transition: background-color 0.3s;
  }
   
  .button-group button.active {
    background-color: #af8cf6; 
    color: white;
    border: none;
  }
   
  .button-group button:hover {
    background-color: #af8cf6; 
    color: white;
  }
   
  /* Content styling */
  .content {
    border: 1px solid #ddd;
    padding: 20px;
    margin-bottom: 20px;
    background-color: #fafafa;
  }

  .content h3{
    font-weight: 30px;
    font-size: 15px;
    padding-bottom: 5px;
    padding-top: 5px;
  }

  .dropdown-group {
    display: flex;
    justify-content: space-between;
    margin-bottom: 20px;
  }
   
  .dropdown-group label {
    flex-basis: 48%;
  }

  .dropdown-group select{
    padding: 5px;
    margin: 3px;
  }
   
  .audio-trimmer h3{
    padding-bottom: 4px;
    padding-top: 4px;
    font-size: 15px;
  }

  .script-upload textarea{
    font-size: 15px;
    padding-top: 4px;
  }
  /* .audio-button {
    width: 30px;
    height: 10px;
    border: 1px solid #333;
    background-color: #af8cf6;
    cursor: pointer;
    transition: background-color 0.3s;
  } */

  /* .label select {
    width: 100%;
    padding: 5px;
    margin-top: 5px;
  } */
   
  /* Script Text styling */
  .script-text {
    margin-bottom: 20px;
    line-height: 1.5;
  }
   
  .script-upload h4{
    padding: 4px;
    font-size: 15px;
    font-weight: 60px;
  }
  /* Audio Preview styling */
  .audio-preview {
    margin-bottom: 10px;
    background-color: #eee;
    padding: 10px;
  }
   
  .media-trimmer {
    margin-bottom: 20px;
    background-color: #eee;
    padding: 10px;
  }
   
  /* Buttons styling */
  .create-resource-btn, .upload-media-btn {
    padding: 10px 20px;
    background-color: #28a745;
    color: white;
    border: none;
    cursor: pointer;
    transition: background-color 0.3s;
  }
   
  .create-resource-btn:hover, .upload-media-btn:hover {
    background-color: #218838;
  }
