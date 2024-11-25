import React from 'react'
import { useSelector } from 'react-redux';
import { MdDownloadForOffline } from "react-icons/md";
import { TRANSCRIPTION_AUDIO_FILE } from '../../../utility/constants';

const Download = ({ setMessage }) => {
    const conversation = useSelector(state => state?.speechstate?.conversation)

    const downloadTranscript = () => {
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
        }
    }

    function onDownload() {
         if (!conversation) {
            setMessage(TRANSCRIPTION_AUDIO_FILE.IN_PROGRESS)
            setTimeout(()=>{
                setMessage(undefined)
            },5000)
        } else {
            downloadTranscript();
            setMessage(TRANSCRIPTION_AUDIO_FILE.DOWNLOADED)
            setTimeout(()=>{
                setMessage(undefined)
            },5000)
        }
    }


    return (
        <div className="audio-container">
            <div>
                <button className="audio-button download" onClick={() => { onDownload() }}>
                    <MdDownloadForOffline className="icon-btn" />
                </button>
            </div>
        </div>
    )
}


export default Download;
