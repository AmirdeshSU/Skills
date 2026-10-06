---
name: spec-before-build
description: Produce the six pre-build specification documents (PRD, technical requirements, app flow, design brief, backend schema, implementation plan) for a NEW website or web app before any code is written. Use whenever the user wants to plan, spec out, scope or brief a website/webapp/SaaS/portal that does not exist yet, mentions vibe coding a business site, asks "what documents do I need before building", asks for a PRD or technical spec for a new product, or wants to hand a build to a developer, freelancer or AI coding tool. Trigger even if they only name one of the six documents, since the others usually follow.
---

# Spec Before Build

Create the six documents that stop an AI-assisted or outsourced build from drifting: PRD, TRD, App Flow, Design Brief, Backend Schema, Implementation Plan.

## Why this matters

AI coding tools fill every unstated decision with a plausible guess. Guesses about features, stack, permissions and data handling are cheap to fix on paper and expensive to fix in code. The documents exist to move those decisions to the person who owns the business before code exists.

Be candid about the limit: AI-drafted specs only help if the owner reviews them. A confident spec full of unconfirmed assumptions is worse than none. So every document ends with an **Assumptions** list, and nothing moves forward until the owner confirms or removes each item.

## Workflow

1. **Intake.** Ask for the business, audience, goal and one success metric, budget, deadline, and who owns accounts. Ask one or two questions at a time, not a questionnaire. If the user already gave answers, do not re-ask.
2. **Draft in this order**, showing each document before starting the next: PRD, TRD, App Flow, Design Brief, Backend Schema, Implementation Plan. Each feeds the next, so errors compound if skipped.
3. **After each document**, list assumptions made and open questions, and wait for approval or corrections.
4. **Deliver** as the user prefers. Default: one Docs artifact or one markdown file per document, plus an index. Do not write application code.
5. **Close** with a handoff checklist (below).

## Decisions only the owner can make

Never invent these. Ask, or mark `[OWNER TO DECIDE]`: business goal and success metric, feature priorities and non-goals, budget, compliance obligations, brand identity, who may see which data, data retention, who owns the domain/hosting/database accounts.

## Document templates

### 1. PRD (what it does)
Goal and success metric; users and their top jobs; features each with description, user, priority (Must/Should/Later), testable acceptance criteria; **non-goals** (prevents scope creep); content the owner must supply; open questions.

### 2. TRD (what it is built on)
Frontend, backend, database, auth, hosting and deploy, third-party services; performance targets; accessibility target (e.g. WCAG 2.2 AA); SEO; security baseline (HTTPS, secrets in environment variables, rate limiting on forms, dependency updates, backups); compliance (privacy policy, cookie consent, regional law); environments and deploy access; monthly cost; rejected options and why. Prefer boring, widely supported choices unless the PRD demands otherwise.

### 3. App Flow (what happens on each click)
Sitemap; per page: purpose, entry points, actions and destinations; key journeys step by step (e.g. visitor to enquiry to confirmation email); edge states (loading, empty, error, 404, form failure, expired session). Offer a Mermaid flowchart of the main journeys.

### 4. Design Brief (how it looks)
Brand personality and 2 to 3 reference sites; logo rules; palette with hex codes and roles; typography and type scale; spacing, radius, shadows; component list with states (hover, focus, disabled, error); imagery and icon style; voice and tone; breakpoints (mobile-first); accessibility rules (contrast, focus outlines, alt text). If the owner has no brand, propose options and let them choose rather than defaulting silently.

### 5. Backend Schema (where data lives, who sees it)
Entities and fields (type, required, unique, constraints); relationships; **role and permission matrix** (who can create/read/update/delete each entity); sensitivity of each field and retention period; auth model (signup, login, reset, session expiry, MFA for admins); validation rules; where secrets live (never in code or the browser); backups, audit log, how a customer's data is exported or deleted; API endpoints with method, path, auth and shapes. Flag the permission matrix for human security review: broken access control is the commonest serious flaw in AI-built apps.

### 6. Implementation Plan (what gets built, in what order)
Phases, each independently shippable and testable (scaffold and deploy a blank page, design system, static pages, forms and database, auth and admin, SEO and analytics, hardening and launch); per task: dependencies, definition of done, how to verify; test plan; security review before launch; launch checklist (DNS, SSL, redirects, analytics, backups verified, privacy policy, form emails delivered); rollback plan; post-launch owner. Instruct the builder to re-read documents 1 to 5 at the start of each phase and report any deviation.

## Handoff checklist

- Owner has confirmed or removed every assumption
- Owner holds all accounts (domain, hosting, database, repository)
- Permission matrix reviewed by someone security-literate
- Owner has a manual test list they can run themselves
- Documents are stored somewhere the builder will actually read them (repo `/docs` is best)

## Style

Plain language, short sentences, tables where comparing. Tag uncertain statements `[Assumption]`. Keep each document to what a builder needs; do not pad.
