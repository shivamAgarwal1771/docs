{
  "errors": [
    {
      "message": "Bad Request",
      "locations": [
        {
          "line": 2,
          "column": 3
        }
      ],
      "path": [
        "updateAgent"
      ],
      "extensions": {
        "code": "BAD_REQUEST",
        "stacktrace": [
          "BadRequestException: Bad Request",
          "    at CallResolver.updateAgent (C:\\Users\\Shivam220802\\OneDrive - EXLService.com (I) Pvt. Ltd\\Desktop\\New folder\\smart-agent-microservices\\conversation-microservice\\src\\call\\call.resolver.ts:152:15)",
          "    at process.processTicksAndRejections (node:internal/process/task_queues:95:5)",
          "    at async target (C:\\Users\\Shivam220802\\OneDrive - EXLService.com (I) Pvt. Ltd\\Desktop\\New folder\\smart-agent-microservices\\conversation-microservice\\node_modules\\@nestjs\\core\\helpers\\external-context-creator.js:74:28)",
          "    at async Object.updateAgent (C:\\Users\\Shivam220802\\OneDrive - EXLService.com (I) Pvt. Ltd\\Desktop\\New folder\\smart-agent-microservices\\conversation-microservice\\node_modules\\@nestjs\\core\\helpers\\external-proxy.js:9:24)"
        ],
        "originalError": {
          "message": "Bad Request",
          "statusCode": 400
        }
      }
    }
  ],
  "data": null
}
