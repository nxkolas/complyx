# FND-08 Run jobs in a separate BullMQ worker

| Field | Value |
| --- | --- |
| Epic | [EP-01 Platform foundation](../README.md#ep-01-platform-foundation) |
| Type | Task |
| Phase | MVP |
| Direct dependencies | [FND-02](FND-02-provision-local-application-infrastructure.md), [FND-04](FND-04-establish-nestjs-api-conventions.md) |

## Summary

Use the local Compose Redis service for both BullMQ producers in the NestJS API and consumers in the separate NestJS worker. Set up named queues, bounded retries, job visibility, graceful worker shutdown, and handlers that can safely retry after a restart. Verify that a job queued while the worker is stopped runs when it starts again. [BullMQ connections](https://docs.bullmq.io/guide/connections)
