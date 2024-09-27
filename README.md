import React, { useEffect, useState } from "react";
import { generateClient } from "aws-amplify/api";
import gql from "graphql-tag";

const client = generateClient();

// Define your subscription query
const ON_CALL_CREATE = `
  subscription Subscription {
        onCreateCall {
            CallId
        }
    }
`;
const ADD_TRANSCRIPT_SEGMENT = `
  subscription Subscription($callId: ID) {
    onAddTranscriptSegment(CallId: $callId) {
      PK
      SK
      CreatedAt
      CallId
      SegmentId
      StartTime
      EndTime
      Transcript
      IsPartial
      Channel
      Sentiment
      SentimentScore {
        Positive
        Negative
        Neutral
        Mixed
      }
      SentimentWeighted
    }
  }
`;

const mapTranscriptSegmentValue = (transcriptSegmentValue) => {
  const {
    CallId: callId,
    SegmentId: segmentId,
    StartTime: startTime,
    EndTime: endTime,
    Transcript: transcript,
    IsPartial: isPartial,
    Channel: channel,
    CreatedAt: createdAt,
    Sentiment: sentiment,
    SentimentScore: sentimentScore,
    SentimentWeighted: sentimentWeighted,
  } = transcriptSegmentValue;

  return {
    callId,
    segmentId,
    startTime,
    endTime,
    transcript,
    isPartial,
    channel,
    createdAt,
    sentiment,
    sentimentScore,
    sentimentWeighted,
  };
};

const Calls = () => {
  const [callList, setCallList] = useState([]);
  const [transcripts, setTranscripts] = useState([]);

  const attachTranscriptionSubscriberForCallId = (callId) => {
    let subscription;
    subscription = client.graphql({ query: ADD_TRANSCRIPT_SEGMENT }).subscribe({
      next: ({ data }) => {
        const transcriptData = data.onAddTranscriptSegment;
        const transcriptSegment = mapTranscriptSegmentValue(transcriptData);
        const { transcript, channel } = transcriptSegment;
        setTranscripts((prev) => [...prev, { transcript, channel }]);
      },
      error: (error) => {
        console.error("Subscription error:", error);
      },
    });

    // Cleanup the subscription on component unmount
    return () => subscription.unsubscribe();
  };
  useEffect(() => {
    let subscription;
    subscription = client.graphql({ query: ON_CALL_CREATE }).subscribe({
      next: ({ data }) => {
        const newCall = data.onCreateCall;
        attachTranscriptionSubscriberForCallId(newCall);
        subscription.unsubscribe();
      },
      error: (error) => {
        console.error("Subscription error:", error);
      },
    });

    // Cleanup the subscription on component unmount
    return () => subscription.unsubscribe();
  }, []);
console.log(transcripts,"test4")
  return (
    <div>
      {transcripts.map((transcript, index) => (
        <div key={transcript}>
          <strong>{transcript.channel}:</strong> {transcript.transcript}
        </div>
      ))}
    </div>
  );
};

export default Calls;
