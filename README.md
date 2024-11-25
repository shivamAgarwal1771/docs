const extractAudioFromVideo = async () => {
    if (!videoFile) return;

    setIsProcessing(true);

    try {
      // Create a new Blob URL for the video file
      const videoBlobUrl = URL.createObjectURL(videoFile);

      // Create an HTML5 AudioContext to process the audio
      const audioContext = new (window.AudioContext || window.webkitAudioContext)();
      const videoElement = new Audio(videoBlobUrl);

      videoElement.onloadedmetadata = async () => {
        // Decode audio from the video file
        const response = await fetch(videoBlobUrl);
        const arrayBuffer = await response.arrayBuffer();
        const decodedAudioData = await audioContext.decodeAudioData(arrayBuffer);

        // Now we have decoded audio data, you can extract it or create a new audio file
        const audioBlob = await audioBufferToBlob(decodedAudioData);
        const file = new File([audioBlob], 'audio.mp3', { type: 'audio/mpeg' });

        // Set the audio file in the state
        setAudioFile(file);

        // Dispatch the audio file to Redux
        dispatch(setAudioFile(file));

        setIsProcessing(false);
      };
    } catch (error) {
      console.error('Error extracting audio:', error);
      setIsProcessing(false);
    }
  };
