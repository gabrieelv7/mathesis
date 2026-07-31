# StudyFlow — Technical Plan and Implementation Track

*Architecture, study concepts, stages, completion criteria and suggested schedule.*

| **OBJECTIVE** | Guide the construction of a demonstrable product, used as a learning laboratory and professional portfolio. |
|--------------|----------------------------------------------------------------------------------------------------------------|

# 1. Technical objective of the project

This project should function as a practical learning path. The goal is not to start with the most complex architecture possible, but to evolve a functional application in stages, recording decisions, problems encountered and reasons for each change.

> Study Guideline
> Implement the product need first. Introduce an architectural concept when there is a concrete situation that allows you to demonstrate its value and costs.

# 2. Skills that the project can demonstrate

| **Area** | **Concepts to study and demonstrate** |
|-------------|---------------------------------------------------------------------------------------------------|
| Backend | HTTP APIs, domain modeling, validation, authentication, authorization, and background tasks. |
| Architecture | Modular monolith, separation of responsibilities, CQRS, events and eventual consistency.          |
| Data | Relational modeling, migrations, indexes, concurrency, transactions, projections and cache.           |
| Integration | Messaging, outbox, inbox, retries, dead-letter queue and idempotency.                             |
| Frontend | Authentication, forms, state, query cache, optimistic UI and error handling.       |
| Quality | Unit testing, integration, architecture and end-to-end.                                        |
| Operation | Containers, CI, structured logs, metrics, tracing and health checks.                             |
| Product | Incremental evolution, documentation, feedback and usage-based decisions.                          |

# 3. Implementation principles
- Start as a modular monolith and a single deployment unit.
- Keep the domain independent of infrastructure details.
- Use logical separation between commands and queries before separating databases or processes.
- Add messaging only when there are real asynchronous consumers.
- Document decisions and rejected alternatives in short architectural logs.
- Avoid code or infrastructure without functionality that justifies them.
- Deliver small complete flows, from interface to database and tests.
- Refactor after understanding the pain; not anticipate the entire architecture.

# 4. Suggested initial architecture

The solution can be organized as a modular monolith. Exact project names are not required; The important thing is to make limits and dependencies explicit.

| **Logical area** | **Responsibility** |
|-----------------|-------------------------------------------------------------------------------------|
| API | Endpoints, request authentication, HTTP contracts and global error handling. |
| Application | Use cases, commands, queries, authorization and orchestration.                      |
| Domain | Entities, value objects, invariants, events and business policies.           |
| Infrastructure | Persistence, tokens, email, files, cache and integrations.                        |
| Projections | Construction and updating of reading models.                                     |
| Workers | Outbox processing, notifications, projections and scheduled tasks.               |
| Web frontend | User experience and API consumption.                                            |
| Tests | Unity, integration, architecture and end-to-end.                                   |

# 5. Suggested business modules

| **Module** | **Initial responsibility** |
|--------------------|--------------------------------------------------------|
| Identity | Account, login, tokens, sessions and access recovery. |
| Study Organization | Workspaces, subjects, topics and decks.                 |
| Content | Notes and flashcard content.                       |
| Reviews | Scheduling, daily queue, responses and history.       |
| Study Sessions | Start, pause, end and summary of sessions.          |
| Goals | Goals, sequence and activity calendar.            |
| Reporting | Dashboards, progress and statistics.                  |
| Notifications | Preferences, scheduling and sending notices.           |

# 6. Evolution of CQRS

| **Stage** | **What to do** | **What to learn** |
|---------------------------|--------------------------------------------------------------------------------|---------------------------------------------------------------|
| Logical CQRS | Separate query commands, still using the same database and process.          | Responsibilities, input and output models, and use cases. |
| Reading models | Create specific queries for screens, without reusing entities such as DTOs. | Interface and performance oriented modeling.                 |
| Projections on the same database | Maintain aggregated tables for dashboard, calendar and queue.                    | Update projections and reconstruction.                      |
| Asynchronous processing | Update projections through events processed in worker.                         | Eventual consistency, retries and idempotency.                |
| Separate read database | Move projections to another instance or database.                                 | Partial failures, read delay and distributed operation.    |

> What CQRS does not require
> There is no need to adopt microservices, Event Sourcing, Kafka or different technologies for reading and writing. Separation can only begin at the organization of the code.

# 7. Technical topics and features that justify them

| **Concept** | **Functionality used as a laboratory** |
|---------------------------|--------------------------------------------------------------------------------------------|
| JWT and refresh token | Login, expiration, renewal, sessions per device, and revocation.                          |
| Authorization by resource | Ensure materials, decks, and cards belong to the user or accessible workspace.        |
| CQRS                      | Separate responses and content changes from dashboard and queue queries.              |
| Domain Events | Trigger consequences after review, session completed, goal achieved or content changed. |
| Outbox | Save a review and ensure the corresponding event is published.                  |
| Inbox and idempotency | Avoid double review when repeating a request or message.                                |
| Optimistic concurrency | Detect concurrent editing of notes and flashcards.                                         |
| Cache | Speed up the dashboard, daily queue, permissions, or public content.                           |
| Textual search | Search notes, questions, answers and tags.                                         |
| Scheduled Jobs | Reminders, weekly summary, session expiration and goal consolidation.                   |
| File storage | Images, PDFs, audio, or imports.                                                       |
| Offline-first | Synchronize responses made on the mobile app offline.                             |
| Observability | Monitor request, command, database, event, consumer and projection.                      |

# 8. Authentication strategy as a study track

1. Implement registration and login with secure password storage.

2. Issue short-lived access token.

3. Create persisted and expiring refresh token.

4. Add rotation with each renewal.

5. Detect old token reuse and revoke session family.

6. Allow logout from one session and all sessions.

7. Add email confirmation and password recovery.

8. Apply rate limiting to sensitive endpoints.

9. Later compare secure cookie storage and token held by the client.

# 9. Data strategy
- Initially use a relational database for the transactional model.
- Create versioned migrations and reproducible demo data.
- Design indexes based on real queries, not in advance.
- Save dates in a consistent format and convert to the user's time zone on the appropriate edge.
- Use identifiers, versions and audit records in a standardized way.
- Create rebuildable projections for indicators and queues.
- Physically separate reading and writing only after the version with projections is stable.

# 10. Testing as part of learning

| **Type** | **Priorities** |
|-------------------|------------------------------------------------------------------------------------------|
| Unitary | Review algorithm, goals, sequence, session states and invariants.                 |
| Integration | Persistence, migrations, authentication, transactions, idempotency, outbox and consumers. |
| Architecture | Dependencies between layers, lack of writing in queries and domain isolation.      |
| Contract | Format and compatibility of endpoints consumed by the frontend.                        |
| End to end | Registration, login, article creation, review and visualization of results.                |
| Loading — posterior | Daily queue, dashboard queries and event processing.                          |

# 11. Observability and operation
- Structured logs with correlation identifier and operation context.
- Request metrics, login failure, registered review, outbox queue and projection delay.
- Tracing across the API, application, database, broker and consumer.
- Separate health checks for availability and readiness.
- Simple dashboards to understand behavior and failures.
- Data protection: never record passwords, tokens or unnecessary sensitive content.
- Docker Compose for reproducible local environment.
- CI pipeline running build, testing and architecture validations.

# 12. Suggested schedule

The schedule below is for guidance. Each stage can last one or two weeks depending on availability. The criteria for advancement should be the completion of a usable flow, not just the end of the deadline.

| **Step** | **Main deliveries** | **Study concepts** | **Completion criteria** |
|------------------------|---------------------------------------------------------------------------------|--------------------------------------------------------------|-----------------------------------------------------------------------|
| 1\. Foundation | Repository, initial documentation, API, empty frontend, database and local environment. | Solution structure, Git, containers and migrations.          | Project scales reproducibly and has basic pipeline.          |
| 2\. Identity | Registration, login, access token, refresh token and logout.                          | Security, hashing, expiration, rotation and rate limiting.      | Account flow works and has integration tests.                |
| 3\. Organization | Matters, decks, topics and archiving.                                        | Modeling, validation, authorization and pagination.               | User organizes their own content without accessing other people’s data.        |
| 4\. Flashcards | Basic creation, editing, tags, and version history.                        | Domain, concurrency and extensible types.                   | You can prepare a usable set of cards.                |
| 5\. Review | Daily queue, response, initial algorithm and history.                           | Rules, pure functions, idempotency and parameterized tests. | Cards reappear according to the previous result.                     |
| 6\. Sessions | Beginning, pause, conclusion and summary.                                              | State machine and consistency.                           | A complete session can be run from start to finish.              |
| 7\. CQRS and projections | Dashboard, calendar and progress by subject.                                  | Specific queries, projections and reconstruction.               | Indicators are updated and can be recalculated.                 |
| 8\. Events and outbox | Worker, publishing and projection consumers.                                  | Messaging, outbox, inbox, retry and DLQ.                      | Failures do not lose events and duplications do not corrupt data.          |
| 9\. Read database | Physical separation of projections.                                                 | Eventual consistency and distributed operation.                | Application remains functional with delay or reconstruction of projections. |
| 10\. Advanced Product | Goals, notifications, search and import.                                        | Jobs, caching, fetching and asynchronous processing.               | Advanced functionality is complete and observable.               |
| 11\. Mobile or desktop | New client consuming the same API.                                            | Contracts, synchronization and cross-platform experience.      | Review flow works on a second client.                      |
| 12\. Portfolio | Demo, diagrams, ADRs, metrics and retrospective.                                | Technical communication and presentation.                          | An evaluator understands the product and decisions without reading all the code.   |

# 13. Study plan per cycle

| **In each cycle** | **Questions to answer** |
|----------------------|-------------------------------------------------------------------------------|
| Before implementing | Which user problem will be solved? Which rule needs to be protected?    |
| During | What failures can occur? Can the operation be repeated? Is there concurrency?    |
| When testing | What edge cases, permissions, and transitions need to be checked?       |
| When observing | What logs, metrics and traces help you understand the flow?                      |
| When finished | What did I learn? What became complex? Which decision would change?                    |
| By documenting | Why was this approach chosen and what alternatives were considered? |

# 14. Recommended artifacts for GitHub
- README with problem, screenshots, execution instructions and main decisions.
- Context diagram and current architecture diagram.
- Documentation of the authentication and token renewal flow.
- Documentation of the review algorithm and its versions.
- Response flow diagram, outbox, broker, projection and read database.
- ADRs for important decisions, such as modular monolith, broker and read database.
- Public roadmap with completed features and next steps.
- Demo data and short usage script.
- Testing and quality reporting in the pipeline.
- Architectural retrospective explaining what evolved between versions.

# 15. Quality criteria for each functionality
- Has a clear description of the problem and rules.
- Includes validations and authorization.
- There are tests proportional to the risk.
- Treats repetition, concurrency or failures when applicable.
- Exposes errors in an understandable way to the frontend.
- Can be observed by logs or metrics.
- It is documented to the level necessary for someone else to perform and evaluate.
- Does not introduce infrastructure without demonstrable need.

# 16. Learning Risks and How to Avoid Them

| **Risk** | **How ​​to avoid** |
|---------------------------------------------|----------------------------------------------------------------------------------|
| Getting stuck in architecture before product | Deliver a simple, usable vertical flow first.                        |
| Add too many technologies | Limit each step to a new main concept.                                 |
| Create CRUD without rules | Prioritize review, scheduling, session, idempotency, and history.                |
| Copying ready-made solutions without understanding | Record hypotheses, alternatives and the reason for each decision.                    |
| Abandon due to large scope | Maintain separate backlog between core, evolution and experiments.                   |
| Portfolio difficult to evaluate | Prepare demo, screenshots, diagrams and five-minute script.                |
| Fragile or excessive testing | Test critical rules and integrations; avoid testing internal details that are worthless. |
| Separate banks too soon | Validate projections and reconstruction in the same database before physical separation.       |

# 17. Recommended order of deepening

10. Fundamentals of API, HTTP, persistence and authentication.

11. Domain modeling and review rule testing.

12. Web frontend consuming complete flows.

13. Logical CQRS and screen-oriented queries.

14. Events, projections and background tasks.

15. Outbox, messaging and idempotency.

16. Observability and failure handling.

17. Separation of the read database.

18. Cache, search and files.

19. Offline and second client synchronization.

# 18. Defining project ready as portfolio
- A person can run the environment by following the README.
- The main flow works from registration to review and dashboard.
- Critical rules have automated tests.
- The current architecture is represented in diagrams.
- Important decisions have justification.
- The system has enough logs, metrics or tracing to explain an operation.
- There is an accessible visual demonstration.
- Repository history shows incremental evolution, not just a final delivery.
- The author manages to explain costs, limitations and next steps without presenting architecture as a universal solution.

> Expected result
> In the end, the repository should show not only that you know how to use technologies, but that you can identify problems, model rules, evolve an architecture, test, operate and explain the decisions made.
