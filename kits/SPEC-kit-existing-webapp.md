# The SPEC Kit for an Already-Built Webapp

Goal: recover the six documents from a codebase that already exists, then use them to find gaps, fix drift, and make future changes safe.

## Core rule
Code tells you what the app DOES. Only a human knows what it SHOULD do. Every document below has:
- **Observed:** derived from the code, with file/line references as evidence
- **Intended:** confirmed by the owner
- **Gaps:** anywhere the two differ (bug, missing feature, dead code, undocumented behaviour)

Never let the AI merge Observed and Intended into one statement.

Confidence tags for every claim the AI writes: **[Verified]** (seen in code or ran it), **[Inferred]** (reasoned from code), **[Unknown]** (could not determine, ask owner).

---

## Step 0: Prepare (30 min)
1. Put the repo where Claude can read it (upload, or run Claude Code in the repo).
2. Collect: README, `.env.example`, deployment config (Dockerfile, CI files, hosting settings), database migrations, any old docs, analytics/error logs, list of real users and roles.
3. Get the app running locally or in staging. A spec derived from reading alone will miss runtime behaviour.
4. Never paste real secrets, production data or customer records into the AI. Use `.env.example` and sample data.

**Master prompt (run first):**
"Explore this repository without changing anything. Produce: (1) directory map and purpose of each top-level folder, (2) entry points, (3) frameworks and versions, (4) external services referenced, (5) database layer and migrations location, (6) auth mechanism, (7) test coverage state, (8) anything dead, duplicated or suspicious. Tag every claim [Verified], [Inferred] or [Unknown] and cite file paths."

Then generate the six documents in this order (different from greenfield: technical truth first, because it anchors the rest).

---

## 1. Technical Requirements Document (as-built)

**Generate first.** It is the most verifiable from code.

**Sections**
- Stack and exact versions (from lockfiles), runtime, build tooling
- Architecture: monolith/services, how frontend talks to backend, background jobs, queues, cron
- Hosting, CI/CD pipeline, environments, deploy process, who has access
- Third-party services and APIs, with what data goes to each
- Configuration: every environment variable (name, purpose, required?, never the value)
- Dependency health: outdated, deprecated, known-vulnerable packages (run `npm audit` / `pip-audit` etc. and attach output)
- Performance: bundle size, slow endpoints, N+1 queries, caching
- Observability: logging, error tracking, monitoring, alerts
- Backups and disaster recovery: do they exist, were they ever restored?
- Security baseline observed: HTTPS enforcement, headers, CORS, CSRF, rate limiting, input validation, secrets in code or git history
- Tech debt list

**Prompt:** "Write the as-built TRD using this repo. For each section give Observed facts with file references. Add a Gaps section listing risks and missing basics. Do not suggest fixes yet."

**Owner confirms:** hosting accounts and ownership, budget, compliance obligations, what was deliberately chosen vs accidental.

---

## 2. Backend Schema (as-built)

**Sections**
- Entities, fields, types, constraints, indexes, foreign keys (from migrations or ORM models, cross-checked against the real database schema export)
- Entity relationship diagram (Mermaid)
- Role and permission matrix: **Observed** (what the code enforces) vs **Intended** (what should be enforced)
- Where authorisation is checked: middleware, per-route, database-level rules; list routes that have none
- API endpoint inventory: method, path, auth required, input validation, response shape, who calls it
- Sensitive data map: which fields hold personal, financial or credential data, whether encrypted, logged, or sent to third parties
- Retention and deletion: what actually happens when a user or record is deleted
- Data integrity issues: orphan records, missing constraints, inconsistent enums
- Migration state: does the migration history reproduce the production schema?

**Prompt:** "Extract every data model and every API route. Build the permission matrix by reading the actual authorisation code, not the route names. List every endpoint that lacks an auth check, ownership check or input validation. Mark each finding [Verified] or [Inferred]."

**Why this is the high-value document:** broken access control (one user reading another's data by changing an ID) is the most common serious flaw in AI-assisted apps. Have a security-literate person review the Observed matrix, and test it by hand with two accounts.

---

## 3. App Flow (as-built)

**Sections**
- Route/page inventory (frontend routes plus API routes)
- Per page: purpose, entry points, actions, destinations, data loaded, who can access
- Key user journeys, reconstructed from the code AND walked through in the running app
- Edge states: loading, empty, error, session expiry, offline, invalid input, 404
- Dead pages and orphan routes (exist in code, no link leads to them)
- Broken or incomplete flows (buttons that do nothing, TODOs, half-built features)
- Email/notification triggers

**Prompt:** "List every route and the components it renders. For each journey, trace click-by-click from the code and note the file for each step. List dead routes, unreachable pages, TODO/FIXME comments and features behind flags."

**Owner confirms:** which flows matter most to the business and which "features" are experiments to delete.

---

## 4. Product Requirements Document (reverse-engineered)

**Sections**
- Inferred product purpose and user types (Inferred, owner must correct)
- Feature inventory: each feature with status (Working / Partial / Broken / Dead / Undocumented), evidence, and owner-confirmed priority
- Acceptance criteria written from current behaviour, then marked as keep / change / remove
- Features users actually use (from analytics or logs, not from code alone)
- Non-goals: what the app will not do
- Known bugs and user complaints backlog
- Open questions

**Prompt:** "From the code and route inventory, list every user-facing feature. For each, state what it does today, its completeness, and how I can verify it. Do not guess business intent. Put guesses in an Open Questions section."

**Owner confirms:** the actual goal and success metric. This is the document most likely to be wrong if the AI writes it alone, because intent is not in the code.

---

## 5. Design Brief (extracted)

**Sections**
- Design tokens extracted from CSS/Tailwind config/theme files: colours, fonts, sizes, spacing, radii, shadows
- Component inventory: buttons, forms, modals, tables, nav, with variants and states
- Inconsistencies: near-duplicate colours, one-off font sizes, components built three different ways
- Responsive behaviour: breakpoints in use, screens that break
- Accessibility audit: contrast, keyboard navigation, focus states, labels, alt text, ARIA misuse (run Lighthouse or axe and attach results)
- Brand assets in the repo
- Proposed consolidated design system (the Intended column), owner-approved

**Prompt:** "Extract all colours, font settings, spacing values and component styles actually used in the codebase. Group near-duplicates. Output as a token table with usage counts, then list components with their variants."

---

## 6. Implementation Plan (remediation and roadmap)

For an existing app this is not a build order. It is a prioritised plan to close the gaps from documents 1 to 5.

**Sections**
- Gap register: ID, document, description, severity (Critical / High / Medium / Low), effort, owner
- Priority order: security and data-integrity issues first, then broken flows, then performance, then design consistency, then new features
- Phases, each small enough to ship and verify independently
- Safety net before changing anything: tests for critical flows, a backup, a staging environment, a rollback plan
- Definition of done per task and how it is verified
- Documentation upkeep: update these six docs in the same pull request as any change that affects them

**Prompt:** "Using documents 1 to 5 and the gap lists, create a gap register and a phased remediation plan. Put anything affecting access control, secrets, data loss or payments first. Each task needs a verification step."

---

## Process rules
1. **Evidence or it did not happen.** Every claim cites a file, a command output or a screen you saw.
2. **Review each document yourself** before feeding it into the next prompt. Errors compound.
3. **Do not fix while documenting.** Documentation pass is read-only. Fixes happen in phase work from document 6.
4. **Run it, do not just read it.** Static analysis misses runtime config, feature flags and environment-specific behaviour.
5. **Keep docs in the repo** (`/docs`) and update them with each change, or they will be stale within a month.

## Quick deliverable checklist
- [ ] Stack, versions, hosting, env var inventory
- [ ] Dependency and vulnerability scan output attached
- [ ] ER diagram and endpoint inventory
- [ ] Observed vs Intended permission matrix, tested with two real accounts
- [ ] Route and journey map with dead code listed
- [ ] Feature inventory with status and owner priority
- [ ] Token table and accessibility report
- [ ] Gap register with severities and a phased plan
- [ ] Backup verified and rollback documented before phase 1 starts
