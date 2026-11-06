# FND-02 Provision local application infrastructure

| Field | Value |
| --- | --- |
| Epic | [EP-01 Platform foundation](../README.md#ep-01-platform-foundation) |
| Type | Task |
| Phase | MVP |
| Direct dependencies | [FND-01](FND-01-initialize-the-next-js-and-nestjs-monorepo.md) |

## Summary

Add a Docker Compose file for Docker Desktop with PostgreSQL using a pgvector-enabled image, Redis for BullMQ, and single-node SeaweedFS with a private S3 bucket and local credentials. Bind published ports to loopback, persist service data in named volumes, and configure Redis with AOF persistence and the `noeviction` policy. Provide example environment settings and connectivity checks for all three services. [Docker Compose](https://docs.docker.com/reference/compose-file/services/) · [pgvector](https://github.com/pgvector/pgvector) · [SeaweedFS](https://github.com/seaweedfs/seaweedfs#docker) · [BullMQ Redis configuration](https://docs.bullmq.io/guide/going-to-production)
