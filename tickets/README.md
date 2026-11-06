# ComplyX implementation backlog

The ComplyX MVP uses Next.js for the frontend, NestJS for the API and worker, Better Auth for sign-in and sessions, PostgreSQL with pgvector, Redis with BullMQ, and S3-compatible object storage. NIS2 is the first framework. Reuse the old app’s applicability logic and start the rest of the application from scratch.

The backlog contains 83 ticket summaries across 10 epics. 73 are in MVP scope and 10 are post-MVP. Each ticket has a type, a short scope, and direct dependencies. Add acceptance criteria and implementation subtasks when a ticket is selected for development.

## Local development

| Runs in | Component | Local address and purpose |
| --- | --- | --- |
| Docker Desktop via Compose | PostgreSQL with pgvector | `127.0.0.1:5432` for application data, Better Auth records, and document embeddings. |
| Docker Desktop via Compose | SeaweedFS in single-node mode | `http://127.0.0.1:8333` for a private development bucket accessed over S3. |
| Docker Desktop via Compose | Redis | `redis://127.0.0.1:6379` for BullMQ job state. The NestJS API and worker share this connection setting. |
| Mac | Next.js web app | `http://localhost:3000` via `next dev`. |
| Mac | NestJS API | `http://localhost:3001` via `nest start --watch`. Better Auth runs in this process. |
| Mac | NestJS worker | A separate `nest start --watch` process that consumes BullMQ jobs. |

Once the foundation tickets land, open Docker Desktop, start the three services with `docker compose up -d`, run database migrations, and launch the web app, API, and worker with the repository’s dev scripts.

When a document is uploaded, the API stores the file in SeaweedFS, records its metadata in PostgreSQL, and queues extraction with BullMQ. Redis holds the job while the worker is stopped. The worker picks it up when it starts and records processing status in PostgreSQL so the UI can show progress. [BullMQ connections](https://docs.bullmq.io/guide/connections)

## First implementation pass

| Step | Work to take first |
| --- | --- |
| 1 | Set up the workspace, local services, database and API with [FND-01](01-platform-foundation/FND-01-initialize-the-next-js-and-nestjs-monorepo.md), [FND-02](01-platform-foundation/FND-02-provision-local-application-infrastructure.md), [FND-03](01-platform-foundation/FND-03-set-up-database-migrations-and-seeding.md), [FND-04](01-platform-foundation/FND-04-establish-nestjs-api-conventions.md). Start [APP-01](03-applicability-assessment/APP-01-extract-the-existing-nis2-applicability-logic.md) in parallel once the old app is available. |
| 2 | Complete identity, tenant access and worker setup through [FND-05](01-platform-foundation/FND-05-authenticate-users-and-resolve-their-identity.md), [FND-06](01-platform-foundation/FND-06-manage-organisations-memberships-and-roles.md), [FND-07](01-platform-foundation/FND-07-enforce-tenant-isolation-across-application-boundaries.md), [FND-08](01-platform-foundation/FND-08-run-jobs-in-a-separate-bullmq-worker.md). Publish one reviewed NIS2 requirement under [KB-07](02-compliance-knowledge-base/KB-07-publish-the-initial-nis2-framework-pack.md) and port the applicability checker under [APP-03](03-applicability-assessment/APP-03-port-the-legacy-checker-into-the-nestjs-domain.md). |
| 3 | Follow the linked prerequisites for [QST-05](06-dynamic-assessment-questionnaire/QST-05-suggest-answers-from-cited-evidence.md) and [QST-06](06-dynamic-assessment-questionnaire/QST-06-review-correct-and-confirm-assessment-answers.md) to assess that requirement using one model and one supported document. Confirm an answer and open its cited source passage. |
| 4 | Follow the prerequisites for [ACT-05](08-remediation-and-action-planning/ACT-05-review-approve-and-assign-remediation-actions.md) to review the resulting finding and approve a linked action. Expand the framework pack and complete the remaining MVP tickets once this flow works. |

APP-01 requires access to the old app’s source. The legacy rules and reference cases are inputs to the migration.

## Epics

| Epic | Capability | MVP | Post-MVP |
| --- | --- | ---: | ---: |
| EP-01 | [Platform foundation](#ep-01-platform-foundation) | 9 | 0 |
| EP-02 | [Compliance knowledge base](#ep-02-compliance-knowledge-base) | 7 | 1 |
| EP-03 | [Applicability assessment](#ep-03-applicability-assessment) | 7 | 0 |
| EP-04 | [AI and model management](#ep-04-ai-and-model-management) | 9 | 0 |
| EP-05 | [Document and evidence management](#ep-05-document-and-evidence-management) | 10 | 0 |
| EP-06 | [Dynamic assessment questionnaire](#ep-06-dynamic-assessment-questionnaire) | 10 | 0 |
| EP-07 | [Gap analysis](#ep-07-gap-analysis) | 8 | 0 |
| EP-08 | [Remediation and action planning](#ep-08-remediation-and-action-planning) | 8 | 0 |
| EP-09 | [Dashboard and reporting](#ep-09-dashboard-and-reporting) | 1 | 5 |
| EP-10 | [Production hardening and framework expansion](#ep-10-production-hardening-and-framework-expansion) | 4 | 4 |

## EP-01 Platform foundation

| Ticket | Title | Phase | Direct dependencies |
| --- | --- | --- | --- |
| [FND-01](01-platform-foundation/FND-01-initialize-the-next-js-and-nestjs-monorepo.md) | Initialize the Next.js and NestJS monorepo | MVP | None |
| [FND-02](01-platform-foundation/FND-02-provision-local-application-infrastructure.md) | Provision local application infrastructure | MVP | [FND-01](01-platform-foundation/FND-01-initialize-the-next-js-and-nestjs-monorepo.md) |
| [FND-03](01-platform-foundation/FND-03-set-up-database-migrations-and-seeding.md) | Set up database migrations and seeding | MVP | [FND-02](01-platform-foundation/FND-02-provision-local-application-infrastructure.md) |
| [FND-04](01-platform-foundation/FND-04-establish-nestjs-api-conventions.md) | Establish NestJS API conventions | MVP | [FND-01](01-platform-foundation/FND-01-initialize-the-next-js-and-nestjs-monorepo.md) |
| [FND-05](01-platform-foundation/FND-05-authenticate-users-and-resolve-their-identity.md) | Authenticate users and resolve their identity | MVP | [FND-03](01-platform-foundation/FND-03-set-up-database-migrations-and-seeding.md), [FND-04](01-platform-foundation/FND-04-establish-nestjs-api-conventions.md) |
| [FND-06](01-platform-foundation/FND-06-manage-organisations-memberships-and-roles.md) | Manage organisations, memberships, and roles | MVP | [FND-05](01-platform-foundation/FND-05-authenticate-users-and-resolve-their-identity.md) |
| [FND-07](01-platform-foundation/FND-07-enforce-tenant-isolation-across-application-boundaries.md) | Enforce tenant isolation across application boundaries | MVP | [FND-06](01-platform-foundation/FND-06-manage-organisations-memberships-and-roles.md) |
| [FND-08](01-platform-foundation/FND-08-run-jobs-in-a-separate-bullmq-worker.md) | Run jobs in a separate BullMQ worker | MVP | [FND-02](01-platform-foundation/FND-02-provision-local-application-infrastructure.md), [FND-04](01-platform-foundation/FND-04-establish-nestjs-api-conventions.md) |
| [FND-09](01-platform-foundation/FND-09-set-up-automated-checks-and-development-logging.md) | Set up automated checks and development logging | MVP | [FND-01](01-platform-foundation/FND-01-initialize-the-next-js-and-nestjs-monorepo.md) |

## EP-02 Compliance knowledge base

| Ticket | Title | Phase | Direct dependencies |
| --- | --- | --- | --- |
| [KB-01](02-compliance-knowledge-base/KB-01-version-compliance-framework-packs.md) | Version compliance framework packs | MVP | [FND-03](01-platform-foundation/FND-03-set-up-database-migrations-and-seeding.md) |
| [KB-02](02-compliance-knowledge-base/KB-02-register-regulatory-sources-and-revisions.md) | Register regulatory sources and revisions | MVP | [KB-01](02-compliance-knowledge-base/KB-01-version-compliance-framework-packs.md) |
| [KB-03](02-compliance-knowledge-base/KB-03-define-requirements-and-assessment-criteria.md) | Define requirements and assessment criteria | MVP | [KB-01](02-compliance-knowledge-base/KB-01-version-compliance-framework-packs.md) |
| [KB-04](02-compliance-knowledge-base/KB-04-link-requirements-to-precise-source-references.md) | Link requirements to precise source references | MVP | [KB-02](02-compliance-knowledge-base/KB-02-register-regulatory-sources-and-revisions.md), [KB-03](02-compliance-knowledge-base/KB-03-define-requirements-and-assessment-criteria.md) |
| [KB-05](02-compliance-knowledge-base/KB-05-evaluate-requirement-applicability-rules.md) | Evaluate requirement applicability rules | MVP | [KB-03](02-compliance-knowledge-base/KB-03-define-requirements-and-assessment-criteria.md) |
| [KB-06](02-compliance-knowledge-base/KB-06-import-validate-and-publish-framework-packs.md) | Import, validate, and publish framework packs | MVP | [FND-07](01-platform-foundation/FND-07-enforce-tenant-isolation-across-application-boundaries.md), [KB-04](02-compliance-knowledge-base/KB-04-link-requirements-to-precise-source-references.md), [KB-05](02-compliance-knowledge-base/KB-05-evaluate-requirement-applicability-rules.md) |
| [KB-07](02-compliance-knowledge-base/KB-07-publish-the-initial-nis2-framework-pack.md) | Publish the initial NIS2 framework pack | MVP | [KB-06](02-compliance-knowledge-base/KB-06-import-validate-and-publish-framework-packs.md) |
| [KB-08](02-compliance-knowledge-base/KB-08-build-a-framework-curation-interface.md) | Build a framework curation interface | Post-MVP | [KB-06](02-compliance-knowledge-base/KB-06-import-validate-and-publish-framework-packs.md) |

## EP-03 Applicability assessment

| Ticket | Title | Phase | Direct dependencies |
| --- | --- | --- | --- |
| [APP-01](03-applicability-assessment/APP-01-extract-the-existing-nis2-applicability-logic.md) | Extract the existing NIS2 applicability logic | MVP | None |
| [APP-02](03-applicability-assessment/APP-02-capture-the-organisation-applicability-profile.md) | Capture the organisation applicability profile | MVP | [FND-07](01-platform-foundation/FND-07-enforce-tenant-isolation-across-application-boundaries.md) |
| [APP-03](03-applicability-assessment/APP-03-port-the-legacy-checker-into-the-nestjs-domain.md) | Port the legacy checker into the NestJS domain | MVP | [APP-01](03-applicability-assessment/APP-01-extract-the-existing-nis2-applicability-logic.md), [FND-04](01-platform-foundation/FND-04-establish-nestjs-api-conventions.md) |
| [APP-04](03-applicability-assessment/APP-04-run-and-persist-an-applicability-check.md) | Run and persist an applicability check | MVP | [APP-02](03-applicability-assessment/APP-02-capture-the-organisation-applicability-profile.md), [APP-03](03-applicability-assessment/APP-03-port-the-legacy-checker-into-the-nestjs-domain.md) |
| [APP-05](03-applicability-assessment/APP-05-map-applicability-results-to-requirement-scope.md) | Map applicability results to requirement scope | MVP | [APP-04](03-applicability-assessment/APP-04-run-and-persist-an-applicability-check.md), [KB-05](02-compliance-knowledge-base/KB-05-evaluate-requirement-applicability-rules.md) |
| [APP-06](03-applicability-assessment/APP-06-build-the-applicability-questionnaire-and-result-page.md) | Build the applicability questionnaire and result page | MVP | [APP-04](03-applicability-assessment/APP-04-run-and-persist-an-applicability-check.md) |
| [APP-07](03-applicability-assessment/APP-07-version-applicability-results-and-support-reassessment.md) | Version applicability results and support reassessment | MVP | [APP-05](03-applicability-assessment/APP-05-map-applicability-results-to-requirement-scope.md) |

## EP-04 AI and model management

| Ticket | Title | Phase | Direct dependencies |
| --- | --- | --- | --- |
| [AI-01](04-ai-and-model-management/AI-01-define-a-provider-independent-compliance-ai-service.md) | Define a provider-independent compliance AI service | MVP | [FND-04](01-platform-foundation/FND-04-establish-nestjs-api-conventions.md) |
| [AI-02](04-ai-and-model-management/AI-02-integrate-cloud-generation-providers.md) | Integrate cloud generation providers | MVP | [AI-01](04-ai-and-model-management/AI-01-define-a-provider-independent-compliance-ai-service.md) |
| [AI-03](04-ai-and-model-management/AI-03-integrate-ollama-and-private-model-endpoints.md) | Integrate Ollama and private model endpoints | MVP | [AI-01](04-ai-and-model-management/AI-01-define-a-provider-independent-compliance-ai-service.md), [AI-09](04-ai-and-model-management/AI-09-enforce-model-routing-and-privacy-policies.md) |
| [AI-04](04-ai-and-model-management/AI-04-manage-organisation-model-configurations-and-switching.md) | Manage organisation model configurations and switching | MVP | [FND-07](01-platform-foundation/FND-07-enforce-tenant-isolation-across-application-boundaries.md), [AI-01](04-ai-and-model-management/AI-01-define-a-provider-independent-compliance-ai-service.md), [AI-05](04-ai-and-model-management/AI-05-validate-model-capabilities-before-use.md), [AI-09](04-ai-and-model-management/AI-09-enforce-model-routing-and-privacy-policies.md) |
| [AI-05](04-ai-and-model-management/AI-05-validate-model-capabilities-before-use.md) | Validate model capabilities before use | MVP | [AI-02](04-ai-and-model-management/AI-02-integrate-cloud-generation-providers.md) |
| [AI-06](04-ai-and-model-management/AI-06-version-prompts-for-each-ai-operation.md) | Version prompts for each AI operation | MVP | [AI-01](04-ai-and-model-management/AI-01-define-a-provider-independent-compliance-ai-service.md), [FND-03](01-platform-foundation/FND-03-set-up-database-migrations-and-seeding.md) |
| [AI-07](04-ai-and-model-management/AI-07-validate-structured-outputs-and-handle-retries.md) | Validate structured outputs and handle retries | MVP | [AI-01](04-ai-and-model-management/AI-01-define-a-provider-independent-compliance-ai-service.md) |
| [AI-08](04-ai-and-model-management/AI-08-record-ai-execution-provenance-and-usage.md) | Record AI execution provenance and usage | MVP | [FND-07](01-platform-foundation/FND-07-enforce-tenant-isolation-across-application-boundaries.md), [AI-06](04-ai-and-model-management/AI-06-version-prompts-for-each-ai-operation.md), [AI-07](04-ai-and-model-management/AI-07-validate-structured-outputs-and-handle-retries.md) |
| [AI-09](04-ai-and-model-management/AI-09-enforce-model-routing-and-privacy-policies.md) | Enforce model routing and privacy policies | MVP | [FND-07](01-platform-foundation/FND-07-enforce-tenant-isolation-across-application-boundaries.md), [AI-01](04-ai-and-model-management/AI-01-define-a-provider-independent-compliance-ai-service.md) |

## EP-05 Document and evidence management

| Ticket | Title | Phase | Direct dependencies |
| --- | --- | --- | --- |
| [DOC-01](05-document-and-evidence-management/DOC-01-upload-documents-to-private-storage.md) | Upload documents to private storage | MVP | [FND-07](01-platform-foundation/FND-07-enforce-tenant-isolation-across-application-boundaries.md), [FND-02](01-platform-foundation/FND-02-provision-local-application-infrastructure.md) |
| [DOC-02](05-document-and-evidence-management/DOC-02-track-immutable-document-versions-and-access.md) | Track immutable document versions and access | MVP | [DOC-01](05-document-and-evidence-management/DOC-01-upload-documents-to-private-storage.md) |
| [DOC-03](05-document-and-evidence-management/DOC-03-extract-document-text-in-a-restricted-worker.md) | Extract document text in a restricted worker | MVP | [DOC-02](05-document-and-evidence-management/DOC-02-track-immutable-document-versions-and-access.md), [FND-08](01-platform-foundation/FND-08-run-jobs-in-a-separate-bullmq-worker.md) |
| [DOC-04](05-document-and-evidence-management/DOC-04-anchor-extracted-passages-to-document-versions.md) | Anchor extracted passages to document versions | MVP | [DOC-03](05-document-and-evidence-management/DOC-03-extract-document-text-in-a-restricted-worker.md) |
| [DOC-05](05-document-and-evidence-management/DOC-05-index-passages-with-a-dedicated-embedding-model.md) | Index passages with a dedicated embedding model | MVP | [DOC-04](05-document-and-evidence-management/DOC-04-anchor-extracted-passages-to-document-versions.md), [AI-09](04-ai-and-model-management/AI-09-enforce-model-routing-and-privacy-policies.md) |
| [DOC-06](05-document-and-evidence-management/DOC-06-retrieve-evidence-through-tenant-scoped-hybrid-search.md) | Retrieve evidence through tenant-scoped hybrid search | MVP | [DOC-05](05-document-and-evidence-management/DOC-05-index-passages-with-a-dedicated-embedding-model.md) |
| [DOC-07](05-document-and-evidence-management/DOC-07-extract-candidate-evidence-claims-from-documents.md) | Extract candidate evidence claims from documents | MVP | [DOC-06](05-document-and-evidence-management/DOC-06-retrieve-evidence-through-tenant-scoped-hybrid-search.md), [AI-07](04-ai-and-model-management/AI-07-validate-structured-outputs-and-handle-retries.md), [AI-08](04-ai-and-model-management/AI-08-record-ai-execution-provenance-and-usage.md) |
| [DOC-08](05-document-and-evidence-management/DOC-08-link-evidence-claims-to-assessment-criteria.md) | Link evidence claims to assessment criteria | MVP | [DOC-07](05-document-and-evidence-management/DOC-07-extract-candidate-evidence-claims-from-documents.md), [KB-03](02-compliance-knowledge-base/KB-03-define-requirements-and-assessment-criteria.md) |
| [DOC-09](05-document-and-evidence-management/DOC-09-resolve-and-validate-evidence-and-regulatory-citations.md) | Resolve and validate evidence and regulatory citations | MVP | [DOC-04](05-document-and-evidence-management/DOC-04-anchor-extracted-passages-to-document-versions.md), [KB-04](02-compliance-knowledge-base/KB-04-link-requirements-to-precise-source-references.md) |
| [DOC-10](05-document-and-evidence-management/DOC-10-show-document-processing-status-and-cited-passages.md) | Show document processing status and cited passages | MVP | [DOC-03](05-document-and-evidence-management/DOC-03-extract-document-text-in-a-restricted-worker.md), [DOC-09](05-document-and-evidence-management/DOC-09-resolve-and-validate-evidence-and-regulatory-citations.md) |

## EP-06 Dynamic assessment questionnaire

| Ticket | Title | Phase | Direct dependencies |
| --- | --- | --- | --- |
| [QST-01](06-dynamic-assessment-questionnaire/QST-01-create-versioned-assessment-sessions.md) | Create versioned assessment sessions | MVP | [APP-05](03-applicability-assessment/APP-05-map-applicability-results-to-requirement-scope.md), [KB-06](02-compliance-knowledge-base/KB-06-import-validate-and-publish-framework-packs.md) |
| [QST-02](06-dynamic-assessment-questionnaire/QST-02-track-coverage-at-the-criterion-level.md) | Track coverage at the criterion level | MVP | [QST-01](06-dynamic-assessment-questionnaire/QST-01-create-versioned-assessment-sessions.md) |
| [QST-03](06-dynamic-assessment-questionnaire/QST-03-select-the-next-unresolved-assessment-criterion.md) | Select the next unresolved assessment criterion | MVP | [QST-02](06-dynamic-assessment-questionnaire/QST-02-track-coverage-at-the-criterion-level.md) |
| [QST-04](06-dynamic-assessment-questionnaire/QST-04-generate-questions-for-the-selected-criteria.md) | Generate questions for the selected criteria | MVP | [QST-03](06-dynamic-assessment-questionnaire/QST-03-select-the-next-unresolved-assessment-criterion.md), [AI-04](04-ai-and-model-management/AI-04-manage-organisation-model-configurations-and-switching.md), [AI-08](04-ai-and-model-management/AI-08-record-ai-execution-provenance-and-usage.md) |
| [QST-05](06-dynamic-assessment-questionnaire/QST-05-suggest-answers-from-cited-evidence.md) | Suggest answers from cited evidence | MVP | [QST-04](06-dynamic-assessment-questionnaire/QST-04-generate-questions-for-the-selected-criteria.md), [DOC-08](05-document-and-evidence-management/DOC-08-link-evidence-claims-to-assessment-criteria.md), [DOC-09](05-document-and-evidence-management/DOC-09-resolve-and-validate-evidence-and-regulatory-citations.md) |
| [QST-06](06-dynamic-assessment-questionnaire/QST-06-review-correct-and-confirm-assessment-answers.md) | Review, correct, and confirm assessment answers | MVP | [QST-04](06-dynamic-assessment-questionnaire/QST-04-generate-questions-for-the-selected-criteria.md) |
| [QST-07](06-dynamic-assessment-questionnaire/QST-07-ask-targeted-follow-up-questions.md) | Ask targeted follow-up questions | MVP | [QST-05](06-dynamic-assessment-questionnaire/QST-05-suggest-answers-from-cited-evidence.md), [QST-06](06-dynamic-assessment-questionnaire/QST-06-review-correct-and-confirm-assessment-answers.md) |
| [QST-08](06-dynamic-assessment-questionnaire/QST-08-persist-questionnaire-progress-and-resume-sessions.md) | Persist questionnaire progress and resume sessions | MVP | [QST-06](06-dynamic-assessment-questionnaire/QST-06-review-correct-and-confirm-assessment-answers.md) |
| [QST-09](06-dynamic-assessment-questionnaire/QST-09-build-the-assessment-questionnaire-interface.md) | Build the assessment questionnaire interface | MVP | [QST-08](06-dynamic-assessment-questionnaire/QST-08-persist-questionnaire-progress-and-resume-sessions.md) |
| [QST-10](06-dynamic-assessment-questionnaire/QST-10-finish-questionnaires-with-explicit-unresolved-items.md) | Finish questionnaires with explicit unresolved items | MVP | [QST-07](06-dynamic-assessment-questionnaire/QST-07-ask-targeted-follow-up-questions.md), [QST-08](06-dynamic-assessment-questionnaire/QST-08-persist-questionnaire-progress-and-resume-sessions.md) |

## EP-07 Gap analysis

| Ticket | Title | Phase | Direct dependencies |
| --- | --- | --- | --- |
| [GAP-01](07-gap-analysis/GAP-01-define-findings-and-their-review-lifecycle.md) | Define findings and their review lifecycle | MVP | [QST-02](06-dynamic-assessment-questionnaire/QST-02-track-coverage-at-the-criterion-level.md) |
| [GAP-02](07-gap-analysis/GAP-02-evaluate-evidence-sufficiency-and-contradictions.md) | Evaluate evidence sufficiency and contradictions | MVP | [GAP-01](07-gap-analysis/GAP-01-define-findings-and-their-review-lifecycle.md), [DOC-08](05-document-and-evidence-management/DOC-08-link-evidence-claims-to-assessment-criteria.md), [QST-06](06-dynamic-assessment-questionnaire/QST-06-review-correct-and-confirm-assessment-answers.md) |
| [GAP-03](07-gap-analysis/GAP-03-generate-proposed-requirement-findings.md) | Generate proposed requirement findings | MVP | [GAP-02](07-gap-analysis/GAP-02-evaluate-evidence-sufficiency-and-contradictions.md), [AI-07](04-ai-and-model-management/AI-07-validate-structured-outputs-and-handle-retries.md), [AI-08](04-ai-and-model-management/AI-08-record-ai-execution-provenance-and-usage.md) |
| [GAP-04](07-gap-analysis/GAP-04-validate-proposed-findings-and-citations.md) | Validate proposed findings and citations | MVP | [GAP-03](07-gap-analysis/GAP-03-generate-proposed-requirement-findings.md), [DOC-09](05-document-and-evidence-management/DOC-09-resolve-and-validate-evidence-and-regulatory-citations.md) |
| [GAP-05](07-gap-analysis/GAP-05-review-and-approve-findings.md) | Review and approve findings | MVP | [GAP-04](07-gap-analysis/GAP-04-validate-proposed-findings-and-citations.md), [GAP-07](07-gap-analysis/GAP-07-record-finding-history-and-evaluation-inputs.md) |
| [GAP-06](07-gap-analysis/GAP-06-reassess-findings-affected-by-changed-inputs.md) | Reassess findings affected by changed inputs | MVP | [GAP-05](07-gap-analysis/GAP-05-review-and-approve-findings.md), [GAP-07](07-gap-analysis/GAP-07-record-finding-history-and-evaluation-inputs.md), [FND-08](01-platform-foundation/FND-08-run-jobs-in-a-separate-bullmq-worker.md) |
| [GAP-07](07-gap-analysis/GAP-07-record-finding-history-and-evaluation-inputs.md) | Record finding history and evaluation inputs | MVP | [GAP-03](07-gap-analysis/GAP-03-generate-proposed-requirement-findings.md) |
| [GAP-08](07-gap-analysis/GAP-08-show-requirement-level-readiness-findings.md) | Show requirement-level readiness findings | MVP | [GAP-05](07-gap-analysis/GAP-05-review-and-approve-findings.md), [GAP-07](07-gap-analysis/GAP-07-record-finding-history-and-evaluation-inputs.md) |

## EP-08 Remediation and action planning

| Ticket | Title | Phase | Direct dependencies |
| --- | --- | --- | --- |
| [ACT-01](08-remediation-and-action-planning/ACT-01-model-remediation-plans-and-linked-actions.md) | Model remediation plans and linked actions | MVP | [GAP-01](07-gap-analysis/GAP-01-define-findings-and-their-review-lifecycle.md) |
| [ACT-02](08-remediation-and-action-planning/ACT-02-draft-remediation-actions-from-approved-findings.md) | Draft remediation actions from approved findings | MVP | [ACT-01](08-remediation-and-action-planning/ACT-01-model-remediation-plans-and-linked-actions.md), [GAP-05](07-gap-analysis/GAP-05-review-and-approve-findings.md), [AI-07](04-ai-and-model-management/AI-07-validate-structured-outputs-and-handle-retries.md), [AI-08](04-ai-and-model-management/AI-08-record-ai-execution-provenance-and-usage.md) |
| [ACT-03](08-remediation-and-action-planning/ACT-03-prioritise-actions-and-record-dependencies.md) | Prioritise actions and record dependencies | MVP | [ACT-02](08-remediation-and-action-planning/ACT-02-draft-remediation-actions-from-approved-findings.md) |
| [ACT-04](08-remediation-and-action-planning/ACT-04-deduplicate-actions-and-preserve-requirement-links.md) | Deduplicate actions and preserve requirement links | MVP | [ACT-02](08-remediation-and-action-planning/ACT-02-draft-remediation-actions-from-approved-findings.md) |
| [ACT-05](08-remediation-and-action-planning/ACT-05-review-approve-and-assign-remediation-actions.md) | Review, approve, and assign remediation actions | MVP | [ACT-04](08-remediation-and-action-planning/ACT-04-deduplicate-actions-and-preserve-requirement-links.md), [FND-06](01-platform-foundation/FND-06-manage-organisations-memberships-and-roles.md) |
| [ACT-06](08-remediation-and-action-planning/ACT-06-track-remediation-progress-and-due-dates.md) | Track remediation progress and due dates | MVP | [ACT-05](08-remediation-and-action-planning/ACT-05-review-approve-and-assign-remediation-actions.md), [ACT-03](08-remediation-and-action-planning/ACT-03-prioritise-actions-and-record-dependencies.md) |
| [ACT-07](08-remediation-and-action-planning/ACT-07-verify-completion-with-implementation-evidence.md) | Verify completion with implementation evidence | MVP | [ACT-06](08-remediation-and-action-planning/ACT-06-track-remediation-progress-and-due-dates.md), [GAP-06](07-gap-analysis/GAP-06-reassess-findings-affected-by-changed-inputs.md) |
| [ACT-08](08-remediation-and-action-planning/ACT-08-build-the-remediation-plan-interface.md) | Build the remediation plan interface | MVP | [ACT-06](08-remediation-and-action-planning/ACT-06-track-remediation-progress-and-due-dates.md) |

## EP-09 Dashboard and reporting

| Ticket | Title | Phase | Direct dependencies |
| --- | --- | --- | --- |
| [RPT-01](09-dashboard-and-reporting/RPT-01-show-an-organisation-readiness-summary.md) | Show an organisation readiness summary | MVP | [GAP-08](07-gap-analysis/GAP-08-show-requirement-level-readiness-findings.md), [ACT-08](08-remediation-and-action-planning/ACT-08-build-the-remediation-plan-interface.md) |
| [RPT-02](09-dashboard-and-reporting/RPT-02-summarise-framework-and-criterion-coverage.md) | Summarise framework and criterion coverage | Post-MVP | [RPT-01](09-dashboard-and-reporting/RPT-01-show-an-organisation-readiness-summary.md), [QST-02](06-dynamic-assessment-questionnaire/QST-02-track-coverage-at-the-criterion-level.md) |
| [RPT-03](09-dashboard-and-reporting/RPT-03-show-risk-gap-and-remediation-trends.md) | Show risk, gap, and remediation trends | Post-MVP | [RPT-01](09-dashboard-and-reporting/RPT-01-show-an-organisation-readiness-summary.md), [ACT-06](08-remediation-and-action-planning/ACT-06-track-remediation-progress-and-due-dates.md) |
| [RPT-04](09-dashboard-and-reporting/RPT-04-generate-an-evidence-backed-assessment-report.md) | Generate an evidence-backed assessment report | Post-MVP | [GAP-07](07-gap-analysis/GAP-07-record-finding-history-and-evaluation-inputs.md), [ACT-06](08-remediation-and-action-planning/ACT-06-track-remediation-progress-and-due-dates.md) |
| [RPT-05](09-dashboard-and-reporting/RPT-05-export-assessment-reports-as-pdf-and-structured-data.md) | Export assessment reports as PDF and structured data | Post-MVP | [RPT-04](09-dashboard-and-reporting/RPT-04-generate-an-evidence-backed-assessment-report.md) |
| [RPT-06](09-dashboard-and-reporting/RPT-06-compare-historical-assessments.md) | Compare historical assessments | Post-MVP | [RPT-04](09-dashboard-and-reporting/RPT-04-generate-an-evidence-backed-assessment-report.md), [APP-07](03-applicability-assessment/APP-07-version-applicability-results-and-support-reassessment.md) |

## EP-10 Production hardening and framework expansion

| Ticket | Title | Phase | Direct dependencies |
| --- | --- | --- | --- |
| [EXT-01](10-production-hardening-and-framework-expansion/EXT-01-complete-audit-logging-and-retention-controls.md) | Complete audit logging and retention controls | MVP | [FND-07](01-platform-foundation/FND-07-enforce-tenant-isolation-across-application-boundaries.md), [AI-08](04-ai-and-model-management/AI-08-record-ai-execution-provenance-and-usage.md), [ACT-05](08-remediation-and-action-planning/ACT-05-review-approve-and-assign-remediation-actions.md) |
| [EXT-02](10-production-hardening-and-framework-expansion/EXT-02-test-prompt-injection-and-document-security.md) | Test prompt injection and document security | MVP | [DOC-03](05-document-and-evidence-management/DOC-03-extract-document-text-in-a-restricted-worker.md), [GAP-04](07-gap-analysis/GAP-04-validate-proposed-findings-and-citations.md) |
| [EXT-03](10-production-hardening-and-framework-expansion/EXT-03-run-model-regression-checks-against-known-cases.md) | Run model regression checks against known cases | MVP | [GAP-04](07-gap-analysis/GAP-04-validate-proposed-findings-and-citations.md), [AI-05](04-ai-and-model-management/AI-05-validate-model-capabilities-before-use.md) |
| [EXT-04](10-production-hardening-and-framework-expansion/EXT-04-map-requirements-across-frameworks.md) | Map requirements across frameworks | Post-MVP | [KB-06](02-compliance-knowledge-base/KB-06-import-validate-and-publish-framework-packs.md) |
| [EXT-05](10-production-hardening-and-framework-expansion/EXT-05-reuse-evidence-across-framework-assessments.md) | Reuse evidence across framework assessments | Post-MVP | [EXT-04](10-production-hardening-and-framework-expansion/EXT-04-map-requirements-across-frameworks.md), [DOC-08](05-document-and-evidence-management/DOC-08-link-evidence-claims-to-assessment-criteria.md) |
| [EXT-06](10-production-hardening-and-framework-expansion/EXT-06-detect-regulatory-source-changes-and-assess-their-impact.md) | Detect regulatory source changes and assess their impact | Post-MVP | [KB-06](02-compliance-knowledge-base/KB-06-import-validate-and-publish-framework-packs.md), [APP-07](03-applicability-assessment/APP-07-version-applicability-results-and-support-reassessment.md) |
| [EXT-07](10-production-hardening-and-framework-expansion/EXT-07-verify-production-monitoring-and-resilience.md) | Verify production monitoring and resilience | MVP | [FND-08](01-platform-foundation/FND-08-run-jobs-in-a-separate-bullmq-worker.md), [FND-09](01-platform-foundation/FND-09-set-up-automated-checks-and-development-logging.md), [GAP-06](07-gap-analysis/GAP-06-reassess-findings-affected-by-changed-inputs.md) |
| [EXT-08](10-production-hardening-and-framework-expansion/EXT-08-publish-additional-reviewed-framework-packs.md) | Publish additional reviewed framework packs | Post-MVP | [EXT-04](10-production-hardening-and-framework-expansion/EXT-04-map-requirements-across-frameworks.md), [KB-07](02-compliance-knowledge-base/KB-07-publish-the-initial-nis2-framework-pack.md) |
