const handleAddSpeechSuggestion = (index, value) => {
  // Clone the transcript array immutably
  const newTranscript = [...transcript];

  // Create the new guidance object
  const guidance = {
    type: AI_ASSISTANT_TYPE.SPEECH_SUGGESTION,
    name: 'Speech Suggestion',
    value: value,
  };

  // Clone the eventData and Guidance properties immutably
  const updatedEventData = {
    ...newTranscript[index].eventData,
    Guidance: newTranscript[index].eventData.Guidance ? 
      [...newTranscript[index].eventData.Guidance, guidance] : 
      [guidance],
  };

  // Replace the eventData at the specified index with the updated one
  newTranscript[index] = {
    ...newTranscript[index],
    eventData: updatedEventData,
  };

  // Update the state immutably
  setTranscript(newTranscript);
};



const handleAddKnowledgeArticle = (index) => {
  // Clone the transcript array immutably
  const newTranscript = [...transcript];

  // Create the new guidance object
  const guidance = {
    type: AI_ASSISTANT_TYPE.KNOWLEDGE_ARTICLE,
    name: '',
    value: [],
  };

  // Clone the eventData and Guidance properties immutably
  const updatedEventData = {
    ...newTranscript[index].eventData,
    Guidance: newTranscript[index].eventData.Guidance ? 
      [...newTranscript[index].eventData.Guidance, guidance] : 
      [guidance],
  };

  // Replace the eventData at the specified index with the updated one
  newTranscript[index] = {
    ...newTranscript[index],
    eventData: updatedEventData,
  };

  // Update the state immutably
  setTranscript(newTranscript);
};
