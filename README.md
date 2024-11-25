const handleEditRecomendation = (index, field, newValue) => {
  // Clone the transcript array immutably
  const newTranscript = [...transcript];

  // Clone the eventData and Transcript objects immutably
  const eventDataClone = { 
    ...newTranscript[index].eventData,
    Transcript: {
      ...newTranscript[index].eventData.Transcript,
      // Deep clone the Transcript content and update the field
      Transcript: {
        ...JSON.parse(newTranscript[index].eventData.Transcript.Transcript),  // Parse and clone the Transcript
        [field]: newValue,  // Update the specific field with newValue
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
