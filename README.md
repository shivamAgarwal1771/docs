@ObjectType()
@InputType('CallCategoryInputType')
export class CallCategoryInput {
  @Field()
  CallId: String;

  @Field()
  CallCategories:String;

  @Field({ nullable: true })
  UpdatedAt?: String;
}
input.ts

  async callCategory(input: CallCategoryInput){
    const keys = { PK: `c#${input.CallId}`, SK: `c#${input.CallId}` };
    const args = {
      CallCategories:[input.CallCategories],
      UpdatedAt: new Date().toISOString(),
    };

    return this.model.update(keys, args, {
      condition: new Condition().where('PK').eq(keys.PK).and().attribute('UpdatedAt').lt(args.UpdatedAt),
      return: 'item'
    });
  }

service.ts
