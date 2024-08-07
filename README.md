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
