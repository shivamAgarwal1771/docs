import React, { useState, useEffect } from 'react';
import { useSelector } from 'react-redux';
import { MdDownloadForOffline } from "react-icons/md";
import { TRANSCRIPTION_AUDIO_FILE } from '../../../utility/constants';

const Download = ({ setMessage }) => {
    const [conversationData, setConversationData] = useState(null);
    const conversation = useSelector(state => state?.speechstate?.conversation);

    // Function to download transcript as a file
    const downloadTranscript = () => {
        if (conversation) {
            const conversationURL = URL.createObjectURL(new Blob([JSON.stringify(conversation)], { type: 'application/json' }));

            const a = document.createElement('a');
            a.href = conversationURL;

            a.download = "transcription.json";
            a.style.display = "none";
            document.body.appendChild(a);

            a.click();

            document.body.removeChild(a);
            URL.revokeObjectURL(conversationURL);
        }
    };

    // Function to handle the download logic and fetch data from conversationURL
    function onDownload() {
        if (!conversation) {
            setMessage(TRANSCRIPTION_AUDIO_FILE.IN_PROGRESS);
            setTimeout(() => {
                setMessage(undefined);
            }, 5000);
        } else {
            downloadTranscript();
            setMessage(TRANSCRIPTION_AUDIO_FILE.DOWNLOADED);
            setTimeout(() => {
                setMessage(undefined);
            }, 5000);
        }
    }

    // Fetch data from conversationURL and set it to the state
    useEffect(() => {
        if (conversation) {
            const conversationURL = URL.createObjectURL(new Blob([JSON.stringify(conversation)], { type: 'application/json' }));
            fetch(conversationURL)
                .then(response => response.json()) // Assuming JSON format
                .then(data => {
                    setConversationData(data); // Set the data to the state
                    console.log("Fetched conversation data:", data); // Log the data to the console
                })
                .catch(error => {
                    console.error("Error fetching conversation data:", error);
                });

            // Cleanup the URL after the component unmounts or conversation changes
            return () => {
                URL.revokeObjectURL(conversationURL);
            };
        }
    }, [conversation]); // Run effect when 'conversation' changes

    return (
        <div className="audio-container">
            <div>
                <button className="audio-button download" onClick={onDownload}>
                    <MdDownloadForOffline className="icon-btn" />
                </button>
            </div>

            {/* Optionally display the fetched conversation data */}
            {conversationData && (
                <div>
                    <h4>Fetched Conversation Data:</h4>
                    <pre>{JSON.stringify(conversationData, null, 2)}</pre>
                </div>
            )}
        </div>
    );
};

export default Download;
