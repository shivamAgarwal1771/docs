import { Field, InputType, ObjectType } from '@nestjs/graphql';

@ObjectType()
@InputType('AddCallCategoryInputType')
export class AddCallCategoryInput {
  @Field()
  CallId: String;
  @Field(() => [String])
  CallCategories?: [String];
  @Field()
  UpdatedAt?: String;
}

@ObjectType()
@InputType('AddCallSummaryTextInputType')
export class AddCallSummaryTextInput {
  @Field()
  CallId: String;
  @Field()
  CallSummaryText?: String;
  @Field({ nullable: true })
  UpdatedAt?: String;
}

@ObjectType()
@InputType('CustomSettingsInputType')
export class CustomSettingsInput {
  @Field({ nullable: true })
  LexAliasId: String;
  @Field({ nullable: true })
  LexBotId: String;
}
@ObjectType()
@InputType('BotInputType')
export class BotInput {
  @Field()
  LexAliasId: String;
  @Field()
  LexBotId: String;
  @Field()
  LexBotType: String;
}
@ObjectType()
@InputType('BotCustomSettingsInputType')
export class BotCustomSettingsInput {
  @Field(() => [BotInput])
  BotDetails: [BotInput];
}

@ObjectType()
@InputType('CreateCallInputType')
export class CreateCallInput {
  @Field()
  CallId: string;
  @Field({ nullable: true })
  AgentId: string;
  @Field({ nullable: true })
  CallLanguage: string;
  @Field({ nullable: true })
  CustomerPhoneNumber: string;
  @Field({ nullable: true })
  SystemPhoneNumber: string;
  @Field({ nullable: true })
  Metadatajson: string;
  @Field({ nullable: true })
  ExpiresAfter: number;
  @Field(() => CustomSettingsInput, { nullable: true })
  CustomSettings: CustomSettingsInput;
  @Field(() => BotCustomSettingsInput, { nullable: true })
  BotCustomSettings: BotCustomSettingsInput;
}

@ObjectType()
@InputType('UpdateRecordingUrlInputType')
export class UpdateRecordingUrlInput {
  @Field()
  CallId: string;
  @Field()
  RecordingUrl: string;
}

@ObjectType()
@InputType('UpdatePcaUrlInputType')
export class UpdatePcaUrlInput {
  @Field()
  CallId: string;
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
  @Field({ defaultValue: new Date().toISOString() })
  CreatedAt?: String;
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
@InputType('SubscriptionFilterAddTranscriptInputType')
export class SubscriptionFilterAddTranscriptInput {
  @Field({ nullable: true })
  CallId?: String
  @Field({ nullable: true })
  Channel?: Channel
}

@ObjectType()
@InputType('getTranscriptSegmentInputType')
export class getTranscriptSegmentInput {
  @Field()
  CallId: String;
  @Field({ nullable: true, defaultValue: false })
  isPartial?: Boolean;
}

@ObjectType()
@InputType('getTranscriptSegmentsWithSentimentInputType')
export class getTranscriptSegmentsWithSentimentInput {
  @Field()
  CallId: String;
}

@ObjectType()
@InputType('updateCallCustomSettingsInputType')
export class UpdateCallCustomSettingsInput {
  @Field()
  CallId: String;
  @Field(() => CustomSettingsInput, { nullable: true })
  CustomSettings?: CustomSettingsInput;
  @Field({ nullable: true })
  UpdatedAt?: String;
}

@ObjectType()
@InputType('UpdateCallBotCustomSettingsInputType')
export class UpdateCallBotCustomSettingsInput {
  @Field()
  CallId: String;
  @Field(() => BotCustomSettingsInput, { nullable: true })
  BotCustomSettings?: BotCustomSettingsInput;
  @Field({ nullable: true })
  UpdatedAt?: String;
}

input.ts

import { Field, ID, ObjectType } from '@nestjs/graphql';

export type DynamoDBKey = {
  PK: String;
  SK: String;
};

@ObjectType()
export class DynamoDbBase {
  @Field()
  PK: String;
  @Field()
  SK: String;
  @Field()
  CreatedAt: String;
  @Field({ nullable: true })
  UpdatedAt?: String;
  @Field({ nullable: true })
  ExpiresAfter?: Number;
}
@ObjectType()
export class Bot {
  @Field()
  LexAliasId: String;
  @Field()
  LexBotId: String;
  @Field()
  LexBotType: String;
}
@ObjectType()
export class BotCustomSettings {
  @Field(() => [Bot])
  BotDetails: [Bot];
}
@ObjectType()
export class CustomSettings {
  @Field()
  LexAliasId: String;
  @Field()
  LexBotId: String;
}
@ObjectType()
export class Call extends DynamoDbBase {
  // @Field()
  // PK: String;
  // @Field()
  // SK: String;
  // @Field()
  // CreatedAt: String;
  // @Field({ nullable: true })
  // ExpiresAfter: Number;
  // @Field({ nullable: true })
  // UpdatedAt: String;

  @Field()
  CallId: String;
  @Field({ nullable: true })
  AgentId: String;
  @Field({ nullable: true })
  CallLanguage: String;
  @Field({ nullable: true })
  SystemPhoneNumber: String;
  @Field({ nullable: true })
  Metadatajson: String;
  @Field(() => CustomSettings, { nullable: true })
  CustomSettings: CustomSettings;
  @Field(() => BotCustomSettings, { nullable: true })
  BotCustomSettings: BotCustomSettings;
  @Field({ nullable: true })
  RecordingUrl?: String;
  @Field({ nullable: true })
  PcaUrl?: String;
  @Field({ nullable: true })
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

@ObjectType()
export class TranscriptSegmentList {
  @Field(() => [TranscriptSegment], { nullable: true })
  TranscriptSegments: TranscriptSegment[]
  @Field({ nullable: true })
  nextToken: String
}

@ObjectType()
export class TranscriptSegmentsWithSentimentList {
  @Field(() => [TranscriptSegment], { nullable: true })
  TranscriptSegmentsWithSentiment: TranscriptSegment[]
  @Field({ nullable: true })
  nextToken: String
}

model.ts

import { Schema } from 'dynamoose';

export const CallSchema = new Schema(
  {
    PK: String,
    SK: String,
    CallId: String,
    AgentId: String,
    CreatedAt: String,
    UpdatedAt: String,
    ExpiresAfter: Number,
    CallLanguage: String,
    CustomerPhoneNumber: String,
    SystemPhoneNumber: String,
    Metadatajson: String,
    CustomSettings: Object,
    BotCustomSettings: Object,
  },{
    "saveUnknown": true
  }
);

schema.ts

import { Module } from '@nestjs/common';
import { CallService } from './call.service';
import { CallResolver } from './call.resolver';
import {
  ApolloDriver,
  ApolloDriverConfig,
} from '@nestjs/apollo';
import { GraphQLModule } from '@nestjs/graphql';
import { join } from 'path';
import { DynamooseModule } from 'nestjs-dynamoose';
import { CallSchema } from './schema/call.schema';

@Module({
  imports: [
    DynamooseModule.forFeature([
      {
        name: 'call',
        schema: CallSchema,
      },
    ]),
    GraphQLModule.forRoot<ApolloDriverConfig>({
      driver: ApolloDriver,
      playground: true,
      installSubscriptionHandlers: true,
      autoSchemaFile: join(process.cwd(), 'src/schema.gql'),
    }),
  ],
  providers: [CallResolver, CallService],
})
export class CallModule {}

module.ts

import { Resolver, Query, Mutation, Args, Subscription } from '@nestjs/graphql';
import { CallService } from './call.service';
import { Call, TranscriptSegment, TranscriptSegmentList, TranscriptSegmentsWithSentimentList } from './model/call.model';
import { CreateCallInput, UpdateRecordingUrlInput, UpdatePcaUrlInput, AddTranscriptInput, AddCallSummaryTextInput, UpdateCallCustomSettingsInput, UpdateCallBotCustomSettingsInput, getTranscriptSegmentInput, SubscriptionFilterAddTranscriptInput, getTranscriptSegmentsWithSentimentInput } from './input/call.input';
import { PubSub } from 'graphql-subscriptions';
import Utility from 'src/utillity';
import { InternalServerErrorException, BadRequestException } from '@nestjs/common';
import { ERROR_MESSAGES, LOG_TYPE } from 'src/utillity/constant';

@Resolver(() => Call)
export class CallResolver {
  private pubSub = null;
  constructor(private readonly callService: CallService) {
    this.pubSub = new PubSub();
  }

  @Query(() => [Call], { name: 'getCalls' })
  async findAll() : Promise<Call[]> {
    return this.callService.findAll();
  }

  @Mutation(() => Call, { name: 'createCall' })
  async createCall(@Args('input') input: CreateCallInput): Promise<Call> {
    const [createCallError, createCallResponse] = await Utility.parseResponse(this.callService.createCall(input));
    if(createCallError){
      Utility.smartAgentLog('createCall', ERROR_MESSAGES.CREATE_CALL, createCallError, LOG_TYPE.ERROR)
      throw new InternalServerErrorException();
    }
    this.pubSub.publish('onCreateCall', { onCreateCall: createCallResponse });
    return createCallResponse
  }

  @Subscription(() => Call, {
    name: 'onCreateCall',
  })
  onCreateCall() {
    return this.pubSub.asyncIterator('onCreateCall');
  }

  @Mutation(() => Call, { name: 'updateRecordingUrl' })
  async updateRecordingUrl(@Args('input') input: UpdateRecordingUrlInput): Promise<Call> {
    const [updateRecordingUrlError, updateRecordingUrlResponse] = await Utility.parseResponse(this.callService.updateRecordingUrl(input))

    if(updateRecordingUrlError){
      Utility.smartAgentLog('updateRecordingUrl', ERROR_MESSAGES.UPDATE_RECORDING_URL, updateRecordingUrlError, LOG_TYPE.ERROR)
      throw new BadRequestException();
    }
    return updateRecordingUrlResponse
  }

  @Mutation(() => Call, { name: 'updatePcaUrl' })
  async updatePcaUrl(@Args('input') input: UpdatePcaUrlInput): Promise<Call> {
    const [updatePcaUrlError, updatePcaUrlResponse] = await Utility.parseResponse(this.callService.updatePcaUrl(input))

    if(updatePcaUrlError){
      Utility.smartAgentLog('updatePcaUrlError', ERROR_MESSAGES.UPDATE_PCA_URL, updatePcaUrlError, LOG_TYPE.ERROR)
      throw new BadRequestException();
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

  @Mutation(() => TranscriptSegment, { name: 'addTranscriptSegment' })
  async addTranscriptSegment(@Args('input') input: AddTranscriptInput): Promise<TranscriptSegment> {
    const [addTranscriptSegmentError, addTranscriptSegmentResponse] = await Utility.parseResponse(this.callService.addTranscriptSegment(input));

    if (addTranscriptSegmentError) {
      Utility.smartAgentLog('addTranscriptSegmentError', ERROR_MESSAGES.ADD_TRANSCRIPT_SEGMENT, addTranscriptSegmentError, LOG_TYPE.ERROR)
      throw new BadRequestException();
    }
    this.pubSub.publish('onAddTranscriptSegment', { onAddTranscriptSegment: addTranscriptSegmentResponse });
    return addTranscriptSegmentResponse;
  };

  @Subscription(() => TranscriptSegment, {
    name: 'onAddTranscriptSegment',
    filter: (payload, variables) => {
      if (!variables?.input) return true;

      const { CallId, Channel } = variables?.input;
      return (!CallId || payload?.onAddTranscriptSegment?.CallId === CallId)
        && (!Channel || payload?.onAddTranscriptSegment?.Channel === Channel)
    },
  })
  onAddTranscriptSegment(@Args('input') input: SubscriptionFilterAddTranscriptInput) {
    return this.pubSub.asyncIterator('onAddTranscriptSegment');
  }

  @Query(() => TranscriptSegmentList, { name: 'getTranscriptSegments' })
  async getTranscriptSegments(@Args('input') input: getTranscriptSegmentInput): Promise<TranscriptSegmentList> {
    const [getTranscriptSegmentsError, getTranscriptSegmentsResponse] = await Utility.parseResponse(this.callService.getTranscriptSegments(input));

    if (getTranscriptSegmentsError) {
      Utility.smartAgentLog('getTranscriptSegments', ERROR_MESSAGES.GET_TRANSCRIPTS_SEGMENTS, getTranscriptSegmentsError, LOG_TYPE.ERROR)
      throw new BadRequestException();
    }
    return getTranscriptSegmentsResponse;
  }

  @Query(() => TranscriptSegmentsWithSentimentList, { name: 'getTranscriptSegmentsWithSentiment' })
  async getTranscriptSegmentsWithSentiment(@Args('input') input: getTranscriptSegmentsWithSentimentInput): Promise<TranscriptSegmentsWithSentimentList> {
    const [getTranscriptSegmentsWithSentimentError, getTranscriptSegmentsWithSentimentResponse] = await Utility.parseResponse(this.callService.getTranscriptSegmentsWithSentiment(input));

    if (getTranscriptSegmentsWithSentimentError) {
      Utility.smartAgentLog('getTranscriptSegmentsWithSentimentError', ERROR_MESSAGES.GET_TRANSCRIPT_SEGMENTS_WITH_SENTIMENT, getTranscriptSegmentsWithSentimentError, LOG_TYPE.ERROR)
      throw new BadRequestException();
    }
    return getTranscriptSegmentsWithSentimentResponse;
  };

  @Mutation(() => Call, { name: 'updateCallCustomSettings' })
  async updateCallCustomSettings(@Args('input') input: UpdateCallCustomSettingsInput): Promise<Call> {
    const [updateCallCustomSettingsError, updateCallCustomSettingsResponse] = await Utility.parseResponse(this.callService.updateCallCustomSettings(input));

    if (updateCallCustomSettingsError) {
      Utility.smartAgentLog('updateCallCustomSettings', ERROR_MESSAGES.UPDATE_CALL_CUSTOM_SETTINGS, updateCallCustomSettingsError, LOG_TYPE.ERROR)
      throw new BadRequestException();
    }
    return updateCallCustomSettingsResponse;
  }

  @Mutation(() => Call, { name: 'updateCallBotCustomSettings' })
  async updateCallBotCustomSettings(@Args('input') input: UpdateCallBotCustomSettingsInput): Promise<Call> {
    const [updateCallBotCustomSettingsError, updateCallBotCustomSettingsResponse] = await Utility.parseResponse(this.callService.updateCallBotCustomSettings(input));

    if (updateCallBotCustomSettingsError) {
      Utility.smartAgentLog('updateCallBotCustomSettings', ERROR_MESSAGES.UPDATE_CALL_BOT_CUSTOM_SETTINGS, updateCallBotCustomSettingsError, LOG_TYPE.ERROR)
      throw new BadRequestException();
    }
    return updateCallBotCustomSettingsResponse;
  }
}

resolver.ts

import { Injectable, InternalServerErrorException } from '@nestjs/common';
import { InjectModel, Model } from 'nestjs-dynamoose';
import { Condition } from 'dynamoose'
import { Call, DynamoDBKey, TranscriptSegment, TranscriptSegmentList } from './model/call.model';
import { CreateCallInput, UpdateRecordingUrlInput, UpdatePcaUrlInput, AddTranscriptInput, AddCallSummaryTextInput, UpdateCallCustomSettingsInput, UpdateCallBotCustomSettingsInput, getTranscriptSegmentInput, getTranscriptSegmentsWithSentimentInput } from './input/call.input';
import Utility from 'src/utillity';
import { ERROR_MESSAGES, CONSTANTS, LOG_TYPE } from 'src/utillity/constant';

@Injectable()
export class CallService {
  constructor(
    @InjectModel('call')
    private readonly model: Model<Call, DynamoDBKey>,
    @InjectModel('call')
    private readonly transcriptModel: Model<TranscriptSegment, DynamoDBKey>
  ) { }

  async createCall(call: CreateCallInput) {
    const keys = {
      PK: `c#${call.CallId}`,
      SK: `c#${call.CallId}`,
      CreatedAt: new Date().toISOString(),
      UpdatedAt: new Date().toISOString(),
      ExpiresAfter: Date.now() + 604800 // 1 Week Expiration
    }
    return this.model.create({ ...keys, ...call });
  }

  findAll(): Promise<Call[]> {
    return this.model.scan().exec();
  }

  async updateRecordingUrl(input: UpdateRecordingUrlInput) {
    const key = {
      PK: `c#${input.CallId}`,
      SK: `c#${input.CallId}`
    }

    const args = {
      UpdatedAt: new Date().toISOString(),
      RecordingUrl: input.RecordingUrl
    }

    return this.model.update(key, args, {
      condition: new Condition().filter(CONSTANTS.PK).eq(key.PK),
      return: 'item'
    })
  }

  async updatePcaUrl(input: UpdatePcaUrlInput) {
    const key = {
      PK: `c#${input.CallId}`,
      SK: `c#${input.CallId}`
    }
    const args = {
      UpdatedAt: new Date().toISOString(),
      PcaUrl: input.PcaUrl
    }

    return this.model.update(key, args, {
      condition: new Condition().where(CONSTANTS.PK).eq(key.PK),
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
      condition: new Condition().where(CONSTANTS.PK).eq(key.PK),
      return: 'item'
    });
  }

  async addTranscriptSegment(input: AddTranscriptInput) {
    const keys = {
      PK: `trs#${input.CallId}`,
      SK: `s#${input.SegmentId}`
    }

    const args = {
      SK: `s#${input.SegmentId}`,
      CreatedAt: input?.CreatedAt || new Date().toISOString(),
      ExpiresAfter: Date.now() + 604800,
      ...input
    }

    let newCondition;

    if (input.IsPartial) {
      newCondition = new Condition().attribute(CONSTANTS.IS_PARTIAL).not().exists().or().where(CONSTANTS.IS_PARTIAL).eq(true)
    } else {
      newCondition = new Condition().attribute(CONSTANTS.SENTIMENT).not().exists().or().where(CONSTANTS.SENTIMENT).eq(null)
    }

    return this.transcriptModel.update(keys, args, {
      condition: newCondition,
      return: 'item'
    });
  }

  async getTranscriptSegments(input: getTranscriptSegmentInput) {
    const keys = {
      PK: `trs#${input?.CallId}`
    };

    let result, nextToken, items;
    try {
      result = await this.model.query(CONSTANTS.PK).eq(keys.PK).where(CONSTANTS.IS_PARTIAL).eq(input?.isPartial).exec()
      nextToken = result?.lastKey;
      items = result.find((items) => items)
    } catch (error) {
      console.error(error)
    }

    return { TranscriptSegments: [items], nextToken }
  }

  async getTranscriptSegmentsWithSentiment(input: getTranscriptSegmentsWithSentimentInput) {
    const keys = {
      PK: `trs#${input?.CallId}`,
    };
    const args = {
      isPartial: false
    };

    let result, nextToken, items;
    try {
      result = await this.model.query(CONSTANTS.PK).eq(keys.PK).where(CONSTANTS.IS_PARTIAL).eq(args?.isPartial).exec()
      nextToken = result?.lastKey;
      items = result.find((items) => items)
    } catch (error) {
      console.error(error)
    }

    return { TranscriptSegmentsWithSentiment: [items], nextToken }
  }

  async updateCallCustomSettings(input: UpdateCallCustomSettingsInput) {
    const keys = {
      PK: `c#${input?.CallId}`,
      SK: `c#${input?.CallId}`
    }

    const args = {
      Status: CONSTANTS.STATUS.STARTED,
      CreatedAt: new Date().toISOString(),
      UpdatedAt: new Date().toISOString(),
      CustomSettings: input?.CustomSettings
    };

    return this.model.update(keys, args, {
      condition: new Condition().where(CONSTANTS.PK).eq(keys.PK),
      return: 'item'
    })
  }

  async updateCallBotCustomSettings(input: UpdateCallBotCustomSettingsInput) {
    const keys = {
      PK: `c#${input?.CallId}`,
      SK: `c#${input?.CallId}`
    }

    const args = {
      Status: CONSTANTS.STATUS.STARTED,
      CreatedAt: new Date().toISOString(),
      UpdatedAt: new Date().toISOString(),
      BotCustomSettings: input?.BotCustomSettings
    };

    return this.model.update(keys, args, {
      condition: new Condition().where(CONSTANTS.PK).eq(keys.PK),
      return: 'item'
    })
  }
}

service.ts

