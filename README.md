
  const handleEditRecomendation = (index, field, newValue) => {
    const newTranscript = [...transcript]
    const newRecommendation = JSON.parse(newTranscript[index].eventData.Transcript.Transcript)
    newRecommendation[field] = newValue
    newTranscript[index].eventData.Transcript.Transcript = JSON.stringify(newRecommendation)
    setTranscript(newTranscript)
  }

  TypeError: Cannot assign to read only property 'Transcript' of object '#<Object>'

Source
components\demoCreationComponents\customise\Body.jsx (87:56) @ handleEditRecomendation

  85 |   const newRecommendation = JSON.parse(newTranscript[index].eventData.Transcript.Transcript)
  86 |   newRecommendation[field] = newValue
> 87 |   newTranscript[index].eventData.Transcript.Transcript = JSON.stringify(newRecommendation)
     |                                                      ^
  88 |   setTranscript(newTranscript)
  89 | }
