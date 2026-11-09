# Agent Rules & Documentation

This repository contains the agent rules and documentation for the development workflow.

## Rules Overview

### Master Rules
- Follow all rules in `.agent-rules/` for every project task
- Safety/security restrictions and platform policies come first
- Agent MUST NOT change any rule, scope, architecture, acceptance criterion, coverage threshold, or recorded decision without the user's explicit approval
- **Checkpoint protocol:** Before moving from planning to design, design to implementation, or implementation/QA to deployment, Agent MUST provide a summary of decisions and disagreements, list created/changed files and tests run, identify risks and open work, and request user approval

### Project Planning
- Create/update `docs/01-plan/` files: `PRD.md`, `scope.md`, `WBS.md`, `roadmap.md`, `risks.md`
- Use the `templates/PRD-template.md` template
- Define SMART goals and success criteria
- Break WBS into small, estimable deliverables
- Maintain a risk register with ID, likelihood, impact, score/priority, trigger, owner, and preventive/contingency action

### Software Design Document (SDD)
- Record design in `docs/02-design/SDD.md` using `templates/SDD-template.md`
- Include architecture view, system and module boundaries, data flow
- API/event contracts with versioning and idempotency
- Record lasting architectural decisions in separate ADRs under `docs/02-design/adr/`
- Do not finalize any technology choice or schema/API change without required authorization

### Test-Driven Development (TDD)
- Follow the strict **Red → Green → Refactor** cycle
- Tests MUST have a clear Arrange–Act–Assert (AAA) structure
- Test names must describe the behavior and expectation
- Coverage target: at least **80% overall coverage** and **95% for critical paths**
- Use mocks only for external/non-deterministic boundaries or slow/costly dependencies
- Before commit, run the relevant tests and the required project suite

### Implementation
- Follow the repository's existing formatter, linter, language version, patterns, and architecture
- Keep changes small, focused, understandable, and reviewable
- Apply SOLID principles and Clean Code appropriately
- Every commit MUST be small and use a Conventional Commit message
- **CRITICAL: Zero-Tolerance Fail-Fast Policy for Steps 1–3:** Each command MUST exit with code 0 before the next step may begin
- Create GitHub Issue first + Gate before starting any work or creating a branch
- Sync Default Branch + Gate before creating a new branch
- Create Linked Branch + Gate with format: `<type>/<issue-number>-<short-description>`

### QA and Testing
- Create/update the project strategy in `docs/04-testing/test-plan.md`
- Implement the test pyramid: Unit (largest), Integration, E2E (fewer tests)
- Every fixed bug MUST have a reproducing test that fails before the fix and passes afterward
- Before merge/release, run relevant tests and an appropriately scoped regression suite
- Severity: **SEV-1 Critical** (data leak/loss, security incident, primary service unavailable), **SEV-2 High**, **SEV-3 Medium**, **SEV-4 Low**

### Deployment, Release, and Coverage
- Create/update the runbook at `docs/04-deployment/runbook.md`
- CI MUST enforce at least 80% overall coverage and 95% for critical paths
- Use Semantic Versioning (`MAJOR.MINOR.PATCH`)
- Checklist: DoD and QA approved; CI green; security/permissions and coverage reviewed
- Document rollback procedures, decision authority, approved steps/commands, data and migration compatibility, backup/restore, communications, and post-rollback smoke check

### Architecture & File Management
- Organize application code by **feature**, not by technical layer
- Canonical top-level structure: `src/app/`, `src/features/<feature>/`, `src/shared/`, `src/config/`, `src/tests/`
- `index.ts` is the ONLY public entry point of a feature
- Cross-feature imports MUST go through `shared/` or the consuming feature's public API
- No circular dependencies; `shared/` stays feature-agnostic
- Naming conventions: Components (`PascalCase.tsx`), Hooks (`useSomething.ts`), Directories (`kebab-case`)
- Environment variables and configuration MUST be read only through a typed config module
- Lint, format, type-check, and tests pass via project scripts
- Adding a dependency requires: checking it is actually needed, an active/maintained, permissively licensed package, no known critical vulnerabilities, and an ADR if it introduces an architectural capability

## Templates

### ADR Template
- Use `templates/ADR-template.md`
- Include: Context and Problem, Decision Factors, Options Considered, Decision, Consequences (positive, negative, risks/costs), Migration/Review Plan and References

### PRD Template
- Use `templates/PRD-template.md`
- Include: Status / owner / date / version, Problem and evidence, Users and needs, SMART goals and success criteria, Functional and non-functional requirements, Scope (in-scope / out-of-scope), Acceptance criteria, Dependencies and assumptions, Risks and constraints, DoD and approvers

### SDD Template
- Use `templates/SDD-template.md`
- Include: Context, Goals, Constraints, Architecture and System Boundaries, Modules and Responsibilities, Data Model and Migration, API / Event Contracts, Key Flows (with Mermaid sequence diagram), Security, Privacy, and Failure Modes, Observability, Performance, and Accessibility, Testing and Deployment, Options, Trade-offs, and Related ADRs

### Test Plan Template
- Use `templates/test-plan-template.md`
- Include: Goals and Out of Scope, Strategy and Layers, Test Cases and Acceptance-Criteria Traceability, Environment, Data, and Prerequisites, Tools and Test Execution, Security, Accessibility, Performance, and Regression, Coverage (minimum 80% overall, 95% for critical paths), Entry/Exit Criteria and Go/No-Go, Bugs, Triage, and Results Reporting

## Workflow

1. **Master Rules** - Follow all rules in `.agent-rules/` for every project task
2. **Project Planning** - Create/update `docs/01-plan/` files using templates
3. **Software Design** - Record design in `docs/02-design/SDD.md` and use templates
4. **Test-Driven Development** - Follow Red → Green → Refactor cycle
5. **Implementation** - Follow implementation rules, create GitHub Issue first, sync default branch, create linked branch
6. **QA and Testing** - Create/update test plan, implement test pyramid, run regression tests
7. **Deployment, Release, and Coverage** - Create/update runbook, ensure CI enforces coverage thresholds

## Enforcement Checklist (before any commit)

- [ ] New files are in the correct feature/layer with correct naming
- [ ] No cross-feature deep imports; features communicate only via barrels/shared/
- [ ] No circular dependencies; `shared/` stays feature-agnostic
- [ ] Config/secrets accessed only via the typed config module
- [ ] Lint, format, type-check, and tests pass via project scripts
- [ ] Dependency additions justified, minimal, and (if architectural) recorded as an ADR

## Communication Rule

Always summarize your status in simple, direct developer terms. Clearly separate What I did, What is blocked, and provide an exact copy-paste approval command if waiting for user sign-off.
