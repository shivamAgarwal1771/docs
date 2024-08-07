import { Resolver, Mutation, Args } from '@nestjs/graphql';
import { CallService } from './call.service';
import { UpdateAgentInput, AddIssueDetectedInput, CallCategoryInput } from './input/call.input';
import { UpdateAgentResponse, AddIssueDetectedResponse, CallCategoryResponse } from './model/call.model';

@Resolver()
export class CallResolver {
  constructor(private readonly callService: CallService) {}

  @Mutation(() => UpdateAgentResponse)
  async updateAgent(@Args('input') input: UpdateAgentInput): Promise<UpdateAgentResponse> {
    return this.callService.updateAgent(input);
  }

  @Mutation(() => AddIssueDetectedResponse)
  async addIssueDetected(@Args('input') input: AddIssueDetectedInput): Promise<AddIssueDetectedResponse> {
    return this.callService.addIssueDetected(input);
  }

  @Mutation(() => CallCategoryResponse)
  async callCategory(@Args('input') input: CallCategoryInput): Promise<CallCategoryResponse> {
    return this.callService.callCategory(input);
  }
}
