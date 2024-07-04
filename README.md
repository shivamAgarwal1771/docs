[
    {
        "StartTime": "0.209",
        "EndTime": "1.210",
        "eventData": {
            "Transcript": {
                "Transcript": "{\"Title\":\"\",\"subTitle\":\"\",\"message\":\"\"}",
                "ChannelId": "AGENT_ASSISTANT",
                "StartTime": "0.209",
                "EndTime": "1.210",
                "IsPartial": false,
                "amortizationSchedule":"show",
                "ResultId": "9cc34092-88d3-4847-b200-864da8e1e653",
                "Sentiment": "Neutral",
                "SentimentScore": {
                    "Positive": 0.1,
                    "Negative": 0.1,
                    "Neutral": 0.8,
                    "Mixed": 0
                }
            },
            "Audits": [],
            "Guidance": [
                {
                    "type": "SPEECH_SUGGESTION",
                    "name": "Speech Suggestion",
                    "value": "Thank you for calling Angle Auto. This is <Your Name>. To begin may I have your registered contact number, please?"
                }
            ]
        }
    },
    {
        "StartTime": "0.419",
        "EndTime": "5.8",
        "eventData": {
            "Transcript": {
                "Transcript": "Thank you for calling Angle Auto. This is Alex. To begin with may I have your registered contact number, please?",
                "ChannelId": "ch_1",
                "StartTime": "0.419",
                "EndTime": "5.8",
                "IsPartial": false,
                "ResultId": "a144af06-ebd9-4f37-9de8-9ebee65781bc",
                "Sentiment": "Neutral",
                "SentimentScore": {
                    "Positive": 0.1,
                    "Negative": 0.1,
                    "Neutral": 0.8,
                    "Mixed": 0
                }
            },
            "Audits": [
                {
                    "section": "Qualifying and Verifying the customer",
                    "auditStatus": "FAIL",
                    "questions": [
                        {
                            "question": "Customer Validated",
                            "auditResponse": "No",
                            "options": [
                                "Yes",
                                "No"
                            ]
                        }
                    ]
                },
                {
                    "section": "Call Closing",
                    "auditStatus": "FAIL",
                    "questions": [
                        {
                            "question": "Additional Help Offered",
                            "auditResponse": "No",
                            "options": [
                                "Yes",
                                "No"
                            ]
                        }
                    ]
                }
            ]
        }
    },
    {
        "StartTime": "6.349",
        "EndTime": "14.119",
        "eventData": {
            "Transcript": {
                "Transcript": "Hi, Alex. My contact number is 61422148875.",
                "ChannelId": "ch_0",
                "StartTime": "6.349",
                "EndTime": "14.119",
                "amortizationSchedule":"show",
                "IsPartial": false,
                "ResultId": "0847b73d-e468-4a12-b487-cbcfbccb3581",
                "Sentiment": "Neutral",
                "SentimentScore": {
                    "Positive": 0.1,
                    "Negative": 0.1,
                    "Neutral": 0.8,
                    "Mixed": 0
                }
            }
        }
    },
    ]
