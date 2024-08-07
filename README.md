mutation {
  updateAgent(input: {
    CallId: "some-call-id",
    UpdatedAt: "2024-08-07T00:00:00Z",
    // Add other fields as needed based on your UpdateAgentInput type
  }) {
    success
    message
  }
}
