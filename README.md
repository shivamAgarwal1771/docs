function convertBlobToBase64(blob) {
  return new Promise((resolve, reject) => {
    const reader = new FileReader();
    reader.onloadend = function () {
      const base64String = reader.result.split(',')[1];  // Remove the "data:audio/mp3;base64," part
      resolve(base64String);
    };
    reader.onerror = function (error) {
      reject(error);
    };
    reader.readAsDataURL(blob);  // Read as a data URL (base64 encoded)
  });
}

// Function to fetch the Blob from the Blob URL and convert to Base64
function fetchAudioAsBase64(audioTrimmedUrl) {
  fetch(audioTrimmedUrl)
    .then(response => response.blob())  // Get the Blob
    .then(blob => {
      return convertBlobToBase64(blob);  // Convert the Blob to Base64
    })
    .then(base64Audio => {
      console.log("Base64 Audio:", base64Audio);
      // Now you can use the base64 string (e.g., play the audio, upload it, etc.)
    })
    .catch(error => {
      console.error('Error fetching or converting audio:', error);
    });
}

// Call the function with the Blob URL
fetchAudioAsBase64(audioTrimmedUrl);  // Use the Blob URL here
