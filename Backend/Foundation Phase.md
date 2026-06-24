::: container
# 🧠 Jalsa -- Foundation Phase Implementation Playbook [Phase 0 · 7 Tasks · Pre‑Development]{.small}

::: phase-banner
## 🔧 Phase 0 -- Foundation

**Purpose:** Establish the development environment, version control,
containerised local setup, CI pipeline, logging, and code quality
tooling. This phase ensures all team members can work consistently and
efficiently from day one.

::: meta
📋 Tasks: 7 ⏱️ Total Effort: \~28 hours 👤 Owners: M1 (Backend Lead) +
M5 (DevOps) 🔗 Dependencies: None
:::
:::

::: task-card
::: {.task-header onclick="toggleTask(this)"}
[FND-001]{.task-id} [Git Repository Setup]{.task-name} [ [P0]{.tag
.tag-blue} [Low]{.tag .tag-green} [2h]{.tag} [▾]{.chevron}
]{.task-badges}
:::

::: task-body
### Task Information

-   **Task ID:** FND-001
-   **Task Name:** Git Repository Setup
-   **Owner:** M1 (Backend Lead)
-   **Dependencies:** None
-   **Complexity:** [Low]{.tag .tag-green}
-   **Estimated Effort:** 2 hours

### Objective

Create a version‑controlled Git repository with a structured branch
strategy and protection rules to enable collaborative development.

### Business Purpose

Without proper version control, team members cannot work concurrently,
track changes, or roll back mistakes. This task ensures all code is
stored centrally, changes are reviewed, and the main branch remains
stable.

### Technical Purpose

Establish the source of truth for all project code. Enable CI/CD
integration, pull request workflows, and traceability from development
to production.

### Prerequisites

#### Backend

-   **Git basics:** Understanding of `clone`, `push`, `pull`, `branch`,
    `merge`.
-   **Branching strategies:** Familiarity with GitFlow or GitHub Flow.
-   **Why needed:** To create and manage branches correctly.
-   **Usage:** All team members will use Git daily.

#### Frontend

-   None -- this task is infrastructure.

#### Database

-   None.

### Inputs

-   Access to Git hosting service (GitHub / Azure DevOps / GitLab).
-   Team member list and their access levels.

### Outputs

-   Remote repository with `main` and `develop` branches.
-   Branch protection rules configured.
-   README.md with project overview and setup instructions.

### Detailed Backend Workflow

1.  **Create the repository** -- On GitHub/Azure DevOps, create a new
    repository named `jalsa-api` (or similar). Choose private
    visibility.\
    [Why: Centralises code storage. Expected result: empty repository
    with a default branch.]{style="color:#475569;"}

    ::: pitfall-box
    **Pitfall:** Creating a public repository -- Jalsa is a healthcare
    application; keep it private.
    :::
2.  **Clone locally** -- Run `git clone <repo-url>`.\
    [Why: Local development requires a working
    copy.]{style="color:#475569;"}
3.  **Create `develop` branch** -- `git checkout -b develop` then push.\
    [Why: `main` should only contain production‑ready code; `develop` is
    the integration branch.]{style="color:#475569;"}
4.  **Set branch protection rules** -- On the hosting service, protect
    `main` and `develop`:
    -   Require pull requests before merging.
    -   Require at least 1 approval.
    -   Require status checks to pass (once CI is added later).
    -   Disable force pushes.

    [Why: Prevents accidental direct commits to stable
    branches.]{style="color:#475569;"}
5.  **Create `README.md`** -- Include project name, description,
    technology stack, and setup steps.
6.  **Set up `.gitignore`** -- Use a standard .NET gitignore (include
    `bin/`, `obj/`, `appsettings.Development.json`, etc.).
7.  **Push initial commit** -- Commit README and .gitignore to
    `develop`.

### Data Flow

::: flow-diagram
[Developer]{.step} [→]{.arrow} [Local Git]{.step} [→]{.arrow} [Remote
Repo]{.step} [→]{.arrow} [Pull Request]{.step} [→]{.arrow} [Code
Review]{.step} [→]{.arrow} [Merge]{.step} [→]{.arrow} [Protected
Branch]{.step}
:::

### Deliverables

-   Remote repository accessible to the team.
-   Protected `main` and `develop` branches.
-   README.md with setup instructions.
-   `.gitignore` configured for .NET projects.

### Validation Checklist

-   All team members can clone the repository.
-   Direct push to `main` is blocked.
-   Pull request workflow is functional.
-   README is clear and accurate.

### Testing Strategy

*Unit Tests:* Not applicable -- this is infrastructure.

*Integration Tests:* Not applicable.

*Validation:* Have a team member clone the repo and verify access.

### Common Pitfalls

::: pitfall-box
**Pitfall:** Forgetting to protect the `main` branch -- accidental
direct commits can break production.\
**Mitigation:** Apply protection rules immediately after creation.
:::

::: pitfall-box
**Pitfall:** Using a weak branch naming convention -- use `feature/`,
`hotfix/`, `release/` prefixes.
:::

### Definition of Done

-   Repository exists and is accessible.
-   Branch protection rules are active.
-   README and .gitignore are committed.
-   All team members have been added with appropriate permissions.

### Learning Resources

#### Must Know

-   Git branching strategies (GitFlow / GitHub Flow).
-   Pull request workflow.

#### Nice To Know

-   Git hooks for automation.
-   Semantic versioning for releases.

#### Recommended Learning Order

1.  Git basics (1h).
2.  Branching strategies (1h).
:::
:::

::: task-card
::: {.task-header onclick="toggleTask(this)"}
[FND-002]{.task-id} [Solution Architecture]{.task-name} [ [P0]{.tag
.tag-blue} [Medium]{.tag .tag-amber} [4h]{.tag} [▾]{.chevron}
]{.task-badges}
:::

::: task-body
### Task Information

-   **Task ID:** FND-002
-   **Task Name:** Solution Architecture
-   **Owner:** M1 (Backend Lead)
-   **Dependencies:** FND-001 (Repository exists)
-   **Complexity:** [Medium]{.tag .tag-amber}
-   **Estimated Effort:** 4 hours

### Objective

Create the Clean Architecture solution skeleton with four layers --
Domain, Application, Infrastructure, and API -- ensuring proper
separation of concerns and dependency direction.

### Business Purpose

A well‑structured solution reduces technical debt, makes the codebase
understandable for new developers, and allows independent evolution of
each layer. This is critical for a system expected to evolve over years.

### Technical Purpose

Enforce Clean Architecture principles: the Domain layer contains
business entities and core interfaces, Application holds use cases and
DTOs, Infrastructure implements external concerns (database, AI, blob
storage), and API exposes endpoints. Dependencies point inward.

### Prerequisites

#### Backend

-   **Clean Architecture:** Understanding of layered architecture,
    dependency inversion.
-   **Why needed:** To correctly structure projects and references.
-   **Usage:** Every backend feature will be built within this
    structure.

#### Frontend

-   None -- this is backend infrastructure.

#### Database

-   None -- no database work in this task.

### Inputs

-   FND-001 repository.
-   .NET 8 SDK installed.

### Outputs

-   `Jalsa.sln` solution file.
-   Four class library projects: `Jalsa.Domain`, `Jalsa.Application`,
    `Jalsa.Infrastructure`, `Jalsa.API`.
-   Project references correctly configured.
-   Base NuGet packages added.

### Detailed Backend Workflow

1.  **Create solution** -- `dotnet new sln -n Jalsa`.\
    [Why: A solution file groups related
    projects.]{style="color:#475569;"}
2.  **Create Domain project** --
    `dotnet new classlib -n Jalsa.Domain -o src/Jalsa.Domain`.\
    [Why: The Domain layer holds entities, value objects, and repository
    interfaces -- it has no external
    dependencies.]{style="color:#475569;"}
3.  **Create Application project** --
    `dotnet new classlib -n Jalsa.Application -o src/Jalsa.Application`.\
    [Why: Contains DTOs, service interfaces, use case implementations,
    and validation. References Domain.]{style="color:#475569;"}
4.  **Create Infrastructure project** --
    `dotnet new classlib -n Jalsa.Infrastructure -o src/Jalsa.Infrastructure`.\
    [Why: Implements data access (EF Core), external services (AI, blob,
    email), and repositories. References Domain and
    Application.]{style="color:#475569;"}
5.  **Create API project** --
    `dotnet new webapi -n Jalsa.API -o src/Jalsa.API`.\
    [Why: Controllers, middleware, DI configuration. References
    Application and Infrastructure.]{style="color:#475569;"}
6.  **Add projects to solution** -- `dotnet sln add src/**/*.csproj`.
7.  **Add references**:
    -   Application → Domain
    -   Infrastructure → Domain, Application
    -   API → Application, Infrastructure
8.  **Add base NuGet packages** -- `Microsoft.EntityFrameworkCore`,
    `Serilog`, `AutoMapper`, `FluentValidation`, `Swashbuckle`.
9.  **Build and verify** -- `dotnet build` should succeed with no
    errors.
10. **Commit** -- commit the solution structure to `develop` branch.

### Data Flow

::: flow-diagram
[API]{.step} [→]{.arrow} [Application]{.step} [→]{.arrow}
[Domain]{.step}\
[Infrastructure]{.step} [→]{.arrow} [Application]{.step} [→]{.arrow}
[Domain]{.step}
:::

### API Design

No endpoints yet -- this task only creates the project structure.

### Deliverables

-   Solution file with four projects.
-   Correct project references.
-   Base NuGet packages installed.
-   Solution builds successfully.

### Validation Checklist

-   `dotnet build` passes without errors.
-   Domain project has no references to Infrastructure or API.
-   All projects are added to the solution.
-   Solution committed to `develop`.

### Testing Strategy

No tests required for this infrastructure task.

### Common Pitfalls

::: pitfall-box
**Pitfall:** Adding Infrastructure or API references to Domain --
violates Clean Architecture. Keep Domain pure.
:::

::: pitfall-box
**Pitfall:** Forgetting to add projects to the solution -- they won't
build together. Always use `dotnet sln add`.
:::

::: tip-box
**Tip:** Use `dotnet new` with the `-o` flag to place projects in the
`src/` folder for a clean structure.
:::

### Definition of Done

-   Solution builds without warnings or errors.
-   Project structure follows Clean Architecture.
-   Base NuGet packages are installed.
-   Solution is committed to `develop`.

### Learning Resources

#### Must Know

-   Clean Architecture -- Robert C. Martin.
-   dotnet CLI for project management.

#### Nice To Know

-   Vertical slice architecture as an alternative.

#### Recommended Learning Order

1.  Clean Architecture overview (2h).
2.  dotnet CLI basics (1h).
:::
:::

::: task-card
::: {.task-header onclick="toggleTask(this)"}
[FND-003]{.task-id} [Docker Compose Setup]{.task-name} [ [P0]{.tag
.tag-blue} [Medium]{.tag .tag-amber} [6h]{.tag} [▾]{.chevron}
]{.task-badges}
:::

::: task-body
### Task Information

-   **Task ID:** FND-003
-   **Task Name:** Docker Compose Setup
-   **Owner:** M5 (DevOps / QA / AI)
-   **Dependencies:** FND-002 (Solution exists)
-   **Complexity:** [Medium]{.tag .tag-amber}
-   **Estimated Effort:** 6 hours

### Objective

Create a Docker Compose configuration that runs the API, Angular
frontend, PostgreSQL database, Redis cache, and vector store (pgvector)
in isolated containers for local development.

### Business Purpose

Consistent development environments eliminate \"works on my machine\"
issues. New team members can spin up the entire stack with a single
command, reducing onboarding time.

### Technical Purpose

Containerise all services to mirror production infrastructure locally.
Enables testing of integrations (database, Redis, vector store) without
installing them natively.

### Prerequisites

#### Backend

-   **Docker & Docker Compose:** Understanding of images, containers,
    volumes, networks.
-   **Why needed:** To write the `docker-compose.yml` file.
-   **Usage:** All team members will use Docker for local development.

#### Frontend

-   **Angular build:** Basic understanding of `ng build` and serving via
    Node.
-   **Why needed:** The Angular container will serve the frontend.
-   **Usage:** Frontend developers will run the Angular container.

#### Database

-   **PostgreSQL basics:** Understanding of environment variables,
    ports, persistence.
-   **Why needed:** To configure the PostgreSQL container.
-   **Usage:** All data will be stored in the PostgreSQL container.

### Inputs

-   FND-002 solution structure.
-   Docker and Docker Compose installed.

### Outputs

-   `docker-compose.yml` file.
-   `Dockerfile` for the API.
-   `Dockerfile` for the Angular frontend.
-   Environment variables file (`.env`) for local development.
-   All services can be started with `docker-compose up`.

### Detailed Backend Workflow

1.  **Create API Dockerfile** -- Use
    `mcr.microsoft.com/dotnet/aspnet:8.0` as base, copy build output,
    expose port 5000.\
    [Why: The API runs inside a container.]{style="color:#475569;"}
2.  **Create Angular Dockerfile** -- Use `node:18` to build, then
    `nginx:alpine` to serve the built app.\
    [Why: Angular needs Node to build, but production serving uses
    Nginx.]{style="color:#475569;"}
3.  **Write docker-compose.yml** -- Define services:
    -   `api` -- builds from API Dockerfile, depends on database and
        redis.
    -   `frontend` -- builds from Angular Dockerfile.
    -   `database` -- uses `postgres:15` with pgvector extension.
    -   `redis` -- uses `redis:alpine`.
    -   `vector-store` -- optionally use pgvector or a separate service.

    [Why: All services run together with proper
    networking.]{style="color:#475569;"}
4.  **Configure volumes** -- Mount source code for hot reload in
    development.
5.  **Set environment variables** -- Use `.env` file for database
    connection, JWT secret, OpenAI keys.
6.  **Test** -- Run `docker-compose up --build` and verify all services
    start.
7.  **Commit** -- add Docker files to the repository.

### Data Flow

::: flow-diagram
[docker-compose up]{.step} [→]{.arrow} [API Container]{.step}
[↔]{.arrow} [DB Container]{.step}\
[API Container]{.step} [↔]{.arrow} [Redis Container]{.step}\
[Frontend Container]{.step} [→]{.arrow} [API Container]{.step}
:::

### Deliverables

-   `docker-compose.yml`.
-   `Dockerfile` for API.
-   `Dockerfile` for Angular.
-   `.env.example` with required variables.
-   All services start successfully.

### Validation Checklist

-   `docker-compose up` starts all containers without errors.
-   API is accessible at `http://localhost:5000`.
-   Frontend is accessible at `http://localhost:4200`.
-   Database is initialised and accepts connections.
-   Redis is running.

### Testing Strategy

*Manual validation:* Run `docker-compose up` and verify each service
responds.

*Automated:* Later, a health check endpoint can be added.

### Common Pitfalls

::: pitfall-box
**Pitfall:** Hard‑coding port mappings that conflict with other local
services -- use dynamic ports or document clearly.
:::

::: pitfall-box
**Pitfall:** Forgetting to set environment variables -- the API will
fail to connect to the database. Always provide a `.env.example`.
:::

::: tip-box
**Tip:** Use `docker-compose.override.yml` for developer‑specific
overrides (like volume mounts for hot reload).
:::

### Definition of Done

-   All Docker files are committed.
-   A new team member can run `docker-compose up` and have the full
    stack running within minutes.
-   Documentation in README explains the setup.

### Learning Resources

#### Must Know

-   Docker and Docker Compose basics.
-   Multi‑stage builds for .NET and Angular.

#### Nice To Know

-   Docker networks and volumes.
-   Health checks for containers.

#### Recommended Learning Order

1.  Docker basics (2h).
2.  Docker Compose (2h).
3.  Multi‑stage builds (1h).
:::
:::

::: task-card
::: {.task-header onclick="toggleTask(this)"}
[FND-004]{.task-id} [CI/CD Pipeline (Basic)]{.task-name} [ [P1]{.tag
.tag-amber} [Medium]{.tag .tag-amber} [6h]{.tag} [▾]{.chevron}
]{.task-badges}
:::

::: task-body
### Task Information

-   **Task ID:** FND-004
-   **Task Name:** CI/CD Pipeline (Basic)
-   **Owner:** M5 (DevOps / QA / AI)
-   **Dependencies:** FND-001 (Repository exists)
-   **Complexity:** [Medium]{.tag .tag-amber}
-   **Estimated Effort:** 6 hours

### Objective

Set up a Continuous Integration pipeline that automatically builds and
tests the solution on every pull request to `develop` and `main`
branches.

### Business Purpose

Early detection of integration issues, compilation errors, and test
failures prevents broken code from reaching production. It increases
team confidence and reduces manual verification effort.

### Technical Purpose

Automate the build and test process. Provide fast feedback to
developers. Establish a foundation for later deployment stages (CD).

### Prerequisites

#### Backend

-   **GitHub Actions / Azure DevOps:** Understanding of workflow YAML
    syntax.
-   **Why needed:** To define the pipeline.
-   **Usage:** The pipeline will run on every PR.

#### Frontend

-   None -- the pipeline will build the API only initially.

#### Database

-   None -- no database in this initial pipeline.

### Inputs

-   Repository with solution structure (FND-002).
-   Access to CI/CD platform (GitHub Actions or Azure DevOps).

### Outputs

-   CI pipeline YAML file (e.g., `.github/workflows/ci.yml`).
-   Pipeline executes on every PR.
-   Pipeline runs `dotnet build` and `dotnet test`.
-   Success/failure status is reported on PR.

### Detailed Backend Workflow

1.  **Create workflow file** -- In `.github/workflows/ci.yml` (or Azure
    DevOps YAML).
2.  **Define triggers** -- on `pull_request` to `develop` and `main`.
3.  **Set up .NET** -- Use `actions/setup-dotnet@v3` with .NET 8.
4.  **Restore dependencies** -- `dotnet restore`.
5.  **Build solution** --
    `dotnet build --no-restore --configuration Release`.
6.  **Run tests** -- `dotnet test --no-build --configuration Release`.
7.  **Add status badge** -- Add a badge to README showing pipeline
    status.
8.  **Test the pipeline** -- Create a dummy PR to trigger the workflow.
9.  **Commit** -- add the workflow file to `develop`.

### Data Flow

::: flow-diagram
[Developer]{.step} [→]{.arrow} [Push PR]{.step} [→]{.arrow} [GitHub
Actions]{.step} [→]{.arrow} [Build]{.step} [→]{.arrow} [Test]{.step}
[→]{.arrow} [Status Report]{.step} [→]{.arrow} [PR]{.step}
:::

### Deliverables

-   CI pipeline YAML file.
-   Pipeline runs on PR events.
-   Build and test steps pass.
-   Status badge in README.

### Validation Checklist

-   Pipeline triggers on PR creation.
-   Build step completes successfully.
-   Test step runs (even with no tests, it should succeed).
-   Pipeline status appears on the PR.
-   Badge is visible in README.

### Testing Strategy

*Manual:* Create a test PR and verify the pipeline runs.

*Automated:* The pipeline itself is the test -- future commits will
validate it.

### Common Pitfalls

::: pitfall-box
**Pitfall:** Using the wrong .NET version -- ensure the setup step uses
version 8.0.
:::

::: pitfall-box
**Pitfall:** Forgetting to include `--no-restore` and `--no-build` flags
-- can cause redundant operations and slow down the pipeline.
:::

::: tip-box
**Tip:** Cache the `~/.nuget/packages` folder to speed up restore.
:::

### Definition of Done

-   Pipeline file is committed.
-   Pipeline runs successfully on a test PR.
-   Status badge is added to README.
-   All team members are notified of the pipeline.

### Learning Resources

#### Must Know

-   GitHub Actions YAML syntax.
-   dotnet CLI commands for CI.

#### Nice To Know

-   Parallel job execution.
-   Artifact upload.

#### Recommended Learning Order

1.  GitHub Actions basics (2h).
2.  dotnet CI commands (1h).
:::
:::

::: task-card
::: {.task-header onclick="toggleTask(this)"}
[FND-005]{.task-id} [README & Documentation]{.task-name} [ [P2]{.tag
.tag-green} [Low]{.tag .tag-green} [4h]{.tag} [▾]{.chevron}
]{.task-badges}
:::

::: task-body
### Task Information

-   **Task ID:** FND-005
-   **Task Name:** README & Documentation
-   **Owner:** M1 (Backend Lead)
-   **Dependencies:** FND-002 (Solution exists)
-   **Complexity:** [Low]{.tag .tag-green}
-   **Estimated Effort:** 4 hours

### Objective

Create a comprehensive README.md that explains the project, technology
stack, setup instructions, and contribution guidelines.

### Business Purpose

New team members and stakeholders can quickly understand what the
project is about and how to get started. This reduces onboarding time
and prevents confusion.

### Technical Purpose

Document the architecture decisions, environment setup, and common
development tasks so that the team has a single source of truth.

### Prerequisites

#### Backend

-   **Markdown:** Basic knowledge of Markdown syntax.
-   **Why needed:** To write the README.
-   **Usage:** README will be viewed in the repository.

#### Frontend

-   None.

#### Database

-   None.

### Inputs

-   Project details (name, purpose, stack).
-   Setup steps from FND-003 (Docker).

### Outputs

-   `README.md` file in the repository root.

### Detailed Backend Workflow

1.  **Write project overview** -- What is Jalsa? Who is it for? What
    problem does it solve?
2.  **List technology stack** -- .NET 8, Angular 17, EF Core,
    PostgreSQL, OpenAI, etc.
3.  **Document setup steps** -- Prerequisites, cloning, Docker Compose,
    environment variables.
4.  **Add architecture diagram** -- Use Mermaid or a simple ASCII
    diagram to show the Clean Architecture layers.
5.  **Contribution guidelines** -- Branch naming, PR process, code
    style, testing.
6.  **Add status badges** -- CI pipeline status, code coverage (when
    available).
7.  **Commit** -- add README to `develop`.

### Deliverables

-   README.md with complete sections.
-   Setup instructions validated by another team member.

### Validation Checklist

-   README includes project description and stack.
-   Setup steps are clear and complete.
-   Architecture diagram is present.
-   Contribution guidelines are defined.

### Common Pitfalls

::: pitfall-box
**Pitfall:** Writing too much or too little -- keep it concise but
comprehensive. Link to deeper documentation for advanced topics.
:::

### Definition of Done

-   README.md is committed.
-   A new team member can follow the README to set up the environment.

### Learning Resources

#### Must Know

-   Markdown syntax.
-   Writing good technical documentation.

#### Recommended Learning Order

1.  Markdown quick reference (1h).
2.  Documentation best practices (1h).
:::
:::

::: task-card
::: {.task-header onclick="toggleTask(this)"}
[FND-006]{.task-id} [Code Style & Analyzers]{.task-name} [ [P2]{.tag
.tag-green} [Low]{.tag .tag-green} [3h]{.tag} [▾]{.chevron}
]{.task-badges}
:::

::: task-body
### Task Information

-   **Task ID:** FND-006
-   **Task Name:** Code Style & Analyzers
-   **Owner:** M1 (Backend Lead)
-   **Dependencies:** FND-002 (Solution exists)
-   **Complexity:** [Low]{.tag .tag-green}
-   **Estimated Effort:** 3 hours

### Objective

Configure consistent code style and static code analysis across the
solution to enforce quality and readability.

### Business Purpose

Consistent code style reduces cognitive load during code reviews and
makes the codebase more maintainable. Automated analyzers catch
potential bugs early.

### Technical Purpose

Set up `.editorconfig`, StyleCop, and SonarQube (or Roslyn analyzers) to
enforce naming conventions, formatting, and best practices.

### Prerequisites

#### Backend

-   **EditorConfig:** Understanding of how `.editorconfig` works.
-   **StyleCop / SonarQube:** Familiarity with static analysis tools.
-   **Why needed:** To define and enforce rules.
-   **Usage:** All developers will see warnings/errors in their IDE.

### Inputs

-   Solution structure (FND-002).

### Outputs

-   `.editorconfig` file at solution root.
-   StyleCop or Roslyn analyzer NuGet packages installed.
-   Rules configured to enforce naming, spacing, and documentation.

### Detailed Backend Workflow

1.  **Create `.editorconfig`** -- Use the built‑in template or start
    from a standard .NET config.
2.  **Define core rules** -- Indentation (2 spaces), brace style, naming
    conventions (PascalCase for classes, camelCase for parameters).
3.  **Add StyleCop.Analyzers** --
    `dotnet add package StyleCop.Analyzers` to all projects.
4.  **Configure severity** -- Set rules as `warning` or `error` to
    enforce compliance.
5.  **Add documentation rules** -- Require XML comments for public APIs.
6.  **Test** -- Write a sample class and verify analyzers catch
    violations.
7.  **Commit** -- add `.editorconfig` and NuGet packages.

### Deliverables

-   `.editorconfig` file.
-   StyleCop.Analyzers installed in all projects.
-   Code style rules enforced.

### Validation Checklist

-   `.editorconfig` is applied by the IDE.
-   Analyzers produce warnings for violations.
-   Build fails (or warns) on style violations.

### Common Pitfalls

::: pitfall-box
**Pitfall:** Overly strict rules that cause developer frustration --
balance quality with productivity. Use `warning` for non‑critical rules.
:::

### Definition of Done

-   `.editorconfig` is committed.
-   Analyzers are installed and configured.
-   Existing code passes all style checks.

### Learning Resources

#### Must Know

-   .editorconfig syntax.
-   StyleCop rules.

#### Recommended Learning Order

1.  .editorconfig basics (1h).
2.  StyleCop rules overview (1h).
:::
:::

::: task-card
::: {.task-header onclick="toggleTask(this)"}
[FND-007]{.task-id} [Logging & Telemetry Setup]{.task-name} [ [P1]{.tag
.tag-amber} [Medium]{.tag .tag-amber} [4h]{.tag} [▾]{.chevron}
]{.task-badges}
:::

::: task-body
### Task Information

-   **Task ID:** FND-007
-   **Task Name:** Logging & Telemetry Setup
-   **Owner:** M5 (DevOps / QA / AI)
-   **Dependencies:** FND-002 (Solution exists)
-   **Complexity:** [Medium]{.tag .tag-amber}
-   **Estimated Effort:** 4 hours

### Objective

Configure structured logging with Serilog and send logs to a centralised
sink (Seq or Application Insights) for debugging and monitoring.

### Business Purpose

Production issues need to be diagnosed quickly. Structured logs enable
filtering, searching, and correlation across services.

### Technical Purpose

Serilog writes structured logs (JSON) instead of plain text. The logs
are sent to Seq (local) or Application Insights (cloud) for
visualisation and alerting.

### Prerequisites

#### Backend

-   **Serilog:** Understanding of sinks, enrichers, and configuration.
-   **Why needed:** To integrate logging into the API.
-   **Usage:** All services will use `ILogger` from Serilog.

#### Frontend

-   None.

#### Database

-   None.

### Inputs

-   API project from FND-002.

### Outputs

-   Serilog configured in `Program.cs`.
-   Seq or Application Insights sink configured.
-   Structured logs appear in the sink.
-   Log enrichment with correlation IDs, user context.

### Detailed Backend Workflow

1.  **Install Serilog packages** -- `Serilog`, `Serilog.AspNetCore`,
    `Serilog.Sinks.Seq` (or `Serilog.Sinks.ApplicationInsights`).
2.  **Configure Serilog in Program.cs** -- Use
    `Log.Logger = new LoggerConfiguration()...`
3.  **Add enrichers** -- `WithEnvironmentName()`, `WithMachineName()`,
    `WithCorrelationId()`.
4.  **Set minimum log level** -- `Information` for development,
    `Warning` for production.
5.  **Add Seq sink** -- Provide URL and API key via configuration.
6.  **Add request logging middleware** --
    `app.UseSerilogRequestLogging()`.
7.  **Test** -- Make a request to the API and verify logs appear in Seq.
8.  **Commit** -- add configuration.

### Data Flow

::: flow-diagram
[API Request]{.step} [→]{.arrow} [Serilog Middleware]{.step} [→]{.arrow}
[Structured Log]{.step} [→]{.arrow} [Seq / App Insights]{.step}
[→]{.arrow} [Dashboards / Alerts]{.step}
:::

### Deliverables

-   Serilog configured in the API.
-   Seq/Application Insights sink active.
-   Logs appear with correlation IDs.

### Validation Checklist

-   Logs are written when the API starts.
-   Request logs include HTTP method, path, status code, duration.
-   Logs appear in the configured sink.
-   Correlation ID is propagated.

### Testing Strategy

*Manual:* Hit an API endpoint and check Seq.

*Automated:* Unit tests can assert that `ILogger` is called, but full
validation is manual.

### Common Pitfalls

::: pitfall-box
**Pitfall:** Using plain text logging instead of structured -- always
use `{Property}` syntax in message templates.
:::

::: pitfall-box
**Pitfall:** Logging sensitive data (PII) -- avoid logging patient
names, passwords, etc. Use `Destructure` policies to mask them.
:::

::: tip-box
**Tip:** Use `LogContext` to add contextual properties (like userId,
tenantId) to all logs in a scope.
:::

### Definition of Done

-   Serilog is configured in Program.cs.
-   Logs are visible in Seq (or App Insights).
-   Correlation IDs are present in logs.
-   Documentation updated.

### Learning Resources

#### Must Know

-   Serilog configuration and sinks.
-   Structured logging best practices.

#### Nice To Know

-   Seq query language.
-   App Insights telemetry.

#### Recommended Learning Order

1.  Serilog basics (1.5h).
2.  Seq setup (1h).
3.  Logging best practices (1h).
:::
:::

------------------------------------------------------------------------

## ✅ Phase Completion Criteria {#phase-completion-criteria style="background: #f0fdf4; border-left-color: #22c55e;"}

-   All 7 tasks (FND-001 through FND-007) are completed and their
    deliverables are committed to the `develop` branch.
-   The solution builds successfully (`dotnet build` passes).
-   Docker Compose starts all services (API, Angular, PostgreSQL, Redis,
    Vector store) without errors.
-   CI pipeline runs successfully on a test pull request.
-   README.md is comprehensive and includes setup instructions.
-   Code style analyzers (StyleCop) are configured and produce no
    warnings on the codebase.
-   Serilog is configured and logs appear in Seq (or Application
    Insights).
-   All team members have confirmed they can clone, build, and run the
    environment.

## 📋 Code Review Checklist (for Phase 0 PRs) {#code-review-checklist-for-phase-0-prs style="background: #f0f9ff; border-left-color: #3b82f6;"}

-   No direct commits to `main` or `develop` -- all changes via PR.
-   PR has at least one approval from a different team member.
-   CI pipeline passes (build + test).
-   Code follows the established style (StyleCop warnings fixed).
-   README is updated if relevant.
-   No secrets or hard‑coded values are committed.
-   Docker files are tested and work locally.
-   Logging is properly configured and tested.

## 🚀 Pull Request Checklist {#pull-request-checklist style="background: #fefce8; border-left-color: #f59e0b;"}

-   Branch is up‑to‑date with `develop` (rebase or merge).
-   CI pipeline is green.
-   Code review completed with approval.
-   All checklist items from the Code Review Checklist are satisfied.
-   Squash and merge (or rebase) to keep history clean.

## 🧪 Deployment Readiness Checklist {#deployment-readiness-checklist style="background: #fef2f2; border-left-color: #ef4444;"}

*For Phase 0, deployment readiness means the foundation is solid enough
to begin Phase 1 development.*

-   All Phase 0 tasks are complete and merged.
-   Docker Compose works for all team members.
-   CI pipeline is stable and passes for all PRs.
-   Logging infrastructure is verified.
-   Code quality tooling is active and enforced.
-   Documentation is sufficient for a new developer to start Phase 1.

------------------------------------------------------------------------

::: {style="text-align: center; color: #94a3b8; font-size: 0.9rem; padding: 20px 0 0;"}
Jalsa -- Foundation Phase Implementation Playbook • v1.0 • Generated for
the development team
:::
:::
