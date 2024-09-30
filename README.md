// Constants defining various URIs and authentication details for AWS AppSync
export const host = "znh7evtkjjfevpy7mtp6ioc3tu.appsync-api.us-east-1.amazonaws.com";
export const apiKey = "da2-ksvj4ldfczbw7h5c3utmldgkny";
export const resourceWSURI = "wss://znh7evtkjjfevpy7mtp6ioc3tu.appsync-realtime-api.us-east-1.amazonaws.com/graphql";
export const resourceHttpURI = "https://znh7evtkjjfevpy7mtp6ioc3tu.appsync-api.us-east-1.amazonaws.com/graphql";
export const id = "ts-45786c2d536d6172742d4167656e74"//"transcript-90-4cb8-9fcb-152ae4fd1e90";
const id_updateCall = "uc-45786c2d536d6172742d4167656e74";
export const callIdPlaceholder = `#callid_placeholder#`;
export const textPlaceholder = `#text_placeholder#`;
const aliasId = `TSTALIASID`;
const botId = `VVCTYRGZUO`;
const sessionId = `220335042009125`;

// Authentication information encoded for requests
const authJSON = {
    "host":`${host}`,
    "x-api-key": `${apiKey}`
}

// Encode authentication information
export const authBase = () => {
    return btoa(JSON.stringify(authJSON))
}

// Encode payload information
export const payloadBase = () => {
    return btoa(JSON.stringify({}));
}

// Subscription query for receiving new transcript segments
const subOnAddTranscriptSegment = 
`subscription OnAddTranscriptSegment {
    onAddTranscriptSegment(CallId: "${callIdPlaceholder}") {
        __typename
        PK
        SK
        CreatedAt
        UpdatedAt
        ExpiresAfter
        CallId
        SegmentId
        StartTime
        EndTime
        Transcript
        IsPartial
        Channel
        Sentiment
        SentimentWeighted
        SentimentScore {
            Positive
            Negative
            Neutral
            Mixed
        }
    }
}`;

// Query object for adding transcript segments
const queryAddTranscriptSegment =
{
    "query":`${subOnAddTranscriptSegment}`,
    "variables":{}
};

// Subscription registration details
export const registerOnAddTranscript = {
    "id": `${id}`,
    "payload": {
        "data": `${JSON.stringify(queryAddTranscriptSegment)}`,
        "extensions": {
            "authorization": {
                "x-api-key": `${apiKey}`,
                "host": `${host}`
            }
        }
    },
    "type": "start"
};

const subOnUpdateCall = 
`subscription OnUpdateCall {
    onUpdateCall(CallId: "${callIdPlaceholder}") {
        PK
        SK
        CreatedAt
        UpdatedAt
        ExpiresAfter
        CallId
        CustomerPhoneNumber
        SystemPhoneNumber
        Status
        RecordingUrl
        PcaUrl
        TotalConversationDurationMillis
        AgentId
        Metadatajson
        CallCategories
        IssuesDetected
        CallSummaryText
    }
}`;

const querySubUpdateCall =
{
    "query":`${subOnUpdateCall}`,
    "variables":{}
};

export const registerOnUpdateCall = {
    "id": `${id_updateCall}`,
    "payload": {
        "data": `${JSON.stringify(querySubUpdateCall)}`,
        "extensions": {
            "authorization": {
                "x-api-key": `${apiKey}`,
                "host": `${host}`
            }
        }
    },
    "type": "start"
};

/**
 * UJJWAL: ADDED For Intent
 * Need to check and confirm if required or not
 */
const subOnCreateIntent = 
`subscription OnCreateIntent {
    onCreateIntent(callId: "${callIdPlaceholder}", intent: null) {
        callId
        intent
    }
}`;

/**
 * UJJWAL: ADDED For Intent
 * Need to check and confirm if required or not
 */
const querySubCreateIntent =
{
    "query":`${subOnCreateIntent}`,
    "variables":{}
};

/**
 * UJJWAL: ADDED For Intent
 * Need to check and confirm if required or not
 */
export const registerOnCreateIntent = {
    "id": `${id}`,
    "payload": {
        "data": `${JSON.stringify(querySubCreateIntent)}`,
        "extensions": {
            "authorization": {
                "x-api-key": `${apiKey}`,
                "host": `${host}`
            }
        }
    },
    "type": "start"
};

// Query for retrieving transcript segments
export const getTranscriptsQuery = {
    query:`query GetTranscriptSegments {
        getTranscriptSegments(CallId: "${callIdPlaceholder}") {
            nextToken
            TranscriptSegments {
                PK
                SK
                CreatedAt
                UpdatedAt
                ExpiresAfter
                CallId
                SegmentId
                StartTime
                EndTime
                Transcript
                IsPartial
                Channel
                Sentiment
                SentimentWeighted
                SentimentScore {
                    Positive
                    Negative
                    Neutral
                    Mixed
                }
            }
        }
    }`
};

// Query for retrieving call detail
export const getCallQuery = {
    query:`query GetCall {
        getCall(CallId: "${callIdPlaceholder}") {
            PK
            SK
            CreatedAt
            UpdatedAt
            ExpiresAfter
            CallId
            CustomerPhoneNumber
            SystemPhoneNumber
            Status
            RecordingUrl
            PcaUrl
            TotalConversationDurationMillis
            AgentId
            Metadatajson
            CallCategories
            IssuesDetected
            CallSummaryText
        }
    }`
};
/**
 * Format for Get IntentQuery
 */
export const getIntentQuery = {
    query:`query GetIntent {
        getIntent(callId: "${callIdPlaceholder}") {
            intent
            callId
        }
    }`
};
/**
 * Format for Get AutomationBotQuery
 */
export const automationBotQuery = {
    query: `query MyQuery {
        getAutomationBotResponse(inputBot: {
            botID: "${botId}",
            sessionId: "${sessionId}",
            channel: "channel",
            aliasId: "${aliasId}", 
            text: "${textPlaceholder}"
        })
    }`
};

// Unsubscribe from the subscription
export const unregisterSubscription = {
    "type":"stop",
    "id": `${id}`
};

export const consoleLog = (text) => {
    let outputColor = "color:green; font-size:24px;"
    console.log("%c "+text, outputColor);
};

export const errorToConsole = (text) => {
    let outputColor = "color:red; font-size:24px;"
console.error("%c "+text, outputColor);
};

export const convertMilliseconds = (ms) => {
    const minutes = Math.floor(ms / 60000);
    const seconds = ((ms % 60000) / 1000).toFixed(0);
    console.log("minutes ====== "+minutes+' >>>>>>>>> seconds ========= '+seconds);
    return minutes + ":" + (seconds < 10 ? '0' : '') + seconds;
};


// Constants defining various URIs and authentication details for AWS AppSync
const id_updateCall = "uc-45786c2d536d6172742d4167656e74";
export const callIdPlaceholder = `#callid_placeholder#`;
export const textPlaceholder = `#text_placeholder#`;
const aliasId = `TSTALIASID`;
const botId = `RHVLZ1UOBV`;
const sessionId = `220335042009125`;
export const socketProtocol = 'graphql-ws';

// Subscription query for receiving new transcript segments
const subOnAddTranscriptSegment =
    `subscription OnAddTranscriptSegment {
    onAddTranscriptSegment(CallId: "${callIdPlaceholder}") {
        __typename
        PK
        SK
        CreatedAt
        UpdatedAt
        ExpiresAfter
        CallId
        SegmentId
        StartTime
        EndTime
        Transcript
        IsPartial
        Channel
        Sentiment
        SentimentWeighted
        SentimentScore {
            Positive
            Negative
            Neutral
            Mixed
        }
    }
}`;

// Query object for adding transcript segments
export const queryAddTranscriptSegment =
{
    "query": `${subOnAddTranscriptSegment}`,
    "variables": {}
};

const subOnUpdateCall =
    `subscription OnUpdateCall {
    onUpdateCall(CallId: "${callIdPlaceholder}") {
        PK
        SK
        CreatedAt
        UpdatedAt
        ExpiresAfter
        CallId
        CustomerPhoneNumber
        SystemPhoneNumber
        Status
        RecordingUrl
        PcaUrl
        TotalConversationDurationMillis
        AgentId
        Metadatajson
        CallCategories
        IssuesDetected
        CallSummaryText
    }
}`;

const querySubUpdateCall =
{
    "query": `${subOnUpdateCall}`,
    "variables": {}
};

/*export const registerOnUpdateCall = {
    "id": `${id_updateCall}`,
    "payload": {
        "data": `${JSON.stringify(querySubUpdateCall)}`,
        "extensions": {
            "authorization": {
                "x-api-key": `${apiKey}`,
                "host": `${host}`
            }
        }
    },
    "type": "start"
};
*/
export const consoleLog = (text) => {
    let outputColor = "color:green; font-size:24px;"
    console.trace("%c EXL: " + text, outputColor);
};

export const errorToConsole = (text) => {
    let outputColor = "color:red; font-size:24px;"
    console.trace("%c EXL: " + text, outputColor);
};

export const Speaker = Object.freeze({
    AGENT: 'AGENT',
    CALLER: 'CALLER',
    CUSTOMER: 'CUSTOMER'
});

export const Sentiment = Object.freeze({
    POSITIVE: 'POSITIVE',
    NEGATIVE: 'NEGATIVE',
    NEUTRAL: 'NEUTRAL'
});

export const Color = Object.freeze({
    POSITIVE_COLOR: '#49DAA1',
    NEUTRAL_COLOR: '#3186FB',
    NEGATIVE_COLOR: '#E8837B'
});

export const AGENT_TYPES = new Set(["Jane", "Elizabeth", "Agent"]);

export const CALL_STATUS = {
    CONNECTED: "CONNECTED",
    ENDED: "ENDED"
}

export const SPEAKER = { 
    CALLER: 'CALLER'
}

export const VISIBILITY = { 
    HIDE: "Hide" ,
    SHOW:"Show More"
}

export const FIELDS = {
   ADDRESS: "newAddress",
   DATE:"movementDate"
}

export const WORKFLOW = {
   START_WORKFLOW: "start workflow"
}

export const NUDGES = {
    AGENT_ASSISTANT:"AGENT_ASSISTANT",
    AI_GUIDANCE:"AI Guidance",
    KNOWLEDGE_ARTICLES:"Knowledge Articles"
}

export const CALL_NOTES = {
    "statements": [
        "A Payment Due date change request was raised by the John Doe due to the non-availability of the customer as He is not available for this weekend",
        "Date changed to 15/04/2024",
        "Amount changed to $2000"
    ]
};

export const CALL_SUMMARY_AUDITS = [
    {
        id: "Question1",
        question: "Was the captured intent correct?",
        options: [
            {key: "a", value: "Yes", booleanValue: true, checked: true},
            {key: "b", value: "No", booleanValue: false, checked: false}
        ],
        answer: true,
        interactionHistoryPayloadKey: "Was_the_captured_intent_correct__c"
    },
    {
        id: "Question2",
        question: "Was this issue resolved?",
        options: [
            {key: "a", value: "Yes", booleanValue: true, checked: true},
            {key: "b", value: "No", booleanValue: false, checked: false}
        ],
        answer: true,
        interactionHistoryPayloadKey: "Was_this_issue_resolved__c"
    },
    {
        id: "Question3",
        question: "Is any follow-up needed?",
        options: [
            {key: "a", value: "Yes", booleanValue: true, checked: false},
            {key: "b", value: "No", booleanValue: false, checked: true}
        ],
        answer: false,
        interactionHistoryPayloadKey: "Is_any_follow_up_needed__c"
    }
];

export const USER_FEEDBACK_EVENTS = {
    LIKED: "liked",
    LIKE: "like",
    DISLIKED: "disliked",
    DISLIKE: "dislike"
}
