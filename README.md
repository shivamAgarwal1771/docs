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
