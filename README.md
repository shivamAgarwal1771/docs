import { useEffect, useState } from 'react'
import { useSelector, useDispatch } from 'react-redux'
import { removeStatus } from '../../../store/speech-slice'

import { uploadToS3 } from '../../../utility/text2speech/uploadAudioToS3'

import Loader from '../text2json/Loader';
import TranscriptionStatus from '../text2json/Status';
import VideoTrimmer from './VideoTrimmer'
import { AUDIO_FILE_SPEAKER } from '../../../utility/constants';
import { setProgressBarTranscript } from "../../../store/callSlice"
import {setProgressBarAudio} from "../../../store/callSlice"


export default function VideotoJson() {
    const [audioBlob, setAudioBlob] = useState(null)
    const [videoBlob, setVideoBlob] = useState(null)
    const [videoTrimmedUrl, setVideoTrimmedUrl] = useState(undefined)
    const [audioTrimmedUrl, setAudioTrimmedUrl] = useState(undefined)
    const [firstSpeaker, setFirstSpeaker] = useState(AUDIO_FILE_SPEAKER.FIRST_SPEAKER)
    const [submitted, setSubmitted] = useState(false)
    const [message, setMessage] = useState(undefined)
    const [allDone, setAllDone] = useState(false)

    const dispatch = useDispatch()
    const status = useSelector(state => state?.speechstate?.status)
    const conversation = useSelector(state => state?.speechstate?.conversation)

    useEffect(() => {
        dispatch(removeStatus())
    }, [])

    useEffect(() => {
        const videoUrl = videoBlob ? URL.createObjectURL(videoBlob) : undefined
        const audioUrl = audioBlob ? URL.createObjectURL(audioBlob) : undefined

        setVideoTrimmedUrl(videoUrl)
        setAudioTrimmedUrl(audioUrl)
     

    }, [audioBlob, videoBlob])

    // Effect to update progress when transcript is ready
    useEffect(() => {
        if (status && status.includes('Transcript is ready for download!!')) {
            const conversationURL = URL.createObjectURL(new Blob([JSON.stringify(conversation)], { type: 'application/json' }));
            fetch(conversationURL)
                .then(response => response.json()) 
                .then(data => {
                    setConversationData(data); 
                    dispatch(setProgressBarTranscript(data))
                    console.log("Fetched conversation data test66 :", data); 
                })
                .catch(error => {
                    console.error("Error fetching conversation data:", error);
                });

            return () => {
                URL.revokeObjectURL(conversationURL);
            };
        }
    }, [status, conversation]);

    async function handleSubmit() {
        if (audioBlob) {
            setSubmitted(true)
            try {
                let buffer = await audioBlob.arrayBuffer()
                uploadToS3(buffer, dispatch, firstSpeaker)
                const audioUpload = await uploadToS3(buffer, dispatch, firstSpeaker)
                console.log(audioUpload,"testrx")
            } catch (err) {
                console.error(err)
            }
        }
    }

    function onDownload() {
        if (videoTrimmedUrl && audioTrimmedUrl) {
            const videoDownloadLink = document.createElement('a')
            videoDownloadLink.href = videoTrimmedUrl
            videoDownloadLink.download = 'video.mp4'
            videoDownloadLink.click()

            const audioDownloadLink = document.createElement('a')
            audioDownloadLink.href = audioTrimmedUrl
            audioDownloadLink.download = 'audio.mp3'
            audioDownloadLink.click()
            if (conversation) {
                const conversationURL = URL.createObjectURL(new Blob([JSON.stringify(conversation)], { type: 'application/json' }))
                
                const a = document.createElement('a')
                a.href = conversationURL
                a.download = "transcription.json"
                a.style.display = "none"
                document.body.appendChild(a)
              
                a.click()

                document.body.removeChild(a)
                URL.revokeObjectURL(conversationURL)
                setMessage("Downloaded all - Trimmed Video, its Audio and the Transcript.")
                setAllDone(true)
            } else {
                setMessage("Downloaded only the trimmed video and its audio.<br>Transcript is still being generated.")
                setAllDone(false)
                setTimeout(() => {
                    setMessage(undefined)
                }, 5000)
            }
        }
    }


    return (
        <div className="video-json-container">
            <div className='flex-row'>
                {!submitted &&
                    <VideoTrimmer
                        setAudioBlob={setAudioBlob}
                        setVideoBlob={setVideoBlob}
                        handleSubmit={handleSubmit}
                        firstSpeaker={firstSpeaker}
                        setFirstSpeaker={setFirstSpeaker}
                    />}
                {submitted &&
                    <div className='flex-row space-around status-container'>
                        <div className='main-body-child'>
                            {status.length !== 0 ?
                                <TranscriptionStatus message={message} allDone={allDone} /> :
                                <Loader />}
                        </div>
                        <button
                            onClick={onDownload}
                            className="video-btn green-btn"
                        >
                            Download
                        </button>
                    </div>}
            </div>
        </div>
    )
}
