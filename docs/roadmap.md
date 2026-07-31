# Mathesis — Development Roadmap

## 1. Objective of the roadmap

This roadmap was created to guide the evolution of Mathesis as a study project and portfolio.

The objective is to balance three fronts:

- build a usable product;
- learn relevant technologies for C# job opportunities;
- evolve the architecture gradually, without adding complexity too early.

Each phase must result in a functional, testable, and documented deliverable.

---

## 2. Principles of evolution

- Keep the system executable at the end of each phase.
- Introduce new technologies only when there is a concrete need.
- Avoid microservices, messaging and multiple databases before the domain is functional.
- Document architectural decisions.
- Work with issues, pull requests and GitHub Projects.
- Automate build, testing and deploy with GitHub Actions.
- Prioritize learning over delivery speed.
- Do not copy ready-made solutions without understanding the concepts involved.

---

# Phase 0 — Project preparation

## Objective

Create the base repository, local environment, and workflow on GitHub.

## Deliveries

- Create the public repository `mathesis`.
- Write the initial description of the project.
- Add license.
- Create `README.md`.
- Add functional and technical documentation.
- Configure GitHub Issues.
- Configure GitHub Projects.
- Define labels.
- Define branching strategy.
- Create templates for issues and pull requests.
- Configure master branch protection.
- Create the initial structure of the solution.
- Configure local PostgreSQL with Docker Compose.
- Create the initial GitHub Actions workflow.

## Suggested initial structure

```text
mathesis/
├── src/
│   ├── Mathesis.Api
│   ├── Mathesis.Application
│   ├── Mathesis.Domain
│   └── Mathesis.Infrastructure
├── tests/
│   ├── Mathesis.UnitTests
│   └── Mathesis.IntegrationTests
├── frontend/
├── docs/
├── .github/
│   └── workflows/
├── docker-compose.yml
├── README.md
└── LICENSE
```

## Concepts to study

- Git.
- Branches.
- Pull requests.
- Semantic Versioning.
- Conventional Commits.
- GitHub Actions.
- Docker.
- Docker Compose.
- Environment variables.
- Secrets.
- Structure of .NET solutions.

## Completion criteria

The project must:

- compile;
- run the API locally;
- connect to PostgreSQL;
- run tests;
- automatically validate pull requests on GitHub.

---

# Phase 1 — Basic API and persistence

## Objective

Build the API foundation and learn the complete flow between request, application, domain and database.

## Technical features

- Diagnostic endpoint.
- Configuration by environment.
- Connection to PostgreSQL.
- Entity Framework Core configuration.
- First migration.
- Centralized error handling.
- Standardized answers.
- OpenAPI documentation.
- Basic logs.
- API health check.
- Health check from the database.

## Concepts to study

- ASP.NET Core.
- Controllers.
- Routes.
- HTTP methods.
- Status codes.
- DTOs.
- Model binding.
- Middleware.
- Dependency injection.
- Entity Framework Core.
- Migrations.
- `DbContext`.
- Tracking.
- `AsNoTracking`.
- Entity configuration.
- Indices and constraints.

## Completion criteria

The API must be functional, documented and connected to the database, even without complete business functionalities.

---

# Phase 2 — Authentication and sessions

## Objective

Implement realistic authentication, focusing on security and session management.

## Features

- User registration.
- Login.
- Access token.
- Refresh token.
- Expiration of tokens.
- Refresh token rotation.
- Logout.
- Logout of a session.
- Logout of all sessions.
- List of active sessions.
- Blocking after invalid attempts.
- Password recovery.
- Password reset.
- Email confirmation.
- Basic login audit.

## Important rules

- Passwords should never be stored directly.
- Refresh tokens must be persisted securely.
- Revoked tokens cannot be reused.
- Reusing an old refresh token should invalidate its chain.
- Blocked users cannot obtain new tokens.
- Secrets should never stay in the repository.

## Concepts to study

- ASP.NET Core Identity.
- JWT.
- Claims.
- Authentication.
- Authorization.
- Police.
- Hashing.
- Refresh token rotation.
- Revocation.
- `HttpOnly` cookies.
- `Secure`.
- `SameSite`.
- Rate limiting.
- Protection against brute force.
- Endpoint security.

## Priority tests

- Valid login.
- Invalid login.
- Expired token.
- Refresh valid token.
- Refresh token revoked.
- Token reuse.
- Logout.
- Password recovery.
- User blocked.

## Completion criteria

A user must be able to register, authenticate, renew their session, and revoke their devices securely.

---

# Phase 3 — Subjects, topics and organization

## Objective

Create the first functional Mathesis module.

## Features

### Subjects

- Create matter.
- Edit article.
- Archive material.
- Restore matter.
- List subjects.
- Search for material.
- Order materials.
- Set description.
- Set icon or color.
- Set weekly goal.

### Topics

- Create topic.
- Edit topic.
- Archive topic.
- Organize topics by subject.
- Create simple hierarchy.
- Reorder topics.

### Tags

- Create labels.
- Associate tags with content.
- Filter by tags.

## Important rules

- Materials belong to the user.
- A user cannot access materials from another user.
- Archived subjects do not appear in standard queries.
- A topic belongs to a subject.
- A topic cannot create cycles in the hierarchy.
- Deletions should preserve history when necessary.

## Concepts to study

- Domain modeling.
- Relationships.
- One-to-many.
- Many-to-many.
- Soft delete.
- Pagination.
- Filters.
- Ordering.
- Search.
- Authorization by resource.
- Validations.
- Indexes.
- Referential integrity.

## Completion criteria

The user must be able to structure their study space into subjects, topics and labels.

---

# Phase 4 — Flashcards

## Objective

Build the platform’s content core.

## First version

Implement just the basic flashcard:

```text
Question
Answer
```

## Features

- Create flashcard.
- Edit flashcard.
- Archive flashcard.
- Restore flashcard.
- Associate with a subject.
- Associate with a topic.
- Associate tags.
- Duplicate flashcard.
- List flashcards.
- Search flashcards.
- Filter by subject, topic and tag.
- View basic change history.

## Important rules

- A flashcard must have a question and answer.
- An archived card does not enter sessions.
- User can only change their own cards.
- Changes can restart or preserve the schedule, according to the defined rule.
- Duplicate cards may be flagged in the future.

## Concepts to study

- Entities.
- Value Objects.
- Domain validation.
- Optimistic concurrency.
- Versioning.
- Sanitization.
- Cursor pagination.
- Projected queries.
- Composite indices.
- Audit.

## Future evolution

- Multiple choice.
- Fill in the gap.
- True or false.
- Typed answer.
- Cards with image.
- Cards with code.

## Completion criteria

The user must be able to create, organize, consult and maintain their flashcards.

---

# Phase 5 — Spaced Repetition

## Objective

Implement Mathesis' main business rule.

## Features

- Create a queue of reviews for the day.
- Identify new cards.
- Identify expired cards.
- Present one card at a time.
- Record the user's response.
- Classify as:
  - I made a mistake;
  - Difficult;
  - Good;
  - Easy.
- Calculate the next revision.
- Record previous interval.
- Register next interval.
- Maintain revision history.
- View next revision date.

## First strategy

Start with a simple, predictable rule:

```text
Again   → review in 1 day
Hard    → review in 3 days
Good    → review in 7 days
Easy    → review in 15 days
```

Then, evolve to an algorithm that considers:

- previous interval;
- number of correct answers;
- number of errors;
- difficulty;
- response time;
- frequency of forgetting.

## Concepts to study

- Strategy Pattern.
- Pure functions.
- Algorithms.
- Parameterized tests.
- Dates.
- Time zones.
- Abstracted clock.
- Algorithm versioning.
- Immutable history.
- Deterministic rules.

## Priority tests

- First hit.
- First mistake.
- Consecutive hits.
- Error after sequence of hits.
- Card delayed.
- New card.
- Algorithm change.
- Different time zones.

## Completion criteria

When answering a flashcard, the system should record the attempt and correctly calculate your next review.

---

# Phase 6 — Study Sessions

## Objective

Group reviews into sessions and create a complete study experience.

## Features

- Login.
- Define matter.
- Define number of cards.
- Set time limit.
- Pause session.
- Resume session.
- Cancel session.
- Complete session.
- Record responses.
- View final summary.
- Record duration.
- Record success rate.
- Register revised cards.
- Register new cards.

## Session states

```text
Created
Running
Paused
Completed
Cancelled
Expired
```

## Important rules

- A completed session cannot be changed.
- A canceled session should not count certain indicators.
- A card must not be registered twice for the same answer.
- Repeated requests must be idempotent.
- Abandoned sessions may expire.

## Concepts to study

- State machine.
- Idempotence.
- Competition.
- State persistence.
- Transactions.
- Timeout.
- Cancellation.
- Session recovery.

## Completion criteria

The user must be able to complete a complete session and consult its summary.

---

# Phase 7 — Web Frontend

## Objective

Create the first usable Mathesis interface.

## Home screens

- Login.
- Register.
- Password recovery.
- Basic dashboard.
- List of subjects.
- Subject creation.
- List of topics.
- List of flashcards.
- Flashcard registration.
- Revision queue.
- Study session.
- Session summary.
- Active sessions.
- User Settings.

## Concepts to study

- Componentization.
- State management.
- Routes.
- Forms.
- Validation.
- API consumption.
- Error handling.
- Token renewal.
- Authentication status.
- Loading states.
- Empty states.
- Accessibility.
- Responsiveness.
- Frontend testing.

## Completion criteria

A user should be able to use the main flow without relying on the API documentation.

---

# Phase 8 — First publication

## Objective

Make Mathesis publicly available and learn the deployment flow.

## Suggested structure

```text
GitHub
├── Repository
├── GitHub Actions
└── GitHub Container Registry

Cloudflare Pages
└── Frontend

Railway or Render
└── API ASP.NET Core

Neon
└── PostgreSQL
```

## Deliveries

- API Dockerfile.
- Image published in GHCR.
- CI workflow.
- Deployment workflow.
- Frontend published.
- Published API.
- Published image.
- Environment variables.
- Secrets configured.
- HTTPS.
- Health checks.
- Accessible logs.
- Migrations carried out with control.
- README with published environment.

## Pull request pipeline

```text
Checkout
→ restore
→ build
→ unit tests
→ integration tests
→ build frontend
```

## Main branch pipeline

```text
Validations
→ build image
→ push to GHCR
→ deploy API
→ build frontend
→ deploy to Cloudflare Pages
→ health check
```

## Concepts to study

- CI/CD.
- Docker.
- Registry.
- Secrets.
- Environments.
- Deploy.
- Rollback.
- Health check.
- DNS.
- HTTPS.
- CORS.
- Production logs.
- Migrations.

## Completion criteria

The project must be accessible via the internet and automatically published from the main branch.

---

# Phase 9 — Dashboard and statistics

## Objective

Create useful queries and prepare the project for CQRS.

## Features

- Revisions scheduled for today.
- Delayed revisions.
- New cards.
- Time studied.
- Cards answered.
- Hit rate.
- Progress by subject.
- Weekly activity.
- Study calendar.
- Cards with a higher error rate.
- Future load of revisions.

## Concepts to study

- Aggregations.
- Projections.
- Optimized queries.
- Indexes.
- Generated SQL.
- Execution plans.
- Cache.
- Time series.
- Pagination.
- Product metrics.

## Evolution

Start by consulting the primary database.

Create projections only when queries:

- become complex;
- require many joins;
- are performed frequently;
- need structures different from the writing model.

## Completion criteria

The dashboard must present useful data without degrading the performance of core operations.

---

# Phase 10 — Logical CQRS

## Objective

Clearly separate read and write operations without creating distributed infrastructure.

## Deliveries

- Commands.
- Queries.
- Handlers.
- Specific DTOs.
- Validations by use case.
- Separation between reading and writing models.
- Architectural tests.
- Dependency rules between projects.

## Command examples

```text
CreateSubject
CreateFlashcard
AnswerFlashcard
StartStudySession
CompleteStudySession
```

## Query examples

```text
GetTodayReviews
GetSubjectProgress
GetStudyDashboard
GetFlashcardHistory
GetWeeklyActivity
```

## Concepts to study

- CQRS.
- Command Handler.
- Query Handler.
- Separation of responsibilities.
- Pipeline behaviors.
- Result pattern.
- Architectural tests.
- Cohesion.
- Coupling.

## Restrictions of this phase

- Maintain a single database.
- Maintain a single application.
- Do not use a broker.
- Do not create microservices.
- Do not physically separate reading and writing.

## Completion criteria

Commands must not contain dashboard query logic, and queries must not change state.

---

# Phase 11 — Events and projections

## Objective

Introduce internal events and dedicated reading templates.

## Initial events

```text
FlashcardCreated
FlashcardReviewed
StudySessionCompleted
SubjectArchived
DailyGoalReached
```

## Projections

- Daily queue.
- Progress by subject.
- Weekly summary.
- Activity calendar.
- Performance by flashcard.
- User dashboard.

## Concepts to study

- Domain events.
- Application events.
- Integration events.
- Projections.
- Eventual consistency.
- Consumers.
- Idempotence.
- Reconstruction of projections.
- Versioning of events.

## Implementation strategy

Start by updating projections within the same process.

Then, move the processing to background.

## Completion criteria

Reading screens must use specific projections without compromising domain rules.

---

# Phase 12 — Asynchronous processing and messaging

## Objective

Evolve internal events for reliable asynchronous processing.

## Technical features

- Outbox Pattern.
- Publication worker.
- RabbitMQ.
- Consumers.
- Retry.
- Dead-letter queue.
- Inbox Pattern.
- Idempotence.
- Message monitoring.
- Manual reprocessing.

## Main flow

```text
User answers card
        ↓
Review is saved
        ↓
Message enters the outbox
        ↓
Worker publishes event
        ↓
Consumers update projections
        ↓
Dashboard is updated
```

## Concepts to study

- Messaging.
- Delivery at least once.
- Duplicity.
- Order of events.
- Retry.
- Transient faults.
- Poison messages.
- Outbox.
- Inbox.
- Eventual consistency.
- Workers.

## Completion criteria

Temporary failures in the broker or consumer should not cause loss of events.

---

# Phase 13 — Separate read database

## Objective

Complete the evolution to CQRS with physical separation of read and write.

## Structure

```text
Write PostgreSQL
├── users
├── subjects
├── flashcards
├── reviews
└── sessions

Read PostgreSQL
├── dashboard
├── progress
├── calendar
├── daily queue
└── statistics
```

## Technical features

- Separate connections.
- Separate migrations.
- Asynchronous projections.
- Rebuild projections.
- Delay indicator.
- Health check of projections.
- Reconciliation.
- Lag monitoring.

## Concepts to study

- Read model.
- Write model.
- Eventual consistency.
- Logical replication by events.
- Rebuild.
- Observability.
- Recovery.
- Synchronization strategies.

## Completion criteria

The application should continue to work even if projections are temporarily delayed.

---

# Phase 14 — Observability

## Objective

Make the system diagnosable.

## Logs

- Request.
- User.
- Use case.
- Duration.
- Error.
- Bank.
- Events.
- Consumers.
- Correlation ID.

## Metrics

- Duration of requests.
- Login failures.
- Reviews carried out.
- Sessions completed.
- Messages in the outbox.
- Consumer failures.
- Delay in projections.
- Bank errors.
- Time of queries.

## Tracing

```text
HTTP
→ Controller
→ Handler
→ Banco
→ Outbox
→ Broker
→ Consumer
→ Read database
```

## Technologies to study

- OpenTelemetry.
- Structured logs.
- Metrics.
- Distributed tracing.
- Prometheus.
- Grafana.
- Loki.
- Time.
- Alerts.

## Completion criteria

The system must allow responding:

- what failed;
- where it failed;
- for which user;
- in which request;
- how long it took;
- which service was affected.

---

# Phase 15 — Advanced Features

This phase must be guided by real needs.

## Notes

- Markdown.
- Autosave.
- Version history.
- Backlinks.
- Code blocks.
- Converting note to flashcard.

## Import and export

- CSV.
- JSON.
- Markdown.
- Anki.
- Jobs in the background.
- Bug reporting.

## Files

- Avatars.
- Images.
- PDFs.
- Audio.
- Cloudflare R2.

## Notifications

- Email.
- Web Push.
- Pending revisions.
- Weekly summary.
- Sequence at risk.

## Search

- PostgreSQL Full Text Search.
- Filters.
- Relevance.
- Indexing.
- Future OpenSearch.

## Security

- MFA.
- Passkeys.
- Social login.
- Detection of suspicious activity.
- Cloudflare Turnstile.
- Advanced auditing.

---

# Phase 16 — Mobile or desktop application

## Objective

Create a second client using the same API.

## Mobile

Best suited for:

- quick review;
- notifications;
- offline use;
- synchronization;
- study on the move.

## Desktop

Best suited for:

- editing notes;
- mass creation of cards;
- shortcuts;
- import of files;
- focused study environment.

## Concepts to study

- Shared API.
- Offline-first.
- Local database.
- Synchronization.
- Conflicts.
- Idempotency keys.
- Versioning.
- Local cache.
- Secure token storage.
- Push notifications.

---

# Key milestones

## Milestone 1 — Technical basis

- Repository.
- API.
- PostgreSQL.
- Tests.
- CI.

## Milestone 2 — Complete authentication

- Register.
- Login.
- Tokens.
- Sessions.
- Password recovery.

## Milestone 3 — Usable product

- Subjects.
- Flashcards.
- Daily queue.
- Spaced review.
- Sessions.

## Milestone 4 — Public interface

- Frontend.
- Deploy.
- GitHub Actions.
- Published environment.

## Milestone 5 — Advanced architecture

- CQRS.
- Projections.
- Events.
- Messaging.
- Read database.

## Milestone 6 — Expanded Product

- Statistics.
- Notes.
- Files.
- Notifications.
- Mobile or desktop.

---

# Recommended order

```text
1. Repository and environment
2. API and database
3. Authentication
4. Subjects and topics
5. Flashcards
6. Spaced repetition
7. Sessions
8. Frontend
9. Release
10. Statistics
11. Logical CQRS
12. Eventos
13. Mensageria
14. Read database
15. Observabilidade
16. Advanced features
17. Mobile or desktop
```

---

# Criteria for advancing between phases

Don't move forward just because the code works.

Each phase must have:

- demonstrable functionality;
- tests;
- documentation;
- error handling;
- minimum logs;
- revised pull request;
- issue closed;
- README update;
- tag or release when representing a milestone.

---

# Scope of the first public release

The first public version must contain:

```text
Registration and login
Session management
Subjects
Topics
Basic flashcards
Daily queue
Answer rating
Next-review calculation
Study sessions
History
Dashboard simples
Frontend web
Automatic deployment
```

It does not yet need to contain:

```text
RabbitMQ
Redis
Two databases
Mobile
AI
Advanced notes
Files
Gamification
Collaboration
Microservices
Kubernetes
```

---

# Final guideline

The best roadmap for Mathesis is one in which each new technology solves a problem that has already appeared in the product.

The project must allow you to explain in interviews not only what was used, but why each decision was made, which alternatives were considered and which architectural costs were accepted.
