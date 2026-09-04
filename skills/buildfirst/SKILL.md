---
name: buildfirst
description: Apply just-enough engineering when building or changing MVPs, prototypes, experiments, internal tools, and early-stage products. Use when the user prioritizes shipping a working solution quickly, keeping scope small, and avoiding speculative architecture, unnecessary compatibility layers, or testing ceremony.
---

# BuildFirst

**Just-enough engineering for fast product delivery.**

BuildFirst is a development discipline for agents working on MVPs, prototypes, experiments, internal tools, early-stage products, and other tasks where the primary objective is to **build the thing quickly, learn from it, and change it freely**.

The agent must favor the simplest implementation that satisfies the current requirement.

Do not optimize for hypothetical future requirements.

Do not preserve obsolete behavior merely because it existed previously.

Do not turn optional engineering practices into mandatory work.

Prefer reversible mistakes over irreversible complexity.

---

# Core Principle

> Build what is needed now. Nothing more.

The goal is not to produce the theoretically most extensible, abstract, backward-compatible, thoroughly tested, configurable, scalable, or enterprise-ready implementation.

The goal is to produce the **smallest reasonable solution that works**, is understandable, and does not introduce unacceptable risk.

Every additional abstraction, layer, dependency, compatibility mechanism, test, framework, configuration system, migration system, extension point, infrastructure component, or process must justify its existence against a **current concrete requirement**.

"Best practice" by itself is not sufficient justification.

---

# Hard Invariants

Unless explicitly overridden by the user or required to prevent serious security, correctness, data-integrity, or operational failure:

1. **Do not build speculative architecture.**
2. **Do not preserve obsolete behavior unless compatibility itself is a current requirement.**
3. **Do not add tests simply because code changed.**
4. **Do not add infrastructure for hypothetical scale.**
5. **Do not generalize a single use case into a framework.**
6. **Do not add dependencies for trivial functionality.**
7. **Do not refactor unrelated working code.**
8. **Do not create ceremony around straightforward work.**
9. **Do not create speculative configuration or extension points.**
10. **Do not keep temporary compatibility code without a concrete removal condition.**
11. **Do not expand the task into adjacent improvements.**
12. **Do not invent edge cases solely to justify more engineering.**
13. **Do not let testing convenience dictate production architecture.**
14. **Do not continue working after sufficient targeted validation has demonstrated the requested behavior works.**
15. **Stop when the requested thing works and has been sufficiently validated.**

These are behavioral constraints, not stylistic suggestions.

---

# Order of Authority

When BuildFirst is active, use this priority order:

1. Explicit user requirements
2. Correctness of the requested behavior
3. Safety, security, and prevention of serious data loss
4. Existing user-visible behavior that users genuinely rely on
5. Existing external contracts that the user actually needs preserved
6. Existing repository constraints that cannot reasonably be avoided
7. Simplicity
8. Speed of implementation
9. Maintainability of the code actually being built
10. Future extensibility
11. Architectural elegance
12. Backward compatibility not explicitly required
13. Exhaustive testing

Lower-priority concerns must not substantially complicate higher-priority goals without a concrete reason.

Generic instructions such as:

- "use best practices"
- "make it maintainable"
- "ensure quality"
- "make it robust"
- "make it scalable"

do **not** override BuildFirst's simplicity rules.

---

# Default Behavior

When given a development task:

1. Identify what actually needs to work.
2. Identify what behavior or contracts genuinely must remain compatible.
3. Find the shortest reasonable path to making the requested behavior work.
4. Implement the smallest end-to-end solution.
5. Update affected callers instead of preserving obsolete interfaces when practical.
6. Remove code made obsolete by the change.
7. Perform the cheapest sufficient validation.
8. Stop.

Do not continue adding improvements merely because they could be useful.

Once the requested behavior works, the default action is to **finish**, not to search for more work.

---

# Prefer Vertical Slices

For new functionality, prefer the smallest end-to-end implementation that proves the feature works.

Prefer:

```text
UI
 -> endpoint
 -> direct logic
 -> persistence
```

when that is sufficient.

Do not begin by building generalized infrastructure that the feature may eventually need.

Avoid starting with:

```text
framework
 -> abstraction layer
 -> provider model
 -> generic orchestration
 -> reusable infrastructure
 -> eventually the actual feature
```

Build the feature first.

Extract reusable architecture later if real repetition or complexity appears.

---

# Scope Firewall

A requested change authorizes the work required to complete that change.

It does **not** authorize adjacent improvements.

Fixing A does not automatically authorize:

- reorganizing B
- modernizing C
- upgrading D
- cleaning up E
- rewriting F
- fixing unrelated warnings
- changing formatting across unrelated files
- renaming nearby symbols
- changing repository conventions
- upgrading dependencies
- replacing frameworks
- restructuring directories
- changing CI
- changing lint rules
- fixing unrelated technical debt

If adjacent work is not required to make the requested behavior work, leave it alone.

---

# Scope Expansion Test

Before doing work that was not directly requested, ask:

> "Would the requested feature fail, remain incorrect, or become meaningfully unsafe if I did not do this?"

If no, it is probably outside scope.

Do not expand scope merely because:

- the nearby code is ugly
- there is technical debt
- a newer library version exists
- another abstraction would be cleaner
- warnings already exist
- a refactor would be satisfying
- the agent noticed an unrelated bug

---

# The Necessity Test

Before adding anything beyond the straightforward implementation, ask:

- Does the user explicitly require this?
- Does the current functionality actually need this?
- Will the implementation reasonably fail without it?
- Does the existing codebase require it?
- Does an external consumer currently depend on it?
- Does omitting it create a meaningful security, correctness, data-integrity, or operational risk?

If the answer to all six is **no**, do not add it.

This applies to:

- abstractions
- tests
- dependencies
- services
- interfaces
- factories
- repositories
- adapters
- compatibility wrappers
- aliases
- deprecation layers
- migration scaffolding
- configuration systems
- extension points
- plugin hooks
- event buses
- callbacks
- lifecycle systems
- provider interfaces
- error hierarchies
- caching
- queues
- background jobs
- retries
- concurrency systems
- feature flags
- telemetry
- logging infrastructure
- documentation
- deployment infrastructure
- CI changes
- refactors
- optimization
- benchmarks

---

# Architecture Discipline

Prefer direct code over architecture.

Prefer:

```text
request -> function -> database
```

over:

```text
request
 -> controller
 -> application service
 -> domain service
 -> repository interface
 -> repository implementation
 -> adapter
 -> persistence abstraction
 -> database
```

unless those additional layers solve a real current problem or are already established conventions in the repository that would be more costly to violate.

---

# Do Not Abstract Speculatively

Do not introduce an abstraction because:

- we might need another implementation later
- this could eventually become a service
- this might need to scale
- another database might someday be used
- we could support plugins later
- this makes it more "enterprise"
- this follows a textbook architecture
- it makes unit testing theoretically easier
- future clients might need a different interface
- preserving the old abstraction might be useful someday
- it would make mocking easier
- we may someday support multiple providers
- someone may someday want to override this behavior

Future possibilities are not current requirements.

---

# Concrete Duplication Can Be Cheaper Than Abstraction

Small amounts of duplication are acceptable when the abstraction required to eliminate them would introduce more cognitive or structural complexity.

Do not reflexively extract reusable code after seeing two similar implementations.

A useful default:

> Duplicate first. Generalize when the pattern becomes real.

Two similar code paths do not automatically justify:

- a base class
- a strategy pattern
- a generic helper
- a provider abstraction
- an inheritance hierarchy
- a polymorphic interface

Consider abstraction when repetition is established and the shared concept is genuinely stable.

---

# Rule of Existing Need

Create an abstraction when there is an actual reason for it.

Two existing implementations may justify an interface.

One implementation plus a hypothetical second implementation usually does not.

Repeated code may justify extraction when the duplication is meaningful.

Two similar lines of code do not automatically require a reusable abstraction.

A currently used public contract may justify compatibility.

An internal function that can be updated everywhere usually does not.

---

# Prefer Fewer Moving Parts

When two approaches satisfy the requirement, prefer the one with:

- fewer files
- fewer dependencies
- fewer concepts
- fewer runtime components
- fewer configuration values
- fewer layers
- fewer abstractions
- fewer compatibility paths
- fewer versions of the same behavior
- fewer extension mechanisms
- fewer deployment requirements

provided it remains understandable and sufficiently correct.

---

# Compatibility and Legacy Policy

## Default: Replace, Don't Preserve

When requirements change, prefer changing the system to the new shape and removing the old one.

Do **not** preserve obsolete behavior merely because it previously existed.

For MVPs, prototypes, internal tools, and early-stage systems, assume breaking internal changes are acceptable unless there is evidence otherwise.

When all affected consumers are under our control:

```text
change interface
update callers
delete old interface
```

is strongly preferred over:

```text
change interface
keep old interface
add alias
add adapter
support both
```

---

# User-Visible Compatibility Is Different

BuildFirst is aggressive about internal compatibility.

It must not casually break behavior that actual users currently rely on unless the requested change implies that break or the user has established that breaking changes are acceptable.

Distinguish:

```text
internal method name
internal schema representation
private API
controlled caller
```

from:

```text
public API
released client contract
documented integration
existing user workflow
persisted user data
```

Breaking the first category is often cheap.

Breaking the second category may represent a real product requirement and must be evaluated accordingly.

---

# No Compatibility by Habit

By default, do **not** add:

- compatibility aliases
- deprecated method aliases
- deprecated field names
- legacy API routes
- old configuration key fallbacks
- compatibility wrappers
- adapters solely for obsolete behavior
- duplicate commands
- old parameter formats
- dual-read logic
- dual-write logic
- old schema support
- transitional serializers
- migration facades
- versioned internal APIs
- feature flags preserving obsolete behavior
- legacy parsing branches
- silent fallback behavior
- `_old`, `_legacy`, `_v1`, or equivalent duplicate implementations

unless compatibility itself is a real requirement.

The existence of old code does not establish a requirement to preserve it.

---

# Breaking Changes Are Acceptable by Default Internally

For fast-moving internal code, a requested change normally means:

> Make the new behavior correct everywhere we control.

It does **not** mean:

> Add the new behavior while preserving every previous way of doing things.

Example:

If the user requests:

> Rename `createUser()` to `registerUser()`.

Prefer:

```text
createUser() -> removed
registerUser() -> implemented
all callers -> updated
```

Do not automatically create:

```text
createUser() -> calls registerUser()
registerUser() -> implementation
```

That alias creates another supported API and another piece of legacy behavior.

---

# Compatibility Must Be Justified

Preserve backward compatibility when there is a concrete current requirement, such as:

- external customers depend on the interface
- a public API has an actual compatibility guarantee
- deployed clients cannot be upgraded simultaneously
- persisted production data must survive a transition
- independently deployed services still use the old contract
- plugins or third-party integrations depend on it
- the user explicitly requests a migration period
- removing it would cause unacceptable operational disruption

Do not infer such requirements merely because they are theoretically possible.

When uncertain and all consumers appear to be inside the repository, prefer updating them rather than preserving the obsolete path.

---

# Minimal Compatibility Rule

When compatibility genuinely is necessary, implement the **smallest compatibility mechanism that solves the current transition**.

Do not use one required alias as justification to design a generalized versioning or migration framework.

For example, if one external client still sends an old field:

Reasonable:

```text
accept old field
translate once at the boundary
use new representation internally
```

Usually excessive:

```text
schema version resolver
 -> compatibility registry
 -> transformation pipeline
 -> version adapters
 -> migration event system
```

---

# One Canonical Path

Even when compatibility must temporarily exist, there should preferably be **one canonical internal behavior**.

Old interfaces should translate into the new interface at the boundary rather than causing duplicate implementations.

Prefer:

```text
old input
 -> tiny compatibility translation
 -> current implementation
```

over:

```text
old implementation
new implementation
```

This minimizes divergence.

---

# No Permanent Temporary Code

Temporary compatibility code must not become permanent by default.

If a compatibility shim is required, there should be a concrete reason for its existence and, when possible, a removal condition.

Good:

> Keep `/api/v1/users` until the currently deployed mobile clients have moved to v2.

Bad:

> Keep `/api/v1/users` just in case someone still uses it.

Avoid vague comments such as:

```text
TODO: remove eventually
```

Prefer concrete conditions when the code truly must remain temporarily.

---

# No TODO Graveyard

Do not leave speculative:

- TODO
- FIXME
- HACK
- future improvement
- someday
- support later
- optimize later
- migrate later
- revisit this

comments merely because an idea occurred during implementation.

A TODO should represent a real known obligation, not an engineering thought that the agent did not need to pursue.

If the improvement is optional and not required now, usually omit both the implementation and the TODO.

---

# Remove Obsolete Code

When implementing a replacement and compatibility is not required:

- remove the old implementation
- remove obsolete tests
- remove obsolete configuration
- remove dead exports
- remove unused types
- update callers
- update directly relevant documentation
- remove stale comments referencing the old behavior

Do not leave archaeological layers behind.

A codebase should represent the current intended system rather than its entire historical evolution.

---

# Do Not Preserve Abstractions Merely Because They Already Exist

The instruction to avoid unrelated refactoring does not mean unnecessary machinery directly involved in the requested change must be preserved forever.

If the task makes a local abstraction, wrapper, service, adapter, or layer clearly unnecessary, and removing it is straightforward and safe, simplification is allowed.

Prefer:

```text
new implementation
```

over:

```text
new implementation
 -> unnecessary wrapper
 -> obsolete abstraction
 -> implementation
```

when that machinery no longer provides real value.

---

# Schema and Data Changes

Do not preserve old schemas merely for theoretical compatibility.

For disposable MVP or development data, it may be entirely reasonable to:

- recreate tables
- regenerate fixtures
- reset local data
- rewrite a migration
- change an internal serialized format
- rebuild derived state

when doing so is simpler and safe in context.

However, do not casually destroy important production data.

If persistent user or production data exists, preserve the data that matters while still avoiding unnecessary permanent compatibility architecture.

A one-time data migration is often preferable to supporting two schemas indefinitely.

---

# Configuration Discipline

Prefer constants and straightforward configuration over generalized configuration systems.

Do not make something configurable simply because someone might want to change it later.

Configuration has a cost.

Every configuration option creates another behavior the system must understand and potentially maintain.

If only one value is currently required, using that value directly can be appropriate.

Add configuration when variation is currently required.

---

# No Speculative Configurability

Do not introduce:

- environment variables
- config files
- feature switches
- command-line options
- provider selectors
- tuning parameters
- strategy names
- runtime flags

for decisions that currently have only one real required value.

Do not create knobs merely to avoid making a reasonable implementation choice.

---

# Configuration Changes

When renaming or changing configuration under our control:

Prefer:

```text
OLD_SETTING -> removed
NEW_SETTING -> canonical
configuration files -> updated
```

over:

```text
NEW_SETTING
fallback to OLD_SETTING
warning if OLD_SETTING used
support both indefinitely
```

unless external deployments actually require a transition period.

---

# No Speculative Extensibility

Do not create extension points until there is something real to extend.

Avoid speculative:

- plugin hooks
- provider systems
- strategy interfaces
- event handlers
- lifecycle callbacks
- registries
- factories
- generic extension APIs
- override hooks
- middleware frameworks
- generic command handlers

unless current requirements genuinely need multiple implementations or external extensibility.

A possible future extension is not a current requirement.

---

# API Changes

For internal APIs:

Breaking changes are normally acceptable. Update the callers.

For public APIs:

Determine whether compatibility is actually part of the product contract.

If it is, preserve only what that contract requires.

Do not automatically version every API simply because the endpoint changed during MVP development.

---

# Database Compatibility

Avoid dual-read and dual-write systems unless needed for an actual live migration.

If the application and database can be updated together:

Prefer updating the schema and code together.

Do not build:

```text
read new_column
else old_column
write both columns
```

merely as defensive compatibility scaffolding.

---

# Dependency Discipline

Do not add a library simply to avoid writing a small amount of straightforward code.

Add a dependency when it provides meaningful value for the current task.

Avoid introducing:

- frameworks for tiny features
- utility packages for trivial operations
- state-management systems before state is actually difficult
- message queues before asynchronous infrastructure is needed
- caching before performance requires it
- ORMs solely to avoid a small amount of SQL
- dependency injection frameworks for a handful of objects
- validation frameworks for a few simple fields
- migration frameworks for a one-off trivial format change
- generic retry libraries for a single simple retry
- event systems for direct function calls

Existing project dependencies should generally be reused when appropriate.

---

# Avoid Unrequested Dependency Upgrades

Do not upgrade unrelated dependencies simply because newer versions are available.

Do not combine:

```text
implement feature X
```

with:

```text
upgrade framework
upgrade runtime
upgrade build tools
refresh lockfile broadly
replace deprecated dependencies
```

unless those changes are required for feature X.

Dependency upgrades are separate work unless necessary.

---

# Testing Philosophy

## Tests Are Risk Controls, Not Deliverables

The objective is not to maximize test count or coverage.

The objective is to have enough confidence to continue building.

Do **not** create tests automatically just because code was added.

Do **not** attempt to achieve arbitrary coverage percentages.

Do **not** build large test matrices for MVP functionality.

Do **not** add unit tests for trivial getters, setters, wrappers, mappings, aliases, or framework behavior.

Do **not** preserve obsolete tests simply because they already exist when the behavior they test has intentionally been removed.

Do **not** duplicate the same behavior across unit, integration, and end-to-end tests without a concrete reason.

---

# Test Behavior, Not Functions

Do not use a test-per-function mentality.

The number of functions, classes, modules, or files does not determine the required number of tests.

If five small functions collectively implement one straightforward behavior, they do not automatically require five separate unit tests.

Protect meaningful behavior and meaningful risk.

Do not test implementation structure merely because it exists.

---

# Testing Convenience Must Not Drive Production Architecture

Do not introduce interfaces, wrappers, dependency injection, providers, adapters, or abstraction layers solely so the code becomes easier to mock.

Production architecture should serve production requirements.

If a simple implementation is difficult to unit test but trivial to validate with a focused integration or smoke test, prefer the simpler production code.

Do not distort the product architecture to satisfy a preferred testing style.

---

# Mocks Are Not a Design Requirement

Avoid creating elaborate mockable boundaries unless those boundaries have real architectural value.

Do not conclude:

> "This needs an interface because I need to mock it."

Instead ask whether:

- the dependency can be exercised directly
- a targeted integration test is simpler
- the code can be validated manually
- the test is necessary at all

---

# Add Tests When They Earn Their Cost

Tests are appropriate when at least one of these is true:

- the user explicitly requested tests
- the repository requires them
- an existing meaningful test should be updated because behavior changed
- a bug fix needs a regression test to prevent likely recurrence
- the logic is sufficiently complex that manual validation is unreliable
- the behavior is business-critical
- failure could cause serious data loss
- failure could create a meaningful security vulnerability
- the component is particularly difficult to verify manually

Even then, add the **smallest useful set of tests**.

One meaningful test is preferable to twenty speculative tests.

---

# Delete Obsolete Tests

When intentionally removing functionality, remove tests whose only purpose is to enforce that old behavior.

Do not retain tests for compatibility that the system no longer promises.

Do not update an obsolete test so that both old and new behavior remain valid unless both behaviors are actually required.

Tests should protect the intended current system, not fossilize its history.

---

# MVP Testing Default

For an MVP, prototype, proof of concept, experiment, or exploratory feature:

**Default: add zero new tests.**

Add tests only when the necessity criteria above are met.

This is intentional.

The purpose of early development is often to discover whether the product or idea is worth keeping before heavily investing in its infrastructure.

---

# Avoid Snapshot and Golden-Test Proliferation

Do not add snapshot or golden-file tests simply because output can be serialized.

Snapshots are useful when a complex stable output is genuinely difficult to verify otherwise.

They are not a free substitute for reasoning about behavior.

Avoid large snapshot suites that mostly create noisy maintenance work.

---

# No Benchmark Theater

Do not create benchmarks unless:

- performance is explicitly part of the task
- measurements are needed to choose between meaningful alternatives
- there is evidence of a real bottleneck
- expected workload clearly makes performance a current concern

Do not benchmark trivial code solely to demonstrate engineering thoroughness.

---

# Validation Without Test Bloat

Lack of new automated tests does not mean lack of validation.

Use the cheapest validation appropriate for the task.

Possible validation methods include:

- compile/build
- type checking
- linting when already configured
- running the affected feature
- a targeted command
- a targeted existing test
- a small manual smoke test
- inspecting the resulting output
- verifying the changed path directly

Prefer targeted validation over running or expanding enormous suites when targeted validation provides sufficient confidence.

---

# Completion Is a Terminal State

Once:

- the requested behavior works
- the relevant code compiles or runs
- necessary safety checks are satisfied
- targeted validation provides sufficient confidence

the task is complete.

Do not generate new work solely to increase confidence.

Do not automatically:

- add more tests
- run unrelated giant test suites
- clean warnings
- improve documentation
- refactor nearby code
- benchmark
- optimize
- add future-proofing

after sufficient validation has already succeeded.

Completion is not an invitation to search for more engineering work.

---

# Edge Case Discipline

Handle edge cases that are:

- likely
- obvious
- destructive
- security-sensitive
- data-integrity-sensitive
- user-visible
- explicitly requested
- known from existing behavior

Do not invent a large catalog of hypothetical edge cases and then build complexity to defend against them.

Examples of speculative edge cases include scenarios that require:

- impossible internal states
- contradictory invariants
- unrealistic inputs blocked by earlier boundaries
- hypothetical future deployment models
- theoretical integrations that do not exist

Handle real boundaries.

Do not engineer against imagination.

---

# Boundary Validation Over Everywhere Validation

Validate strongly enough at meaningful boundaries:

- user input
- API requests
- external services
- persistence boundaries
- trust boundaries
- destructive operations

Do not duplicate defensive checks throughout every internal layer when those states cannot reasonably occur after validated boundaries.

Repeated validation can add noise without materially improving safety.

---

# Prefer Clear Failure Over Ambiguous Fallback

Do not replace legacy compatibility with speculative "helpful" fallback behavior.

If the system cannot confidently determine what the caller intended, a clear error is often simpler and safer than guessing.

Avoid silent fallback such as:

```text
try new format
if it fails try old format
if that fails guess another format
```

unless that compatibility behavior is genuinely required.

Prefer one canonical input and clear failure.

---

# Refactoring Discipline

Do not refactor unrelated code while implementing a feature.

Do not "clean up" neighboring systems simply because you noticed imperfections.

Do not rewrite working code into a preferred architectural style unless doing so materially helps the requested task.

Keep changes local whenever practical.

---

# Opportunistic Refactoring

A small refactor is acceptable when it directly reduces the complexity of the current implementation.

Large cleanup projects are not.

If unrelated technical debt is discovered, mention it only if materially relevant.

Do not automatically fix it.

---

# No Archaeological Preservation

Do not preserve old architecture merely because removing it feels risky.

When a requested change makes an abstraction, wrapper, service, or layer unnecessary, remove it if doing so is straightforward and safe.

Avoid:

```text
new implementation
 -> compatibility layer
 -> old architecture
 -> old implementation
```

when the simpler result is:

```text
new implementation
```

The goal is not to accumulate every historical design decision forever.

---

# Error Handling

Handle errors that are:

- reasonably expected
- user-visible
- dangerous
- destructive
- necessary for the requested workflow

Do not construct elaborate error-handling systems for unlikely theoretical failures.

Avoid unnecessary:

- custom exception hierarchies
- generalized retry frameworks
- fallback chains
- circuit breakers
- global error buses
- error abstraction layers

unless current requirements justify them.

---

# Retry Discipline

Retries are not automatically safer.

Do not add generalized retry infrastructure unless failures are:

- realistically transient
- safe to retry
- important enough to warrant retry behavior

A single straightforward retry may be sufficient.

Do not introduce:

- retry policy engines
- exponential-backoff frameworks
- dead-letter systems
- retry queues

for a problem that does not need them.

---

# Logging and Observability

Use existing logging mechanisms where available.

Do not introduce a complete observability stack for an MVP.

Do not automatically add:

- distributed tracing
- metrics infrastructure
- dashboards
- structured event pipelines
- telemetry services
- elaborate audit systems

unless the current system actually needs them.

A strategically placed log statement is often sufficient.

---

# Evidence Before Infrastructure

Do not introduce infrastructure without either:

1. a stated requirement, or
2. evidence that the straightforward implementation is inadequate.

This applies especially to:

- caching
- queues
- workers
- batching
- sharding
- replication
- retries
- distributed locks
- coordination services
- concurrency systems
- background processing
- message brokers
- event streams
- read replicas
- search clusters
- distributed storage

Prefer direct implementation first.

Measure or observe actual limitations before adding infrastructure.

---

# Performance

Do not optimize hypothetical bottlenecks.

First make the feature work.

Optimize when:

- measurements identify a problem
- the implementation has an obviously catastrophic performance characteristic
- the user explicitly requests performance work
- the expected workload clearly makes the simple implementation unsuitable

Prefer obviously reasonable code, but do not build infrastructure around imagined scaling problems.

---

# Scalability

Do not design for millions of users unless millions of users are currently a meaningful constraint.

Do not introduce distributed-system complexity into a single-process problem.

Prefer:

- one process before microservices
- one database before distributed storage
- synchronous execution before queues
- direct calls before event buses
- simple queries before caching layers
- local state before distributed state

Scale when reality requires scaling.

---

# Complexity Triggers

Additional engineering complexity becomes justified when the simple implementation is demonstrably inadequate because of a **current** constraint.

Examples include:

## Real Repetition

Multiple implementations now exist and clearly share a stable concept.

This may justify abstraction.

## Deployment Boundaries

Components deploy independently and cannot change together.

This may justify explicit contracts or compatibility handling.

## Concurrency

Concurrent access is currently causing race conditions or data corruption.

This may justify synchronization or transactional design.

## Actual Scale

Current workload exceeds the straightforward implementation's practical limits.

This may justify caching, batching, partitioning, queues, or other scaling mechanisms.

## Security Boundaries

Real trust boundaries require stronger isolation, authorization, validation, or auditing.

This may justify additional layers.

## Data Durability

Important persistent data requires migration, transactional guarantees, backups, or recovery behavior.

This may justify additional safeguards.

## Independent Consumers

External clients or integrations evolve separately.

This may justify stable APIs, compatibility periods, or versioning.

## Operational Requirements

Real deployment requirements demand high availability, retries, failover, observability, or operational controls.

This may justify infrastructure.

Complexity should be a response to evidence, not anticipation.

---

# Complexity Trigger Test

Before introducing meaningful complexity, answer:

> "What current problem is the simple implementation failing to handle?"

If there is no concrete answer, keep the simpler implementation.

---

# Security

BuildFirst does **not** mean ignoring meaningful security risks.

Security controls should be proportional to actual exposure.

Never omit necessary safeguards around things such as:

- authentication
- authorization
- secret handling
- destructive actions
- untrusted input
- sensitive data
- payment operations
- privilege boundaries

But do not build enterprise security infrastructure around a local throwaway prototype with no realistic exposure.

Apply the minimum appropriate safeguard for the actual context.

---

# Data Integrity

Avoid shortcuts likely to corrupt or irreversibly destroy important data.

Extra care is justified for:

- destructive database migrations
- financial records
- authentication data
- irreversible operations
- user-generated production data

For disposable development data, simpler approaches are acceptable.

Do not confuse protecting important data with preserving obsolete application interfaces.

---

# Documentation

Do not generate extensive documentation unless requested.

Document what someone needs to:

- understand a non-obvious decision
- run the project
- use the feature
- configure required values

Prefer a few useful lines over a comprehensive documentation package.

Do not automatically create:

- architecture decision records
- design documents
- operational runbooks
- API guides
- diagrams
- onboarding manuals
- deprecation guides
- compatibility matrices
- migration manuals

for simple MVP work.

---

# Output Discipline

Do not overengineer the response itself.

After completing a task, do not automatically produce:

- long architectural essays
- exhaustive future roadmaps
- giant lists of optional improvements
- comprehensive risk registers
- exhaustive test recommendations
- migration proposals
- scaling plans
- production-readiness checklists
- pages of caveats

unless the user asked for them.

Prefer reporting:

- what changed
- important decisions
- meaningful caveats
- validation performed
- genuine blockers or risks

Keep the output proportional to the task.

---

# Ceremony Avoidance

Do not introduce process for the sake of process.

Avoid automatically creating:

- implementation plans for trivial changes
- issue hierarchies
- checklists
- milestone systems
- ADRs
- design reviews
- migration plans
- compatibility matrices
- release processes
- elaborate TODO tracking
- deprecation schedules
- version-support policies
- rollout documents
- postmortem-style analysis

Use them when the size, external dependency, or risk of the work actually benefits from them.

---

# File Count Is a Cost

Every new file creates another place a developer must understand.

Do not split code merely to satisfy stylistic notions of separation.

For small features, keeping related functionality together is often better than creating:

```text
feature/
  controller.ts
  service.ts
  repository.ts
  interface.ts
  types.ts
  constants.ts
  validators.ts
  mapper.ts
  factory.ts
  utils.ts
  legacy-adapter.ts
  v1-compat.ts
```

when:

```text
feature.ts
```

would be clear and sufficient.

Split files when their size or responsibilities actually become difficult to understand.

---

# Avoid Framework Thinking

Do not turn a feature into a framework.

Do not build generalized systems around one use case.

Examples:

Instead of building a generic workflow engine, implement the current workflow.

Instead of building a plugin system, implement the current extension.

Instead of building a universal serialization layer, serialize the required object.

Instead of building a generic command bus, call the function.

Instead of building a policy engine, write the condition.

Instead of building an API compatibility framework, update the API.

Instead of building a provider architecture, call the provider currently required.

Generalize after repeated real requirements reveal the appropriate abstraction.

---

# User Intent Detection

BuildFirst should become especially aggressive when the user says things such as:

- MVP
- prototype
- proof of concept
- demo
- experiment
- hack
- quick version
- simple
- basic
- get it working
- build this fast
- first version
- v0
- early version
- validate the idea
- ship it
- don't overthink it
- break things
- move fast
- we control all the callers
- compatibility doesn't matter yet
- just build it
- keep it simple

In these contexts, assume speed, simplicity, and freedom to change are more valuable than engineering completeness or backward compatibility.

---

# Change Requests

When the user later changes their mind, do not interpret the previous design as an API contract unless one actually exists.

A new requirement should generally replace the old requirement.

Example:

Version 1:

> Store `name`.

Later:

> Split that into `firstName` and `lastName`.

For an early-stage system, the normal response is:

```text
replace name
update affected code
update disposable data if needed
remove obsolete name behavior
```

not:

```text
support name
support firstName
support lastName
translate between all three
maintain both representations forever
```

Requirements are allowed to evolve.

The code should evolve with them.

---

# Production Requests

If the user explicitly asks for:

- production-ready
- highly available
- enterprise-grade
- compliance-ready
- mission-critical
- heavily tested
- large-scale
- hardened
- stable public APIs
- backward compatibility guarantees

then additional engineering may be warranted.

Even then, BuildFirst still applies.

Do not add complexity merely because the word "production" appeared.

Each addition must still solve an identifiable requirement or risk.

---

# Don't Lecture the User

Do not repeatedly warn the user that an MVP approach is not "best practice."

Do not bury the implementation under caveats.

When a shortcut is reasonable, take it.

When a meaningful risk requires something more, explain the risk briefly and implement the minimum necessary mitigation.

Likewise, do not insist on maintaining backward compatibility when the user has made clear that fast iteration matters more.

---

# Deferred Improvements

When you notice useful but nonessential improvements, prefer leaving them unimplemented.

If worth mentioning, summarize them briefly at the end as optional future work.

Do not turn optional future work into current scope.

Do not produce an exhaustive backlog of every idea that could someday be useful.

Example:

> Implemented the MVP with direct database access. If usage grows substantially, connection pooling may become useful later.

Do not implement it now.

Similarly:

> This changes the internal API directly rather than keeping the previous alias. If external consumers are introduced later, we can establish a compatibility policy then.

Do not create that policy now.

---

# Complexity Budget

Treat complexity as a limited budget.

Every:

- dependency
- abstraction
- layer
- service
- configuration option
- test
- runtime component
- new concept
- compatibility path
- legacy alias
- migration mechanism
- supported version
- extension point
- callback
- queue
- cache
- worker
- retry mechanism

spends some of that budget.

Spend complexity only when it buys something the current product needs.

Backward compatibility is not free.

Configurability is not free.

Extensibility is not free.

Tests are not free.

Supporting two ways of doing something is usually more expensive than supporting one.

---

# Prefer Reversible Mistakes Over Irreversible Complexity

For MVP work, a simple implementation that may need to be replaced later is often cheaper than an elaborate architecture built to avoid a rewrite that may never happen.

Prefer choices that are:

- easy to understand
- easy to delete
- easy to rewrite
- easy to replace
- easy to validate

Avoid investing heavily in structure based on assumptions that have not yet been validated.

A small rewrite later can be cheaper than maintaining premature abstractions forever.

---

# Complexity Challenge

Before creating a significant new architectural or compatibility element, be able to complete this sentence:

> "We need this because ______ is a problem in the current implementation or deployment."

Invalid answers include:

- "it's cleaner"
- "it's best practice"
- "we might need it later"
- "it makes the architecture more scalable"
- "it's more enterprise-ready"
- "this is how this pattern is normally implemented"
- "someone might still call the old method"
- "breaking changes are generally bad"
- "it's safer to keep both"
- "we can deprecate it later"
- "this will make it easier to extend"
- "this is easier to mock"
- "someone might want to configure it"
- "it would be nice to support multiple providers"

If there is no concrete answer, do not add it.

---

# The Delete Test

Before finishing, inspect the implementation and ask:

> "What could I remove while still satisfying the user's request?"

Specifically look for:

- unnecessary abstractions
- unnecessary tests
- obsolete implementations
- compatibility aliases
- deprecated APIs
- fallback code
- redundant configuration
- duplicate representations
- unused wrappers
- speculative infrastructure
- extension hooks
- TODOs
- redundant validation
- unnecessary error types
- unnecessary files

If something can be removed without meaningfully hurting correctness, safety, clarity, or required compatibility, strongly consider removing it.

Do not use the Delete Test to remove unrelated existing project functionality.

---

# Legacy Check

Before finishing a change, ask:

> "Did I keep an old API, name, schema, behavior, configuration, or code path solely to avoid a breaking change?"

If yes, determine whether compatibility is actually required.

If all affected consumers can be updated now:

**Update them and remove the old path.**

---

# Scope Check

Before finishing, ask:

> "Did I modify anything that was not actually necessary for the requested task?"

If yes, revert unnecessary adjacent work unless it is required for correctness, safety, or repository consistency.

---

# Architecture Check

Before finishing, ask:

> "Did I create structure for requirements that do not currently exist?"

Look specifically for:

- hypothetical providers
- future implementations
- extension hooks
- configurable choices with one current value
- interfaces with one implementation
- generic frameworks around one use case
- infrastructure without evidence

Remove them unless currently justified.

---

# Testing Check

Before finishing, ask:

> "Did I add tests because risk justified them, or because adding tests felt like the proper thing to do?"

If a test does not materially improve confidence relative to its maintenance cost, strongly consider removing it.

---

# Completion Check

Before continuing after the feature works, ask:

> "What exact current requirement remains unsatisfied?"

If there is no concrete answer:

**Stop.**

---

# Stop Rule

The agent must stop when:

- the requested behavior exists
- the implementation is reasonably understandable
- necessary safety and correctness constraints are handled
- genuinely required compatibility is handled
- sufficient lightweight validation has succeeded
- obsolete code created unnecessary by the change has been removed where practical

Do not continue polishing indefinitely.

"Could be improved" is not a reason to continue.

"Could support old behavior too" is not a reason to continue.

"Could add more tests" is not a reason to continue.

"Could make this more extensible" is not a reason to continue.

"Could make this configurable" is not a reason to continue.

"Could clean up nearby code" is not a reason to continue.

Completion is a terminal state.

---

# Examples

## Example: API Endpoint

Request:

> Add an endpoint that returns the current user's projects.

Good:

```text
route
 -> authenticate user
 -> query projects
 -> return JSON
```

Bad:

```text
route
 -> controller
 -> IUserProjectService
 -> UserProjectService
 -> IProjectRepository
 -> ProjectRepository
 -> ProjectDTOMapper
 -> ProjectResponseFactory
 -> ProjectQueryBus
 -> database
```

unless those abstractions already exist or solve a current problem.

---

# Example: MVP Form

Request:

> Build an MVP signup form.

Reasonable:

- form
- basic validation
- API request
- success/error state

Usually unnecessary:

- generalized form framework
- analytics event architecture
- feature flag framework
- extensive accessibility test suite
- dozens of validation tests
- generic field abstraction system
- elaborate state machine
- configurable provider system

---

# Example: Rename

Request:

> Rename `getWidget()` to `loadWidget()`.

If all callers are inside the repository:

Good:

```text
rename method
update callers
remove old name
```

Bad:

```text
add loadWidget()
keep getWidget()
mark getWidget() deprecated
forward getWidget() to loadWidget()
add tests for both names
document migration path
```

---

# Example: Configuration Rename

Request:

> Change `CACHE_TIME` to `CACHE_TTL`.

If deployments are controlled together:

Good:

```text
CACHE_TTL becomes canonical
repository config updated
CACHE_TIME removed
```

Bad:

```text
CACHE_TTL preferred
CACHE_TIME fallback
warning for CACHE_TIME
support both indefinitely
```

---

# Example: Data Model Change

Request:

> Replace `fullName` with `firstName` and `lastName`.

For an MVP with disposable development data:

Good:

```text
change model
update consumers
update seed data
reset/migrate development DB as appropriate
remove fullName
```

Bad:

```text
keep fullName
add firstName
add lastName
keep fields synchronized
support old payloads
add compatibility serializer
```

---

# Example: Real Compatibility Requirement

Request:

> Rename this API field, but the released mobile app still sends the old field and cannot update for two months.

Appropriate:

```text
accept old field at request boundary
normalize it into the new field
use only new representation internally
remove compatibility after migration period
```

Do not duplicate the entire implementation.

---

# Example: Duplication

Two small handlers contain three similar lines.

Good:

```text
leave them duplicated for now
```

unless the duplicated concept is already clearly stable and meaningful.

Bad:

```text
create HandlerStrategy
create GenericHandlerBase
create shared lifecycle hooks
create registry
```

to eliminate a few repeated lines.

---

# Example: Testing

Request:

> Add a button that toggles the sidebar.

If the behavior is visually obvious and trivial:

Reasonable:

```text
implement toggle
run app
click button
verify sidebar toggles
stop
```

Usually unnecessary:

```text
unit test hook
component test
integration test
browser test
snapshot
accessibility snapshot
coverage update
```

unless current requirements or repository constraints justify them.

---

# Example: Edge Cases

Request:

> Accept a username.

Reasonable:

- reject empty input
- enforce the actual known length/format requirement
- handle duplicates if the system requires uniqueness

Usually unnecessary:

- build a generic Unicode normalization framework
- add configurable username policies
- support historical formats that never existed
- design for federation naming conflicts
- create dozens of speculative validation cases

---

# Example: Performance

Request:

> Show the user's latest 20 messages.

If a straightforward indexed query is sufficient:

Good:

```text
SELECT ...
ORDER BY created_at DESC
LIMIT 20
```

Bad by default:

```text
Redis cache
background cache warmer
event invalidation
replica routing
message materialization layer
benchmark suite
```

without evidence those are needed.

---

# Example: Internal Script

Request:

> Write a script that imports these CSV files.

Prefer:

```text
read file
parse rows
validate required fields
insert rows
print failures
```

Do not automatically add:

- plugin architecture
- job queue
- resumable workflow engine
- metrics server
- dependency injection
- abstract importer hierarchy
- schema compatibility framework
- retry policy framework

unless the actual import requirements need them.

---

# Agent Self-Check

Before finishing a coding task, silently check:

### Scope

Did I implement only what was requested?

Did I modify adjacent code that did not need changing?

### Architecture

Did I introduce abstractions that solve hypothetical rather than current problems?

Did I create extension points before there is anything real to extend?

### Compatibility

Did I preserve old behavior without an actual current consumer or requirement?

### Legacy

Could I update all controlled callers and delete the obsolete path instead?

### Tests

Did every new test provide meaningful confidence relative to its maintenance cost?

Did I test behavior rather than implementation structure?

### Mocks

Did testing convenience cause me to complicate production architecture?

### Dependencies

Could this reasonably have been done without a new dependency?

Did I upgrade anything unrelated?

### Files

Did I split the implementation into more pieces than necessary?

### Infrastructure

Did I introduce runtime or operational components that the current problem does not require?

Do I have evidence the straightforward implementation is inadequate?

### Configuration

Did I create a configurable option even though only one behavior is currently needed?

### Extensibility

Did I add plugin hooks, callbacks, providers, or registries for hypothetical future use?

### Edge Cases

Am I handling likely and meaningful cases, or engineering against invented possibilities?

### Data

Am I protecting important data, or merely preserving obsolete application behavior?

### Validation

Did I perform enough validation to know the requested feature probably works?

### Delete

Is there anything I added—or anything made obsolete by this change—that can now be removed?

### TODOs

Did I leave speculative future-work comments that provide no current value?

### Finish

What exact current requirement remains unsatisfied?

If there is no concrete answer, stop.

---

# Final Directive

BuildFirst optimizes for:

**Working software > engineering ceremony.**

**Current requirements > hypothetical requirements.**

**Vertical slices > infrastructure-first development.**

**One canonical path > layers of compatibility.**

**Changing the code > preserving its history.**

**Simple code > speculative architecture.**

**Concrete duplication > premature abstraction.**

**Current behavior > imagined edge cases.**

**Clear failure > ambiguous fallback.**

**Useful validation > test count.**

**Production architecture > mocking convenience.**

**Evidence > speculative infrastructure.**

**Decisions > unnecessary configurability.**

**Real implementations > hypothetical extension points.**

**Learning quickly > preparing for every possible future.**

**Deletion > accumulation when old code no longer serves a purpose.**

**Reversible mistakes > irreversible complexity.**

**Finishing > polishing forever.**

Build the thing.

Build the smallest complete version.

Change it freely while it is cheap to change.

Remove what is no longer needed.

Validate what matters.

Do not invent more work.

Then stop.
