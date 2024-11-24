uploadDemo.jsx:144 
 Error Uploading data TypeError: transcript.text is not a function
    at eval (uploadDemo.jsx:138:77)
eval	@	uploadDemo.jsx:144

getting this error when handlesumbit function fun in below code please reolve

import React, { useEffect, useMemo, useState } from 'react';
import { useSelector, useDispatch } from 'react-redux';
import { setIsEditDemo, setDemoCode, setSuccessMsg, undoSuccessCount, setDemoData, setDemoTranscriptSubmitted, updateDemoData, setIsCreateDemo, setUnsavedChanges } from '../../../store/demo-slice'
import { manipulateCallSummary, manipulateCustomerDetails } from '../../../utility/upload-demo/manipulate';
import UploadBody from './UploadBody';
import getCredentials from '../../../utility/authentication/get-credentials';
import uploadDemoToS3 from '../../../utility/upload-demo/uploadToS3';
import { checkAudioFormat, checkCodeAvailability, checkJsonFormat } from '../../../utility/upload-demo/check';
import { useCallback } from 'react';
import toast, { Toaster } from 'react-hot-toast';
import { PLATFORM_NAME, UPLOAD_DEMO_MSG } from "../../../utility/constants";
import { getFolderFromS3 } from '../../../utility/upload-demo/getfolderfroms3';
import Loading from "../../../components/common/Loading";
import { setScreen } from '../../../store/userSlice';
import { formHelperTextClasses } from '@mui/material';
import ProgressBar from '../../../pages/progressLeader/progressBar';

export default function UploadDemo({ audio, setAudio }) {
    const dispatch = useDispatch();
    const demoState = useSelector((state) => state?.demostate);
    const appCallData = useSelector((state)=>state?.call)
    const transcriptData = appCallData.progressBarTranscript
    const fileCode = demoState?.getDemoCode;
    const demoData = demoState?.getDemoData;
    const isCreateDemo = demoState?.createDemo;
    const isEditDemo = demoState?.editDemo;
    let errorMsg = useSelector(state => state?.demostate?.errorMsg)
    let successMsg = useSelector(state => state?.demostate?.successMsg)
    let successCount = useSelector(state => state?.demostate?.successCount)
    const [isValid, setIsValid] = useState(false);
    const [loader, setLoader] = useState(false);
    const [finalTranscript,setFinalTranscript] =useState(null)
    const [transcript, setTranscript] = useState(null);
    const [metadata, setMetadata] = useState(demoData?.metadata);

    function changeObjType(requiredData) {
        let newArray = [];
        Object.keys(requiredData).map((item) => {
            newArray.push({ key: item, value: requiredData[item] });
        });
        requiredData = newArray;
        return requiredData;
    }

    useEffect(() => {
        if (isEditDemo && !(Object.keys(demoData).length)) {
            const getData = async () => {
                try {
                    const credentials = await getCredentials();
                    const fetchedData = await getFolderFromS3(credentials, fileCode);
                    fetchedData.metadata.summary.callSummary = changeObjType(fetchedData?.metadata?.summary?.callSummary);
                    fetchedData.metadata.personalInformation = changeObjType(fetchedData?.metadata?.personalInformation);
                    fetchedData?.transcript?.forEach((obj) => {
                        if (obj.eventData.Context && !Array.isArray(obj.eventData.Context)) {
                            obj.eventData.Context = Array(obj?.eventData?.Context)
                        }
                    });
                    if (fetchedData?.audio) {
                        setAudio(fetchedData?.audio);
                        delete fetchedData?.audio
                    }
                    dispatch(setDemoData(fetchedData));
                } catch (err) {
                    console.error("Error fetching data from s3", err);
                }
            };
            getData().then(() => setLoader(false));
        }
    }, []);

    useEffect(() => {
        isEditDemo && !(Object.keys(demoData).length) && setLoader(true);
        if (demoData && isEditDemo) {
            setTranscript(demoData?.transcript);
            console.log(demoData?.metadata,"test2")
            setMetadata(demoData?.metadata);
        }
    }, [demoData]);

    useEffect(() => {
        dispatch(undoSuccessCount())
    }, [])

    const requiredFields = useMemo(() => ['agent', 'selectedLanguage', 'useCase', 'industry', 'channel', 'interactionDate', 'aht', 'code'], [])

    const validateMetadata = useCallback((metadata) => {
        return requiredFields.every(field => metadata[field] && metadata[field].trim() !== '')
    }, []);

    useEffect(() => {
        metadata && setIsValid(validateMetadata(metadata))
        metadata && dispatch(updateDemoData({ type: "metadata", data: metadata }))
        isEditDemo && dispatch(setUnsavedChanges(true));
    }, [metadata, requiredFields])


    useEffect(()=>{
        
        setTranscript(finalTranscript)
    },[finalTranscript])

    function handleInput(field, value) {
        console.log(field,value,"test99")
        let newMetadata = { ...metadata }
        newMetadata = { ...newMetadata, [field]: value }
        console.log(newMetadata,"test77")
        setMetadata(newMetadata)
    }

    async function handleSubmit() {
        if (isValid && audio && transcript && metadata) {
            console.log(audio,metadata,transcript,"test11")
            setLoader(true);
            dispatch(setUnsavedChanges(false));
            let audioFormat, audioType;
            if (audio) {
                audioFormat = audio.type ? audio.type : 'audio/mpeg';
                audioType = audioFormat.split('/')[1]
            }

            let check_audio = checkAudioFormat(audioFormat.split('/')[0], dispatch)
            let jsonFormat = transcript.type ? transcript.type : 'application/json'
            let check_json = checkJsonFormat(jsonFormat.split('/')[1], dispatch)

            let manipulateMetadata = manipulateCallSummary(metadata)
            let manipulatepersonalInformationData = manipulateCustomerDetails(manipulateMetadata)
            try {
                if (!isEditDemo) {
                    let codeAvailibility = await checkCodeAvailability(manipulatepersonalInformationData.code)
                    if (!codeAvailibility) {
                        toast.error(UPLOAD_DEMO_MSG.CODE_FAILURE_MSG)
                        return null;
                    }

                    toast.success(UPLOAD_DEMO_MSG.CODE_SUCCESS_MSG)
                }
                if (check_audio && check_json) {
                    const audioBuffer = !isEditDemo ? await audio.arrayBuffer() : audio;
                    const transcriptString = !isEditDemo ? await transcript.text() : JSON.stringify(transcript);

                    let credentials = await getCredentials()
                    await uploadDemoToS3(audioBuffer, audioType, transcriptString, manipulatepersonalInformationData, dispatch, credentials, metadata?.code,demoState)
                }
            } catch(err) {
                console.error("Error Uploading data",err);
            }
        } else {
            const missingFields = requiredFields.filter(field => !metadata[field] || metadata[field].trim() === '')
  
            if (missingFields.length > 0) {
                toast.error(`Missing fields: ${missingFields.join(', ')}`)
            return false;
            }
        }
    }

    useEffect(() => {
        if (errorMsg !== '') {
            dispatch(undoSuccessCount())
            toast.error(errorMsg)
        }
    }, [errorMsg])

    useEffect(() => {
        if (successCount === 3) {
            dispatch(undoSuccessCount())
            dispatch(setSuccessMsg('Successfully uploaded'))
            setTimeout(() => {
                dispatch(setSuccessMsg(''))
            }, 5000)
        }
    }, [successCount])

    useEffect(() => {
        if (successMsg) {
            toast.success(successMsg)
            setTimeout(() => {
                dispatch(setIsCreateDemo(false));
                dispatch(setScreen(PLATFORM_NAME.SMART_AGENT));
                dispatch(setIsEditDemo(false));
                dispatch(setDemoCode(null));
                dispatch(setDemoTranscriptSubmitted(false));
                dispatch(setDemoData({}))
                setLoader(false)
            }, 1000)
        }
    }, [successMsg])

    function handleStep(value){
        if(value==8)
        {
            handleSubmit()
        }
    }


    return (
        <div className="upload-demo-container">
            {/* <ProgressBar handleInput={handleInput}/> */}
            <UploadBody finalTranscript={finalTranscript} setFinalTranscript= {setFinalTranscript} handleStep={handleStep} audio={audio} transcript={transcript} setAudio={setAudio} setTranscript={setTranscript} metadata={metadata ? JSON.parse(JSON.stringify(metadata)) : {}} handleInput={handleInput} handleSubmit={handleSubmit} />
            {loader && <Loading isLoading={loader} />}
            <Toaster
                position="top-right"
                reverseOrder={false}
            />
        </div>
    )
}
