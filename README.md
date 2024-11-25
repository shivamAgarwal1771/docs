// Function to convert video to audio
const convertVideoToAudio = (videoFile, setAudioUrl) => {
  // Create an object URL for the video file to load into a video element
  const videoUrl = URL.createObjectURL(videoFile);

  // Create a video element to load the video file
  const videoElement = document.createElement('video');
  videoElement.src = videoUrl;

  // Wait until the video metadata is loaded (needed for capturing the stream)
  videoElement.onloadedmetadata = () => {
    // Capture the video stream, including audio tracks
    const stream = videoElement.captureStream();
    const audioTracks = stream.getAudioTracks();

    if (audioTracks.length > 0) {
      // Create a new MediaStream with only the audio tracks
      const audioStream = new MediaStream([audioTracks[0]]);
      const audioContext = new (window.AudioContext || window.webkitAudioContext)();
      const mediaSource = audioContext.createMediaStreamSource(audioStream);

      // Create a destination for the audio stream
      const audioDestination = audioContext.createMediaStreamDestination();
      mediaSource.connect(audioDestination);

      // Create a MediaRecorder to record the audio
      const recorder = new MediaRecorder(audioDestination.stream);
      const chunks = [];

      // Push audio data when available
      recorder.ondataavailable = (event) => {
        chunks.push(event.data);
      };

      // When recording stops, create a Blob and a URL for the audio file
      recorder.onstop = () => {
        const audioBlob = new Blob(chunks, { type: 'audio/mp3' }); // or 'audio/wav'
        const audioUrl = URL.createObjectURL(audioBlob);
        setAudioUrl(audioUrl); // Set the audio URL to state (for downloading)
      };

      // Start recording the audio
      recorder.start();

      // Stop recording when the video ends
      videoElement.onended = () => {
        recorder.stop();
      };

      // Play the video so that the audio can be captured
      videoElement.play();
    } else {
      alert('No audio tracks found in the video.');
    }
  };

  // Handle video error (e.g., unsupported file format)
  videoElement.onerror = (err) => {
    console.error('Error loading video:', err);
    alert('There was an error processing the video.');
  };
};
