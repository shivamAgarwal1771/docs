import { Resolver, Mutation, Args, BadRequestException } from '@nestjs/graphql';
import { CallService } from './call.service';
import { UpdateAgentInput, AddIssueDetectedInput, CallCategoryInput } from './input/call.input';
import { UpdateAgentResponse, AddIssueDetectedResponse, CallCategoryResponse } from './model/call.model';
import { Utility } from './utility'; // Ensure Utility is imported
import { ERROR_MESSAGES, LOG_TYPE } from './constants'; // Ensure constants are imported

@Resolver()
export class CallResolver {
  constructor(private readonly callService: CallService) {}

  @Mutation(() => UpdateAgentResponse, { name: 'updateAgent' })
  async updateAgent(@Args('input') input: UpdateAgentInput): Promise<UpdateAgentResponse> {
    const [updateAgentError, updateAgentResponse] = await Utility.parseResponse(this.callService.updateAgent(input));

    if (updateAgentError) {
      Utility.smartAgentLog('updateAgent', ERROR_MESSAGES.UPDATE_AGENT, updateAgentError, LOG_TYPE.ERROR);
      throw new BadRequestException();
    }
    return updateAgentResponse;
  }

  @Mutation(() => AddIssueDetectedResponse, { name: 'addIssueDetected' })
  async addIssueDetected(@Args('input') input: AddIssueDetectedInput): Promise<AddIssueDetectedResponse> {
    const [addIssueDetectedError, addIssueDetectedResponse] = await Utility.parseResponse(this.callService.addIssueDetected(input));

    if (addIssueDetectedError) {
      Utility.smartAgentLog('addIssueDetected', ERROR_MESSAGES.ADD_ISSUE_DETECTED, addIssueDetectedError, LOG_TYPE.ERROR);
      throw new BadRequestException();
    }
    return addIssueDetectedResponse;
  }

  @Mutation(() => CallCategoryResponse, { name: 'callCategory' })
  async callCategory(@Args('input') input: CallCategoryInput): Promise<CallCategoryResponse> {
    const [callCategoryError, callCategoryResponse] = await Utility.parseResponse(this.callService.callCategory(input));

    if (callCategoryError) {
      Utility.smartAgentLog('callCategory', ERROR_MESSAGES.CALL_CATEGORY, callCategoryError, LOG_TYPE.ERROR);
      throw new BadRequestException();
    }
    return callCategoryResponse;
  }
}
