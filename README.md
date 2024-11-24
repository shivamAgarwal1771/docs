const handleAddKnowledgeArticle = (index) => {
  const newTranscript = [...transcript];
  let guidance = {
    "type": AI_ASSISTANT_TYPE.KNOWLEDGE_ARTICLE,
    "name": '',
    "value": []
  };

  // Ensure we're creating a mutable copy of the Guidance array
  let Gui = [...(newTranscript[index].eventData.Guidance || [])];
  Gui.push(guidance); // Push the new guidance
  newTranscript[index].eventData.Guidance = Gui; // Assign back the new array

  setTranscript(newTranscript);
};
