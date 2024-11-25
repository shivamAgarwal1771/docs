  const handleAddContext = (index, newContext) => {
    const newTranscript = [...transcript]
    let context = newTranscript[index].eventData?.Context ?
      newTranscript[index].eventData.Context :
      []
    context.push(newContext)
    newTranscript[index].eventData.Context = context
    setTranscript(newTranscript)
  }


  TypeError: Cannot add property Context, object is not extensible

Source
components\demoCreationComponents\customise\Body.jsx (182:42) @ handleAddContext

  180 |     []
  181 |   context.push(newContext)
> 182 |   newTranscript[index].eventData.Context = context
      |                                        ^
  183 |   setTranscript(newTranscript)
  184 | }
