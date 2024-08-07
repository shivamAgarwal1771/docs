import { ObjectType, Field } from '@nestjs/graphql';

@ObjectType()
export class UpdateAgentResponse {
  @Field()
  success: boolean;

  @Field()
  message: string;
}

@ObjectType()
export class AddIssueDetectedResponse {
  @Field()
  success: boolean;

  @Field()
  message: string;
}

@ObjectType()
export class CallCategoryResponse {
  @Field()
  success: boolean;

  @Field()
  message: string;
}
