const handleEditRecomendation = (index, field, newValue) => {
  // Clone the transcript array immutably
  const newTranscript = [...transcript];

  // Clone the eventData and the Transcript properties immutably
  const eventDataClone = { 
    ...newTranscript[index].eventData,
    Transcript: {
      ...newTranscript[index].eventData.Transcript,
      // Ensure the Transcript itself is not frozen by cloning it
      Transcript: {
        ...JSON.parse(newTranscript[index].eventData.Transcript.Transcript),
        [field]: newValue, // Update the field
      }
    }
  };

  // Update the new transcript at the specified index
  newTranscript[index] = {
    ...newTranscript[index],
    eventData: eventDataClone,  // Use the updated eventData
  };

  // Update the state immutably
  setTranscript(newTranscript);
};
