{"Title":"","subTitle":"","message":""} ={"Title":"","subTitle":"","message":"g"}

above is the console of this     newTranscript[index].eventData.Transcript.Transcript = JSON.stringify(newRecommendation)

getting this error
TypeError: Cannot assign to read only property 'Transcript' of object '#<Object>'

Source
components\demoCreationComponents\customise\Body.jsx (88:56) @ handleEditRecomendation

  86 |   newRecommendation[field] = newValue
  87 |   console.log(newTranscript[index].eventData.Transcript.Transcript,JSON.stringify(newRecommendation),"test90")
> 88 |   newTranscript[index].eventData.Transcript.Transcript = JSON.stringify(newRecommendation)
     |                                                      ^
  89 |   setTranscript(newTranscript)
  90 | }
