# FND-05 Authenticate users and resolve their identity

| Field | Value |
| --- | --- |
| Epic | [EP-01 Platform foundation](../README.md#ep-01-platform-foundation) |
| Type | Story |
| Phase | MVP |
| Direct dependencies | [FND-03](FND-03-set-up-database-migrations-and-seeding.md), [FND-04](FND-04-establish-nestjs-api-conventions.md) |

## Summary

Run Better Auth in the NestJS API with PostgreSQL-backed sessions. Connect the Next.js sign-in UI to the API using session cookies, configure the local web origin for trusted credentialed requests, and resolve the authenticated user on protected API routes. [Better Auth NestJS integration](https://better-auth.com/docs/integrations/nestjs)
