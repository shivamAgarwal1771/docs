const handleAddKnowledgeArticle = (index) => {
  // Make a deep copy of the transcript to avoid mutation
  const newTranscript = JSON.parse(JSON.stringify(transcript));

  // Define the new knowledge article guidance
  let guidance = {
    "type": AI_ASSISTANT_TYPE.KNOWLEDGE_ARTICLE,
    "name": '',
    "value": []
  };

  // Ensure we're working with a new copy of the Guidance array, not the original
  let Gui = [...(newTranscript[index].eventData.Guidance || [])];
  
  // Add the new knowledge article to the guidance
  Gui.push(guidance);
  
  // Update the transcript with the new guidance array
  newTranscript[index].eventData.Guidance = Gui;

  // Update the state
  setTranscript(newTranscript);
};
