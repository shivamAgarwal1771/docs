const handleEditRecommendation = (index, field, newValue) => {
  // Clone the transcript array immutably
  const newTranscript = [...transcript];

  // Create the new Transcript object
  const updatedTranscript = {
    Title: "",
    subTitle: "",
    message: newValue // Set the new value for message
  };

  // Clone the eventData and update the Transcript immutably
  const updatedEventData = {
    ...newTranscript[index].eventData,
    Transcript: updatedTranscript // Replace the Transcript with the updated one
  };

  // Update the transcript at the specified index with the new eventData
  newTranscript[index] = {
    ...newTranscript[index],
    eventData: updatedEventData
  };

  // Update the state immutably
  setTranscript(newTranscript);
};
