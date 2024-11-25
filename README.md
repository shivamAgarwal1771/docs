  const handleAddSpeechSuggestion = (index, value) => {
    const newTranscript = [...transcript]
    let guidance = {
      "type": AI_ASSISTANT_TYPE.SPEECH_SUGGESTION,
      "name": "Speech Suggestion",
      "value": value
    }
    let Gui = newTranscript[index].eventData.Guidance ? newTranscript[index].eventData.Guidance : []
    Gui.push(guidance)
    newTranscript[index].eventData.Guidance = Gui
    setTranscript(newTranscript)
  }

  const handleAddKnowledgeArticle = (index) => {
    const newTranscript = [...transcript]
    let guidance = {
      "type": AI_ASSISTANT_TYPE.KNOWLEDGE_ARTICLE,
      "name": '',
      "value": []
    }
    let Gui = newTranscript[index].eventData.Guidance ? newTranscript[index].eventData.Guidance : []
    Gui.push(guidance)
    newTranscript[index].eventData.Guidance = Gui
    setTranscript(newTranscript)
  }
