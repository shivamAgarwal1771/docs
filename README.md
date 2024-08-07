  @Mutation(() => Call, { name: 'updateCallBotCustomSettings' })
  async updateCallBotCustomSettings(@Args('input') input: UpdateCallBotCustomSettingsInput): Promise<Call> {
    const [updateCallBotCustomSettingsError, updateCallBotCustomSettingsResponse] = await Utility.parseResponse(this.callService.updateCallBotCustomSettings(input));

    if (updateCallBotCustomSettingsError) {
      Utility.smartAgentLog('updateCallBotCustomSettings', ERROR_MESSAGES.UPDATE_CALL_BOT_CUSTOM_SETTINGS, updateCallBotCustomSettingsError, LOG_TYPE.ERROR)
      throw new BadRequestException();
    }
    return updateCallBotCustomSettingsResponse;
  }
