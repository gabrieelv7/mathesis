# Mathesis

Mathesis is a study and spaced-repetition platform designed to help users organize learning material, create flashcards, schedule reviews, run study sessions, and track long-term progress.

The project is also a practical learning environment for modern backend and full-stack development with C#, ASP.NET Core, PostgreSQL, automated testing, CI/CD, and an evolutionary CQRS architecture.

> The project is currently under active development.

---

## Project goals

Mathesis has two main goals:

1. Build a useful study platform based on active recall and spaced repetition.
2. Serve as a portfolio project that demonstrates software design, backend development, testing, infrastructure, and architectural evolution.

The project intentionally starts as a modular monolith and will evolve only when new technical requirements justify additional complexity.

---

## Core idea

Users organize their studies into subjects and topics, create flashcards, and review them through scheduled study sessions.

Each flashcard review records the user's result and calculates the next review date.

A simplified flow:

```text
Create a subject
        ↓
Create topics and flashcards
        ↓
Start a study session
        ↓
Review flashcards
        ↓
Rate each answer
        ↓
Calculate the next review
        ↓
Update history and progress
```

Initial answer ratings:

```text
Again
Hard
Good
Easy
```

The first scheduling strategy will use simple and predictable intervals. Later versions may introduce adaptive and versioned scheduling algorithms.

---

## Planned features

### Authentication and account security

- User registration
- Email confirmation
- Login
- Access tokens
- Refresh tokens
- Refresh token rotation
- Session revocation
- Password recovery
- Account lockout
- Active session management

### Study organization

- Subjects
- Topics
- Tags
- Archiving and restoration
- Search and filtering
- Manual ordering
- Weekly goals

### Flashcards

- Basic question-and-answer cards
- Card organization by subject and topic
- Tagging
- Search and filtering
- Revision history
- Archiving
- Duplicate detection
- Additional card types in later versions

### Spaced repetition

- Daily review queue
- New and overdue cards
- Review history
- Next-review calculation
- Versioned scheduling strategies
- Deterministic and testable scheduling rules

### Study sessions

- Start, pause, resume, complete, cancel, and expire sessions
- Review limits
- Time limits
- Session summary
- Accuracy and duration metrics
- Idempotent answer submission

### Reporting

- Daily review count
- Overdue cards
- Study time
- Accuracy
- Subject progress
- Weekly activity
- Review calendar
- Difficult cards
- Future review workload

---

## Learning objectives

The project is intended to explore and demonstrate:

- C#
- .NET
- ASP.NET Core Web API
- HTTP and REST
- Entity Framework Core
- PostgreSQL
- Authentication and authorization
- JWT and refresh tokens
- Domain modeling
- Modular monoliths
- CQRS
- Domain events
- Read projections
- Eventual consistency
- Outbox and Inbox patterns
- RabbitMQ
- Background workers
- Idempotency
- Optimistic concurrency
- Docker
- GitHub Actions
- Integration testing
- Testcontainers
- Observability
- OpenTelemetry
- CI/CD
- Cloud deployment

---

## Initial technology stack

### Backend

- C#
- .NET 10
- ASP.NET Core Web API
- Controllers
- Entity Framework Core
- ASP.NET Core Identity
- OpenAPI

### Data

- PostgreSQL
- Npgsql

### Testing

- xUnit
- Testcontainers

### Development and infrastructure

- Linux Mint
- Visual Studio Code
- C# Dev Kit
- Docker
- Docker Compose
- Git
- GitHub
- GitHub Actions
- GitHub Container Registry

### Initial deployment direction

- Frontend: Cloudflare Pages
- API: container hosting platform
- Database: managed PostgreSQL
- CI/CD: GitHub Actions

The exact hosting providers may change as the project evolves.

---

## Initial architecture

Mathesis starts as a modular monolith with separated responsibilities.

```text
src/
├── Mathesis.Api
├── Mathesis.Application
├── Mathesis.Domain
└── Mathesis.Infrastructure

tests/
├── Mathesis.UnitTests
└── Mathesis.IntegrationTests

frontend/
docs/
.github/
```

### Project responsibilities

#### `Mathesis.Api`

- HTTP endpoints
- Controllers
- Authentication middleware
- OpenAPI
- Dependency composition
- API-specific configuration

#### `Mathesis.Application`

- Use cases
- Commands
- Queries
- Application validation
- Interfaces
- Orchestration

#### `Mathesis.Domain`

- Domain entities
- Value objects
- Business rules
- Domain services
- Scheduling rules
- Domain events

#### `Mathesis.Infrastructure`

- Entity Framework Core
- PostgreSQL
- ASP.NET Core Identity
- External services
- Persistence implementations
- Email delivery
- Infrastructure configuration

#### `Mathesis.UnitTests`

- Domain rules
- Scheduling algorithms
- Application behavior
- Pure business logic

#### `Mathesis.IntegrationTests`

- API behavior
- Database integration
- Authentication flows
- Migrations
- Persistence
- End-to-end infrastructure paths

---

## Dependency direction

```text
Mathesis.Api
├── Mathesis.Application
└── Mathesis.Infrastructure

Mathesis.Application
└── Mathesis.Domain

Mathesis.Infrastructure
├── Mathesis.Application
└── Mathesis.Domain
```

The Domain project should remain independent from ASP.NET Core, Entity Framework Core, and infrastructure concerns whenever possible.

---

## Architectural evolution

The architecture will evolve gradually.

### Stage 1 — Modular monolith

- Single application
- Single PostgreSQL database
- Clear module boundaries
- Commands and queries inside the same process

### Stage 2 — Logical CQRS

- Explicit commands and queries
- Dedicated handlers
- Separate read and write models
- Single physical database

### Stage 3 — Read projections

- Query-specific projection tables
- Dashboard projections
- Study statistics projections
- Projection rebuilding

### Stage 4 — Asynchronous processing

- Domain and integration events
- Background workers
- Outbox Pattern
- RabbitMQ
- Idempotent consumers
- Retry and dead-letter handling

### Stage 5 — Physical read/write separation

- Write database
- Read database
- Eventual consistency
- Projection lag monitoring
- Projection recovery and rebuilding

Microsservices and Event Sourcing are not initial goals.

---

## Roadmap summary

### Milestone 1 — Project foundation

- Repository setup
- Solution structure
- PostgreSQL
- Docker Compose
- Build and test pipeline

### Milestone 2 — Authentication

- Registration
- Login
- Access tokens
- Refresh tokens
- Session management
- Password recovery

### Milestone 3 — Core study domain

- Subjects
- Topics
- Tags
- Flashcards

### Milestone 4 — Spaced repetition

- Daily review queue
- Review history
- Scheduling strategy
- Study sessions

### Milestone 5 — First web interface

- Authentication screens
- Subject management
- Flashcard management
- Review session
- Basic dashboard

### Milestone 6 — First public deployment

- Docker image
- GitHub Actions
- API deployment
- Frontend deployment
- Managed PostgreSQL

### Milestone 7 — Advanced architecture

- CQRS
- Projections
- Events
- Outbox
- Messaging
- Separate read database

See the full roadmap in:

```text
docs/roadmap.md
```

---

## Repository workflow

The project uses a pull-request-based workflow, even when developed by a single contributor.

```text
Issue
  ↓
Feature branch
  ↓
Commits
  ↓
Pull request
  ↓
GitHub Actions
  ↓
Review
  ↓
Squash merge
```

Example branch names:

```text
feat/user-registration
feat/daily-review-queue
fix/refresh-token-reuse
docs/update-roadmap
chore/configure-editor
```

Example commit messages:

```text
feat(auth): add user registration
fix(reviews): prevent duplicate review submissions
test(reviews): add scheduling tests
docs: update project roadmap
ci: add pull request validation
chore: configure repository settings
```

---

## Local development

### Requirements

Install:

- .NET 10 SDK
- Git
- Docker
- Docker Compose
- Visual Studio Code
- C# Dev Kit

Optional tools:

- DBeaver
- Bruno
- PostgreSQL command-line client

Verify the environment:

```bash
dotnet --info
git --version
docker --version
docker compose version
```

---

## Build and test

Restore packages:

```bash
dotnet restore
```

Build:

```bash
dotnet build
```

Run tests:

```bash
dotnet test
```

Check formatting:

```bash
dotnet format --verify-no-changes
```

Run the API:

```bash
dotnet watch --project src/Mathesis.Api run
```

---

## Local infrastructure

The initial local infrastructure will run through Docker Compose.

Expected services:

```text
PostgreSQL
Mail development service
Additional services only when required
```

Start services:

```bash
docker compose up -d
```

View services:

```bash
docker compose ps
```

View logs:

```bash
docker compose logs -f
```

Stop services:

```bash
docker compose down
```

---

## Database migrations

The project uses Entity Framework Core migrations.

Create a migration:

```bash
dotnet ef migrations add MigrationName \
  --project src/Mathesis.Infrastructure \
  --startup-project src/Mathesis.Api \
  --output-dir Persistence/Migrations
```

Apply migrations:

```bash
dotnet ef database update \
  --project src/Mathesis.Infrastructure \
  --startup-project src/Mathesis.Api
```

Generate an idempotent SQL script:

```bash
dotnet ef migrations script \
  --idempotent \
  --project src/Mathesis.Infrastructure \
  --startup-project src/Mathesis.Api \
  --output artifacts/migrations-idempotent.sql
```

---

## CI/CD

GitHub Actions will be used for continuous integration and deployment.

### Pull request validation

The CI pipeline should execute:

```text
Restore
Build
Formatting validation
Unit tests
Integration tests
Frontend validation
```

Equivalent local commands:

```bash
dotnet restore
dotnet format --verify-no-changes
dotnet build --configuration Release --no-restore
dotnet test --configuration Release --no-build
```

### Main branch deployment

The deployment pipeline is expected to:

```text
Validate the solution
Build the API container
Publish the image to GHCR
Deploy the API
Build the frontend
Deploy the frontend
Run health checks
```

---

## Documentation

Planned documentation:

```text
docs/
├── product-vision-and-business-rules.md
├── technical-plan-and-learning-path.md
├── roadmap.md
├── development-commands.md
└── architecture-decisions/
```

Architecture decisions should be documented through ADRs when relevant.

Examples:

```text
ADR-001: Use ASP.NET Core Controllers
ADR-002: Use PostgreSQL
ADR-003: Start as a modular monolith
ADR-004: Use logical CQRS before physical separation
```

---

## Testing strategy

### Unit tests

Focus on:

- spaced-repetition rules;
- scheduling strategies;
- session state transitions;
- business validation;
- authorization rules;
- time and timezone behavior.

### Integration tests

Focus on:

- API endpoints;
- PostgreSQL behavior;
- Entity Framework mappings;
- migrations;
- authentication;
- refresh token flows;
- persistence;
- idempotency.

### Architecture tests

Future architecture tests may verify:

- Domain does not depend on Infrastructure.
- Application does not depend on API.
- Queries do not modify state.
- Commands do not depend on read projections.

---

## Security principles

The project should follow these principles:

- Never store plain-text passwords.
- Never commit secrets.
- Use short-lived access tokens.
- Rotate refresh tokens.
- Support session revocation.
- Apply rate limiting to sensitive endpoints.
- Validate all user input.
- Enforce authorization at the resource level.
- Avoid logging credentials, tokens, or sensitive user content.
- Treat uploads and generated HTML as untrusted.

---

## Non-goals for the first release

The first release will not include:

- Microservices
- Kubernetes
- Event Sourcing
- Multiple physical databases
- RabbitMQ
- Redis
- AI-generated flashcards
- Mobile application
- Desktop application
- Real-time collaboration
- Advanced gamification
- Complex note editing
- Large-scale distributed infrastructure

These capabilities may be introduced later when they solve a concrete problem.

---

## Contributing

This is primarily a personal learning and portfolio project.

External contributions may be accepted in the future. Until then, issues and discussions can be used for feedback, suggestions, and architecture review.

---

## License

This project is licensed under the MIT License. See the [LICENSE](LICENSE) file for details.

---

## Name

The name **Mathesis** comes from the Greek word associated with learning, study, and the acquisition of knowledge.
