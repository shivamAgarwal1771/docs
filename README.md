function convertBlobToFile(blob, fileName) {
  // Create a new File object with the desired properties
  const lastModified = Date.now();  // Use current timestamp or set a custom one
  const lastModifiedDate = new Date(lastModified);
  
  // Create the File object
  const file = new File([blob], fileName, {
    type: blob.type,            // audio/mpeg or other MIME type
    lastModified: lastModified,  // timestamp of last modification
  });

  // Optional: Set additional file properties manually if needed
  file.lastModifiedDate = lastModifiedDate;

  console.log('Created file:', file);
  return file;
}

function fetchAudioAndConvertToFile(audioTrimmedUrl, fileName = 'audio.mp3') {
  // Fetch the Blob from the Blob URL
  fetch(audioTrimmedUrl)
    .then(response => response.blob())  // Convert to Blob
    .then(blob => {
      // Convert Blob to File with the desired properties
      const file = convertBlobToFile(blob, fileName);
      
      // Now `file` is a File object with properties like `name`, `size`, `type`, `lastModified`
      // You can use this file in a File input or upload it to a server
    })
    .catch(error => {
      console.error('Error fetching or converting audio:', error);
    });
}

// Call the function with the Blob URL
fetchAudioAndConvertToFile(audioTrimmedUrl);  // Pass your Blob URL here
