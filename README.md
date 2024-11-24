async function handleSubmit() {
    if (isValid && audio && transcript && metadata) {
        console.log(audio, metadata, transcript, "test11")
        setLoader(true);
        dispatch(setUnsavedChanges(false));

        let audioFormat, audioType;
        if (audio) {
            audioFormat = audio.type ? audio.type : 'audio/mpeg';
            audioType = audioFormat.split('/')[1];
        }

        let check_audio = checkAudioFormat(audioFormat.split('/')[0], dispatch);
        let jsonFormat = transcript.type ? transcript.type : 'application/json';
        let check_json = checkJsonFormat(jsonFormat.split('/')[1], dispatch);

        let manipulateMetadata = manipulateCallSummary(metadata);
        let manipulatepersonalInformationData = manipulateCustomerDetails(manipulateMetadata);

        try {
            if (!isEditDemo) {
                let codeAvailibility = await checkCodeAvailability(manipulatepersonalInformationData.code);
                if (!codeAvailibility) {
                    toast.error(UPLOAD_DEMO_MSG.CODE_FAILURE_MSG);
                    return null;
                }

                toast.success(UPLOAD_DEMO_MSG.CODE_SUCCESS_MSG);
            }

            if (check_audio && check_json) {
                const audioBuffer = !isEditDemo ? await audio.arrayBuffer() : audio;

                // Check if transcript is a Blob (for text extraction) or a JSON object
                let transcriptString = '';
                if (isEditDemo) {
                    // If transcript is already a JSON object (as a result of editing), stringify it
                    transcriptString = JSON.stringify(transcript);
                } else if (transcript instanceof Blob) {
                    // If transcript is a Blob or File, use .text() method
                    transcriptString = await transcript.text();
                } else {
                    // Fallback: assume it's already a string or parsed JSON
                    transcriptString = JSON.stringify(transcript);
                }

                let credentials = await getCredentials();
                await uploadDemoToS3(audioBuffer, audioType, transcriptString, manipulatepersonalInformationData, dispatch, credentials, metadata?.code, demoState);
            }
        } catch (err) {
            console.error("Error Uploading data", err);
        }
    } else {
        const missingFields = requiredFields.filter(field => !metadata[field] || metadata[field].trim() === '');
        if (missingFields.length > 0) {
            toast.error(`Missing fields: ${missingFields.join(', ')}`);
            return false;
        }
    }
}
