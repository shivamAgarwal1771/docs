// call.service.ts
import { Injectable } from '@nestjs/common';
import { InjectModel, Model } from 'nestjs-dynamoose';
import { UpdateAgentInput, AddIssueDetectedInput, CallCategoryInput } from './input/call.input';
import { UpdateAgentResponse, AddIssueDetectedResponse, CallCategoryResponse } from './model/call.model';
import { Call } from './model/call.model'; // Import Call model

@Injectable()
export class CallService {
  constructor(
    @InjectModel('call')
    private readonly model: Model<Call>,
  ) {}

  async updateAgent(input: UpdateAgentInput): Promise<UpdateAgentResponse> {
    const PK = `c#${input.CallId}`;
    const UpdatedAt = input.UpdatedAt || new Date().toISOString();

    const updateExpression = {
      UpdateExpression: "SET AgentId = :AgentId, UpdatedAt = :UpdatedAt",
      ExpressionAttributeValues: {
        ":AgentId": input.AgentId,
        ":UpdatedAt": UpdatedAt,
      },
      ConditionExpression: "attribute_exists(PK) AND UpdatedAt < :UpdatedAt",
      ExpressionAttributeNames: {
        "#PK": "PK",
        "#UpdatedAt": "UpdatedAt"
      }
    };

    await this.model.update(
      { PK, SK: PK },
      updateExpression
    );

    return { success: true, message: 'Agent updated successfully' };
  }

  async addIssueDetected(input: AddIssueDetectedInput): Promise<AddIssueDetectedResponse> {
    const PK = `c#${input.CallId}`;
    const UpdatedAt = input.UpdatedAt || new Date().toISOString();

    const updateExpression = {
      UpdateExpression: "SET IssueId = :IssueId, UpdatedAt = :UpdatedAt",
      ExpressionAttributeValues: {
        ":IssueId": input.IssueId,
        ":UpdatedAt": UpdatedAt,
      },
      ConditionExpression: "attribute_exists(PK) AND UpdatedAt < :UpdatedAt",
      ExpressionAttributeNames: {
        "#PK": "PK",
        "#UpdatedAt": "UpdatedAt"
      }
    };

    await this.model.update(
      { PK, SK: PK },
      updateExpression
    );

    return { success: true, message: 'Issue added successfully' };
  }

  async callCategory(input: CallCategoryInput): Promise<CallCategoryResponse> {
    const PK = `c#${input.CallId}`;
    const UpdatedAt = input.UpdatedAt || new Date().toISOString();

    const updateExpression = {
      UpdateExpression: "SET Categories = list_append(if_not_exists(Categories, :EmptyList), :Categories), UpdatedAt = :UpdatedAt",
      ExpressionAttributeValues: {
        ":Categories": input.Categories,
        ":EmptyList": [],
        ":UpdatedAt": UpdatedAt,
      },
      ConditionExpression: "attribute_exists(PK) AND UpdatedAt < :UpdatedAt",
      ExpressionAttributeNames: {
        "#PK": "PK",
        "#UpdatedAt": "UpdatedAt"
      }
    };

    await this.model.update(
      { PK, SK: PK },
      updateExpression
    );

    return { success: true, message: 'Call category updated successfully' };
  }
}
