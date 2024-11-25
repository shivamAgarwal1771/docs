fetch(audioTrimmedUrl)
  .then(response => response.blob())  // Convert the response into a Blob
  .then(blob => {
    // Now, you have the Blob, which represents the audio file
    // You can convert the Blob to other formats, such as ArrayBuffer, or play it directly
    
    console.log('Fetched audio Blob:', blob);

    // Optionally: Convert the Blob to ArrayBuffer for further processing
    const reader = new FileReader();
    reader.onloadend = function () {
      const arrayBuffer = reader.result;
      console.log('ArrayBuffer containing audio data:', arrayBuffer);
      // You can now use the arrayBuffer for playback or upload, etc.
      
      // Example: Playing the audio using an audio element
      const audioBlobUrl = URL.createObjectURL(new Blob([arrayBuffer], { type: 'audio/mp3' }));
      const audioElement = new Audio(audioBlobUrl);
      audioElement.play();
    };
    reader.readAsArrayBuffer(blob);  // Read Blob as an ArrayBuffer
  })
  .catch(error => {
    console.error('Error fetching audio data:', error);
  });
