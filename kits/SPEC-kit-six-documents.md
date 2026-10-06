# The SPEC Kit: Six Documents Before You Build

Fill these in order. Each document feeds the next. Anything marked **YOU DECIDE** must come from a human who owns the business, not from the AI.

Suggested workflow: draft each with Claude, then review it yourself, then paste the approved version into the next prompt as context.

---

## 1. Product Requirements Document (PRD)

**Purpose:** What the site does and for whom. Feature by feature. Nothing about tools yet.

**What goes in it**
- Business goal (one sentence) and how you will know it worked (e.g. 20 enquiries/month)
- Target users and their top 3 jobs-to-be-done
- Feature list, each with: description, who uses it, priority (Must / Should / Later), acceptance criteria
- Explicit **non-goals** (what you are NOT building in v1)
- Content you must supply (copy, photos, pricing, legal pages)
- Open questions

**Template**
```
Product: 
Goal: 
Success metric: 
Users: 
  - User type 1: needs...
Features:
  F1. [Name] — Priority: Must
      Description:
      Who uses it:
      Acceptance criteria: (testable, e.g. "form rejects invalid email and shows an error")
Non-goals:
Open questions:
```

**YOU DECIDE:** the goal, the priorities, the non-goals. If you skip non-goals, the build will grow.

**Prompt for Claude:** "Interview me one question at a time about my business website, then draft a PRD using this template. Flag anything you had to assume."

---

## 2. Technical Requirements Document (TRD)

**Purpose:** The stack and constraints, so nobody guesses mid-build.

**What goes in it**
- Framework, language, hosting, domain/DNS, email provider, payment provider
- Database and auth provider
- Third-party services and APIs (analytics, CRM, forms, maps)
- Performance targets (e.g. load under 3s on mobile), browser/device support
- Accessibility target (e.g. WCAG 2.2 AA)
- SEO requirements (meta, sitemap, structured data)
- Security baseline: HTTPS, secrets handling, dependency updates, rate limiting on forms, backups
- Compliance: privacy policy, cookie consent, GDPR/DPDP or other regional laws
- Environments: local, staging, production; who has deploy access
- Monthly running-cost estimate and who pays

**Template**
```
Frontend: 
Backend/API: 
Database: 
Auth: 
Hosting & deploy: 
Third-party services:
Performance targets:
Accessibility:
Security baseline:
Compliance:
Environments:
Monthly cost estimate:
Rejected options and why:
```

**YOU DECIDE:** budget, compliance obligations, who owns the accounts (domain, hosting, database must be in YOUR name, not the builder's).

---

## 3. App Flow

**Purpose:** Every path a visitor can take, page by page.

**What goes in it**
- Sitemap (all pages)
- For each page: purpose, entry points, main actions, where each action leads
- Key user journeys (e.g. visitor → services → enquiry form → thank-you → confirmation email)
- Logged-in vs logged-out behaviour, if any
- Edge states: empty states, loading, errors, 404, form failure, expired session

**Template**
```
Sitemap:
Page: [Name]
  Purpose:
  Reached from:
  Actions:
    - Click X → goes to Y
    - Submit form → success: Z / failure: error shown inline
  Edge states:
Journey 1: [Name] — step 1 → step 2 → ...
```

**Tip:** Ask Claude for a Mermaid flowchart of the journeys and check it against your real customer behaviour.

---

## 4. Design Brief

**Purpose:** Keeps every screen on brand.

**What goes in it**
- Brand personality (3 adjectives) and 2–3 reference sites you like and why
- Logo files and usage rules
- Colour palette with hex codes and roles (primary, secondary, background, text, error, success)
- Typography: fonts, sizes, weights, line heights for headings/body/buttons
- Spacing scale, border radius, shadows
- Components: buttons, forms, cards, nav, footer, with states (hover, focus, disabled, error)
- Imagery and icon style, tone of voice for copy
- Responsive rules: mobile-first breakpoints
- Accessibility: minimum contrast, focus outlines, alt text rule

**Template**
```
Brand personality:
References:
Colours: primary #______ (use: ), secondary #______ ...
Fonts: Headings: ___ / Body: ___
Type scale:
Spacing & radius:
Components & states:
Imagery/icon style:
Voice & tone:
Breakpoints:
Accessibility rules:
```

**YOU DECIDE:** everything brand-related. If you leave it to the AI you get a generic template look.

---

## 5. Backend Schema

**Purpose:** Where data lives, its shape, and who can touch it. This is the document most often skipped and most often the cause of data leaks.

**What goes in it**
- Entities (tables/collections) and fields with types, required/optional, unique constraints
- Relationships (one-to-many, etc.)
- Roles (public visitor, customer, staff, admin) and a permission matrix: who can create / read / update / delete each entity
- Data classification: which fields are personal or sensitive, how long they are kept, how they are deleted
- Auth model: sign-up, login, password reset, session expiry, MFA for admin
- Validation rules and input sanitisation
- Secrets and API keys: where stored (environment variables, never in code or front end)
- Backups, audit logging, and how to export/delete a customer's data on request
- API endpoints: method, path, auth required, request/response shape

**Template**
```
Entity: Enquiry
  id (uuid, pk)
  name (text, required)
  email (text, required, validated)
  message (text, required, max 2000)
  created_at (timestamp)
  Sensitivity: personal data. Retention: 24 months.
Permissions:
  Entity     | Visitor | Customer | Staff | Admin
  Enquiry    | create  | -        | read  | read/update/delete
Endpoints:
  POST /api/enquiries — public, rate-limited, validated
Auth:
Backups & retention:
Audit log:
```

**YOU DECIDE:** who sees client data and how long you keep it. Have a security-literate person review the permission matrix before any code is written.

---

## 6. Implementation Plan

**Purpose:** What gets built, in what order, and how you verify each step.

**What goes in it**
- Phases with milestones, each independently testable (e.g. 1: scaffold + deploy "hello world"; 2: design system; 3: static pages; 4: forms + database; 5: auth/admin; 6: SEO/analytics; 7: hardening and launch)
- Per task: description, dependencies, files/areas touched, definition of done
- Test plan: what you click through manually, plus automated tests for forms and permissions
- Security review checkpoint before launch
- Launch checklist: DNS, SSL, redirects, analytics, backups verified, privacy policy live, 404 page, form emails delivered
- Rollback plan
- Post-launch: monitoring, who fixes bugs, update cadence

**Template**
```
Phase 1: [Name] — Done when: 
  Task 1.1: ... depends on: ... verify by: ...
Phase 2: ...
Pre-launch checklist:
Rollback plan:
Post-launch owner:
```

**Rule for AI builds:** one phase at a time. Ask the AI to re-read documents 1–5 at the start of each phase and to report anything in the code that deviates from them.

---

## Master prompt

"I am building a business website. Act as a product and engineering lead. Work through these six documents in order: PRD, TRD, App Flow, Design Brief, Backend Schema, Implementation Plan. For each: ask me the questions you need, one at a time, draft the document using the template I give you, list every assumption you made, and wait for my approval before moving to the next. Do not write any code."

## Before you hand this to a builder
1. You own all accounts (domain, hosting, database, repo).
2. Every assumption the AI listed has been confirmed or removed.
3. The permission matrix and the launch checklist have been reviewed by someone who understands security.
4. You have a test list you can run yourself.
