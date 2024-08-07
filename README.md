import { InputType, Field } from '@nestjs/graphql';

@InputType()
export class UpdateAgentInput {
  @Field()
  CallId: string;

  @Field({ nullable: true })
  UpdatedAt?: string;

  // Add other fields as necessary
}

@InputType()
export class AddIssueDetectedInput {
  @Field()
  CallId: string;

  @Field({ nullable: true })
  UpdatedAt?: string;

  // Add other fields as necessary
}

@InputType()
export class CallCategoryInput {
  @Field()
  CallId: string;

  @Field({ nullable: true })
  UpdatedAt?: string;

  // Add other fields as necessary
}
