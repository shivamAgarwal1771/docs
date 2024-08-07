import { ObjectType, Field } from '@nestjs/graphql';

@ObjectType()
export class UpdateAgentResponse {
  @Field()
  success: boolean;

  @Field()
  message: string;

  // Add other fields as necessary
}

@ObjectType()
export class AddIssueDetectedResponse {
  @Field()
  success: boolean;

  @Field()
  message: string;

  // Add other fields as necessary
}

@ObjectType()
export class CallCategoryResponse {
  @Field()
  success: boolean;

  @Field()
  message: string;

  // Add other fields as necessary
}



