const handleAddContext = (index, newContext) => {
    const newTranscript = [...transcript]; // Clone the transcript array

    // Clone the eventData object before modifying it
    const updatedEventData = { 
        ...newTranscript[index].eventData, 
        Context: newTranscript[index].eventData?.Context ? 
            [...newTranscript[index].eventData.Context, newContext] : 
            [newContext] 
    };

    // Replace the eventData with the updated version
    newTranscript[index] = {
        ...newTranscript[index],
        eventData: updatedEventData
    };

    setTranscript(newTranscript);
}
