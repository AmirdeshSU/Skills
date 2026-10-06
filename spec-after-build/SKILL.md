---
name: spec-after-build
description: Reverse-engineer the six specification documents (as-built TRD, backend schema and permission matrix, app flow, PRD, design brief, remediation plan) from an EXISTING webapp or codebase, and find the gaps between what the app does and what it should do. Use whenever the user has an already-built, vibe-coded, inherited or undocumented website/webapp and wants it documented, audited, handed over, cleaned up, made safe to change, or asks "what does this app actually do", "document this codebase", "audit this app", "write specs for my existing site", or "what's wrong with what the AI built". Trigger even if they mention only one document, such as the schema or permission matrix.
---

# Spec After Build

Recover the six documents from a codebase that already exists, then use them to find gaps and plan safe fixes.

## Core rule: Observed vs Intended

Code shows what the app DOES. Only the owner knows what it SHOULD do. If those are merged into one statement, the documents bless bugs as features and look authoritative while recording mistakes. So every document has three parts:

- **Observed**: derived from the code or the running app, with file paths, command output or screens as evidence
- **Intended**: confirmed by the owner
- **Gaps**: where they differ (bug, missing feature, dead code, undocumented behaviour, risk)

Tag every claim: **[Verified]** (read in code or ran it), **[Inferred]** (reasoned from code), **[Unknown]** (could not determine, ask the owner). Never present an inference as a fact.

## Rules of engagement

1. **Read-only.** Documenting is not fixing. Do not change code, config or data during the documentation pass. Fixes come later from the remediation plan.
2. **Evidence or it did not happen.** Cite files, lines or command output.
3. **Run it if you can.** Static reading misses runtime config, feature flags and environment differences. Ask for staging access or sample data.
4. **Protect secrets.** Never read, print or store real secret values, production data or customer records. Inventory environment variable names from `.env.example` or code references only. If a real secret appears in the repo or git history, report it as a Critical finding without reproducing the value.
5. **Review between documents.** Show each document and let the owner correct it before building the next, since errors compound.

## Workflow

### Step 0: Orient
Explore the repo and report: directory map and purpose of each top-level folder, entry points, frameworks and exact versions (from lockfiles), external services, database layer and migrations, auth mechanism, test state, dead or duplicated code. Ask the owner for README, deploy config, any old docs, list of real roles, and whether a running copy exists.

### Step 1: As-built TRD (generate first, most verifiable)
Stack and versions; architecture and background jobs; hosting, CI/CD, environments, who has deploy access; third-party services and what data each receives; every environment variable (name, purpose, required, never value); dependency health (run `npm audit`, `pip-audit` or equivalent and attach output); performance hotspots; logging and monitoring; backups and whether a restore was ever tested; observed security baseline (HTTPS, headers, CORS, CSRF, rate limiting, validation, secrets in code or git history); tech debt.

### Step 2: As-built Backend Schema (highest value)
Entities, fields, constraints, indexes, relationships (cross-check migrations against the real schema export if available); Mermaid ER diagram; **permission matrix, Observed vs Intended**, built by reading the actual authorisation code, not route names; list every endpoint lacking an auth check, ownership check or input validation; endpoint inventory; sensitive-data map (what is stored, encrypted, logged, sent to third parties); what really happens on delete; orphan records and missing constraints; does migration history reproduce production?

Broken access control (one user reading another's data by changing an ID) is the most common serious flaw in AI-built apps. Recommend a manual two-account test of the Observed matrix and have a security-literate person review it.

### Step 3: As-built App Flow
Route and page inventory (frontend and API); per page: purpose, access rules, data loaded, actions and destinations; key journeys traced click by click with file references and walked through in the running app; edge states; dead and orphan routes; broken or half-built flows, TODO/FIXME, feature flags; email and notification triggers.

### Step 4: Reverse-engineered PRD
Inferred purpose and users (marked [Inferred], owner must correct); feature inventory with status (Working, Partial, Broken, Dead, Undocumented), evidence and owner-confirmed priority; acceptance criteria from current behaviour marked keep, change or remove; what users actually use (analytics or logs, not code alone); non-goals; known bugs and complaints; open questions. Do not guess business intent; intent is not in the code and this is the document most likely to be wrong if written without the owner.

### Step 5: Extracted Design Brief
Design tokens actually used (colours, fonts, sizes, spacing, radii, shadows) as a table with usage counts; near-duplicates grouped; component inventory with variants and states; inconsistencies; breakpoints and screens that break; accessibility audit (run Lighthouse or axe if possible); brand assets; a proposed consolidated system as the Intended column for owner approval.

### Step 6: Remediation Plan
A **gap register**: ID, source document, description, severity (Critical, High, Medium, Low), effort, owner. Priority order: access control, secrets and data-loss risks first; then broken flows; then performance; then design consistency; then new features. Phases small enough to ship and verify independently. Safety net before any change: backup, staging, tests on critical flows, rollback plan. Definition of done and verification per task. Rule: update these documents in the same pull request as any change that affects them.

## Deliverables

Default to one document per file or Docs artifact, stored in `/docs` in the repo, plus an index and the gap register as a table. End with a short summary: top five risks by severity, what needs owner decisions, and the recommended first phase.

## Style

Plain, direct, evidence-first. Lead with the worst finding. Do not soften security gaps. If most of a section is [Inferred] or [Unknown], say so at the top of that section.
