// call.service.ts
import { Injectable } from '@nestjs/common';
import { InjectModel, Model } from 'nestjs-dynamoose';
import { UpdateAgentInput, AddIssueDetectedInput, CallCategoryInput } from './input/call.input';
import { UpdateAgentResponse, AddIssueDetectedResponse, CallCategoryResponse } from './model/call.model';
import { Condition } from 'dynamoose'; // Ensure Condition is imported
import { Call } from './model/call.model'; // Import Call model

@Injectable()
export class CallService {
  constructor(
    @InjectModel('call')
    private readonly model: Model<Call>,
  ) {}

  async updateAgent(input: UpdateAgentInput): Promise<UpdateAgentResponse> {
    const keys = { PK: `c#${input.CallId}`, SK: `c#${input.CallId}` };
    const args = {
      ...input,
      UpdatedAt: new Date().toISOString(),
    };

    await this.model.update(keys, args, {
      condition: new Condition().where('PK').eq(keys.PK).and().attribute('UpdatedAt').lt(args.UpdatedAt),
      return: 'item'
    });

    return { success: true, message: 'Agent updated successfully' };
  }

  async addIssueDetected(input: AddIssueDetectedInput): Promise<AddIssueDetectedResponse> {
    const keys = { PK: `c#${input.CallId}`, SK: `c#${input.CallId}` };
    const args = {
      ...input,
      UpdatedAt: new Date().toISOString(),
    };

    await this.model.update(keys, args, {
      condition: new Condition().where('PK').eq(keys.PK).and().attribute('UpdatedAt').lt(args.UpdatedAt),
      return: 'item'
    });

    return { success: true, message: 'Issue added successfully' };
  }

  async callCategory(input: CallCategoryInput): Promise<CallCategoryResponse> {
    const keys = { PK: `c#${input.CallId}`, SK: `c#${input.CallId}` };
    const args = {
      ...input,
      UpdatedAt: new Date().toISOString(),
      CallCategories: { action: 'ADD', value: input.Categories },
    };

    await this.model.update(keys, args, {
      condition: new Condition().where('PK').eq(keys.PK).and().attribute('UpdatedAt').lt(args.UpdatedAt),
      return: 'item'
    });

    return { success: true, message: 'Call category updated successfully' };
  }
}
