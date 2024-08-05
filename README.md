conversation-microservice/src/call/call.resolver.ts
Browse file contents
Comment on file
 import { Resolver, Query, Mutation, Args, Subscription } from '@nestjs/graphql';
 import { CallService } from './call.service';
 import { Call } from './model/call.model';
 import { CreateCallInput, UpdateRecordingUrlInput, UpdatePcaUrlInput } from './input/call.input';
 import { Call, TranscriptSegment } from './model/call.model';
 import { CreateCallInput, UpdateRecordingUrlInput, UpdatePcaUrlInput, AddTranscriptInput, AddCallSummaryTextInput, UpdateCallCustomSettingsInput } from './input/call.input';
 import { PubSub } from 'graphql-subscriptions';
 import Utility from 'src/utillity';
 import { InternalServerErrorException, BadRequestException } from '@nestjs/common';
@@ -58,4 +58,38 @@
     }
     return updatePcaUrlResponse
   }
 
   @Mutation(() => Call, {name: 'addCallSummaryText'})
   async addCallSummaryText(@Args('input') input: AddCallSummaryTextInput): Promise<Call> {
     const [addCallSummaryTextError, addCallSummaryTextResponse] = await Utility.parseResponse(this.callService.addCallSummaryText(input));
 
     if(addCallSummaryTextError) {
       Utility.smartAgentLog('addTranscriptError', ERROR_MESSAGES.ADD_CALL_SUMMARY, addCallSummaryTextError, LOG_TYPE.ERROR)
       throw new BadRequestException();
     }
 
     return addCallSummaryTextResponse;
   }
 
   @Mutation(() => TranscriptSegment, { name: 'addTranscript' })
   async addTranscript(@Args('input') input: AddTranscriptInput): Promise<TranscriptSegment> {
     const [addTranscriptError, addTranscriptResponse] = await Utility.parseResponse(this.callService.addTranscript(input));
 
     if(addTranscriptError) {
       Utility.smartAgentLog('addTranscriptError', ERROR_MESSAGES.ADD_TRANSCRIPT, addTranscriptError, LOG_TYPE.ERROR)
       throw new BadRequestException();
     }
     return addTranscriptResponse;
   };
 
   @Mutation(() => Call, {name: 'updateCallCustomSettings'})
   async updateCallCustomSettings(@Args('input') input: UpdateCallCustomSettingsInput): Promise<Call> {
     const [updateCallCustomSettingsError, updateCallCustomSettingsResponse] = await Utility.parseResponse(this.callService.updateCallCustomSettings(input));
 
     if(updateCallCustomSettingsError) {
       Utility.smartAgentLog('addTranscriptError', ERROR_MESSAGES.UPDATE_CALL_CUSTOM_SETTINGS, updateCallCustomSettingsError, LOG_TYPE.ERROR)
       throw new BadRequestException();
     }
     return updateCallCustomSettingsResponse;
   }
 }

conversation-microservice/src/call/call.service.ts
Browse file contents
Comment on file
 import { Injectable, InternalServerErrorException } from '@nestjs/common';
 import { InjectModel, Model } from 'nestjs-dynamoose';
 import { Condition } from 'dynamoose'
 import { Call, DynamoDBKey } from './model/call.model';
 import { CreateCallInput, UpdateRecordingUrlInput, UpdatePcaUrlInput } from './input/call.input';
 import { Call, DynamoDBKey, TranscriptSegment } from './model/call.model';
 import { CreateCallInput, UpdateRecordingUrlInput, UpdatePcaUrlInput, AddTranscriptInput, AddCallSummaryTextInput, UpdateCallCustomSettingsInput } from './input/call.input';
 import Utility from 'src/utillity';
 import { ERROR_MESSAGES, LOG_TYPE } from 'src/utillity/constant';
  
@@ -10,7 +10,9 @@
 export class CallService {
   constructor(
     @InjectModel('call')
     private readonly model: Model<Call, DynamoDBKey>
     private readonly model: Model<Call, DynamoDBKey>,
     @InjectModel('call')
     private readonly transcriptModel: Model<TranscriptSegment, DynamoDBKey>
   ) { }
  
   async createCall(call: CreateCallInput) {
@@ -61,4 +62,67 @@
       return: 'item'
     })
   }
 
   async addCallSummaryText(input: AddCallSummaryTextInput) {
     const key = {
       PK: `c#${input.CallId}`,
       SK: `c#${input.CallId}`
     }
 
     const args = {
       UpdatedAt: new Date().toISOString(),
       CallSummaryText: input?.CallSummaryText
     }
 
     return this.model.update(key, args, {
       condition: new Condition().where("PK").eq(key.PK),
       return: 'item'
     });
   }
 
   async addTranscript(input: AddTranscriptInput) {
     const keys = {
       PK: `trs#${input.CallId}`,
       SK: `s#${input.SegmentId}`
     }
 
     const args = {
       SK: `s#${input.SegmentId}`,
       CreatedAt: new Date().toISOString(),
       ExpiresAfter: Date.now() + 604800,
       ...input
     }
 
     let newCondition;
 
     if (input.IsPartial) {
       newCondition = new Condition().attribute("IsPartial").not().exists().or().where("IsPartial").eq(true)
     } else {
       newCondition = new Condition().attribute("Sentiment").not().exists().or().where("Sentiment").eq(null)
     }
 
     return this.transcriptModel.update(keys, args, {
       condition: newCondition,
       return: 'item'
     });
   }
 
   async updateCallCustomSettings(input: UpdateCallCustomSettingsInput) {
     const keys = {
       PK: `c#${input?.CallId}`,
       SK: `c#${input?.CallId}`
     }
 
     const args = {
       Status: "STARTED",
       CreatedAt: new Date().toISOString(),
       UpdatedAt: new Date().toISOString(),
       CustomSettings: input?.CustomSettings
     };
 
     return this.model.update(keys, args, {
       condition: new Condition().where("PK").eq(keys.PK),
       return: 'item'
     })
   }
 }

conversation-microservice/src/call/input/call.input.ts
Browse file contents
Comment on file
@@ -18,9 +18,10 @@
   CallId: String;
   @Field()
   CallSummaryText?: String;
   @Field()
   @Field({ nullable: true })
   UpdatedAt?: String;
 }
 
 @ObjectType()
 @InputType('CustomSettingsInputType')
 export class CustomSettingsInput {
@@ -24,9 +25,9 @@
 @ObjectType()
 @InputType('CustomSettingsInputType')
 export class CustomSettingsInput {
   @Field()
   @Field({ nullable: true })
   LexAliasId: String;
   @Field()
   @Field({ nullable: true })
   LexBotId: String;
 }
 @ObjectType()
@@ -86,3 +87,84 @@
   @Field()
   PcaUrl: string;
 }
 
 enum CallStatus {
   STARTED = 'STARTED',
   TRANSCRIBING = 'TRANSCRIBING',
   ERRORED = 'ERRORED',
   ENDED = 'ENDED',
 }
 
 enum Channel {
   CALLER = 'CALLER',
   AGENT = 'AGENT',
   AGENT_VOICETONE = 'AGENT_VOICETONE',
   CALLER_VOICETONE = 'CALLER_VOICETONE',
   AGENT_ASSISTANT = 'AGENT_ASSISTANT',
   CATEGORY_MATCH = 'CATEGORY_MATCH'
 }
 
 enum Sentiment {
   POSITIVE = 'POSITIVE',
   NEGATIVE = 'NEGATIVE',
   NEUTRAL = 'NEUTRAL',
   MIXED = 'MIXED'
 };
 
 @ObjectType()
 @InputType('SentimentScoreInputType')
 export class SentimentScoreInput {
   @Field()
   Positive: String;
   @Field()
   Negative: String;
   @Field()
   Neutral: String;
   @Field()
   Mixed: String
 }
 
 @ObjectType()
 @InputType('AddTranscriptInputType')
 export class AddTranscriptInput {
   @Field()
   CallId: string;
   @Field()
   // @IsEnum(CallStatusInput)
   Status: CallStatus;
   @Field()
   SegmentId: string;
   @Field()
   StartTime: Number;
   @Field()
   EndTime: Number;
   @Field()
   Transcript: string;
   @Field({ nullable: true, defaultValue: null })
   TranslatedTranscript: string;
   @Field()
   IsPartial: Boolean;
   @Field()
   Channel: Channel;
   // @Field({ nullable: true })
   // CreatedAt: Date;
   // @Field({ nullable: true })
   // ExpiresAfter: Number; //TimeStamp
   @Field({ nullable: true, defaultValue: null })
   Sentiment: Sentiment;
   @Field({ nullable: true, defaultValue: null })
   SentimentScore: SentimentScoreInput;
   @Field({ nullable: true, defaultValue: null })
   SentimentWeighted: Number;
 }
 
 @ObjectType()
 @InputType('updateCallCustomSettingsInput')
 export class UpdateCallCustomSettingsInput {
   @Field()
   CallId: String;
   @Field(() => CustomSettingsInput, { nullable: true })
   CustomSettings?: CustomSettingsInput;
   @Field({ nullable: true })
   UpdatedAt?: String;
 }

conversation-microservice/src/call/model/call.model.ts
Browse file contents
Comment on file
@@ -70,4 +70,61 @@
   RecordingUrl?: String;
   @Field({ nullable: true })
   PcaUrl?: String;
   @Field({nullable: true})
   CallSummaryText?: String
 }
 
 
 enum Channel {
   CALLER = 'CALLER',
   AGENT = 'AGENT',
   AGENT_VOICETONE = 'AGENT_VOICETONE',
   CALLER_VOICETONE = 'CALLER_VOICETONE',
   AGENT_ASSISTANT = 'AGENT_ASSISTANT',
   CATEGORY_MATCH = 'CATEGORY_MATCH'
 }
 
 enum Sentiment {
   POSITIVE = 'POSITIVE',
   NEGATIVE = 'NEGATIVE',
   NEUTRAL = 'NEUTRAL',
   MIXED = 'MIXED'
 };
 
 @ObjectType()
 export class SentimentScore {
   @Field()
   Positive: String;
   @Field()
   Negative: String;
   @Field()
   Neutral: String;
   @Field()
   Mixed: String
 }
 
 @ObjectType()
 export class TranscriptSegment extends DynamoDbBase {
   @Field()
   CallId: String;
   @Field()
   SegmentId: String;
   @Field()
   StartTime: Number;
   @Field()
   EndTime: Number;
   @Field()
   Transcript: String;
   @Field({ nullable: true })
   TranslatedTranscript: String;
   @Field()
   IsPartial: Boolean;
   @Field()
   Channel: Channel;
   @Field({ nullable: true })
   Sentiment: Sentiment;
   @Field(() => SentimentScore, { nullable: true })
   SentimentScore: SentimentScore;
   @Field({ nullable: true })
   SentimentWeighted: Number;
 }

conversation-microservice/src/schema.gql
Browse file contents
Comment on file
@@ -32,11 +32,38 @@
   BotCustomSettings: BotCustomSettings
   RecordingUrl: String
   PcaUrl: String
   CallSummaryText: String
 }
 
 type SentimentScore {
   Positive: String!
   Negative: String!
   Neutral: String!
   Mixed: String!
 }
 
 type TranscriptSegment {
   PK: String!
   SK: String!
   CreatedAt: String!
   UpdatedAt: String
   ExpiresAfter: Float
   CallId: String!
   SegmentId: String!
   StartTime: Float!
   EndTime: Float!
   Transcript: String!
   TranslatedTranscript: String
   IsPartial: Boolean!
   Channel: String!
   Sentiment: String
   SentimentScore: SentimentScore
   SentimentWeighted: Float
 }
  
 type CustomSettingsInput {
   LexAliasId: String!
   LexBotId: String!
   LexAliasId: String
   LexBotId: String
 }
  
 type BotInput {
@@ -49,9 +76,16 @@
   BotDetails: [BotInput!]!
 }
  
 type SentimentScoreInput {
   Positive: String!
   Negative: String!
   Neutral: String!
   Mixed: String!
 }
 
 input CustomSettingsInputType {
   LexAliasId: String!
   LexBotId: String!
   LexAliasId: String
   LexBotId: String
 }
  
 input BotInputType {
@@ -64,6 +98,13 @@
   BotDetails: [BotInputType!]!
 }
  
 input SentimentScoreInputType {
   Positive: String!
   Negative: String!
   Neutral: String!
   Mixed: String!
 }
 
 type Query {
   getCalls: [Call!]!
 }
@@ -72,6 +113,9 @@
   createCall(input: CreateCallInputType!): Call!
   updateRecordingUrl(input: UpdateRecordingUrlInputType!): Call!
   updatePcaUrl(input: UpdatePcaUrlInputType!): Call!
   addCallSummaryText(input: AddCallSummaryTextInputType!): Call!
   addTranscript(input: AddTranscriptInputType!): TranscriptSegment!
   updateCallCustomSettings(input: updateCallCustomSettingsInput!): Call!
 }
  
 input CreateCallInputType {
@@ -96,6 +140,33 @@
   PcaUrl: String!
 }
  
 input AddCallSummaryTextInputType {
   CallId: String!
   CallSummaryText: String!
   UpdatedAt: String
 }
 
 input AddTranscriptInputType {
   CallId: String!
   Status: String!
   SegmentId: String!
   StartTime: Float!
   EndTime: Float!
   Transcript: String!
   TranslatedTranscript: String = null
   IsPartial: Boolean!
   Channel: String!
   Sentiment: String = null
   SentimentScore: SentimentScoreInputType = null
   SentimentWeighted: Float = null
 }
 
 input updateCallCustomSettingsInput {
   CallId: String!
   CustomSettings: CustomSettingsInputType
   UpdatedAt: String
 }
 
 type Subscription {
   onCreateCall: Call!
 }

conversation-microservice/src/utillity/constant.ts
Browse file contents
Comment on file
 export const ERROR_MESSAGES = {
   CREATE_CALL : "Unable to create call!",
   UPDATE_RECORDING_URL: "Unable to update recording URL!",
   UPDATE_PCA_URL: "Unable to update PCA URL!"
   UPDATE_PCA_URL: "Unable to update PCA URL!",
   ADD_TRANSCRIPT: "Unable to add transcript!",
   ADD_CALL_SUMMARY: "Unable to add call summary!",
   UPDATE_CALL_CUSTOM_SETTINGS: "Unable to update call custom settings!"
 }
  
 export const LOG_TYPE = {
