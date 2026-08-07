---
name: ship-ready
description: Forces production-grade feature depth instead of AI's default happy-path-only output. Use this skill EVERY TIME the user is building, speccing, or asking you to add a feature to a real product/app/website — not just when they explicitly say "make it production ready." Trigger on requests like "build a settings page," "add a dashboard," "create user accounts," "add a projects feature," "build X like Github/Stripe/Linear," or any request to build/extend a SaaS-style app, admin panel, or multi-user product. Also trigger when the user complains their app "feels thin," "feels like a demo," "isn't as filled out as [competitor]," or asks how to reach production-grade UX as a solo dev. This skill BLOCKS you from writing feature code until you've produced a Feature Surface Spec — do not skip the spec step even if the user's request seems simple, because simple-seeming requests (e.g. "add a delete button") are exactly where AI defaults to happy-path-only and misses the depth that makes software feel production-ready.
---

# ShipReady

## Why this skill exists

Left alone, AI code generation defaults to the **happy path**: the primary flow, looking good, first pass. Production software (GitHub, Stripe, Linear, Firebase) isn't deeper because a bigger AI was used — it's deeper because thousands of real users hit edge cases over years and someone built for each one. As a solo dev you don't have that feedback loop yet. This skill is a substitute forcing-function: it makes you (Claude) systematically enumerate the surface area a real user/team would eventually demand, BEFORE writing code, instead of only after someone complains.

**The core failure mode this prevents:** building the "create + list" version of a feature and calling it done, when production-grade means create/read/update/delete/archive/restore/transfer, four different actor permissions, six different UI states, and a confirmation flow for anything destructive.

## The Gate (read this part every time)

When the user asks you to build, add, or spec any feature for a real multi-user product — **do not jump to code.** First produce a **Feature Surface Spec** (format below). Show it to the user. Let them cut anything that's out of scope for v1 — that's their call, not yours. But the enumeration has to happen first, on the record, so scope is a *choice* they made rather than a *gap* neither of you noticed.

Exceptions — skip the gate for:
- Pure styling/CSS/visual polish requests with no new functionality
- Bug fixes to existing features
- Prototypes explicitly framed as throwaway/experimental ("just a quick mockup," "for a demo tomorrow")
- The user says "skip the spec, just build it" — respect that, but mention once that you can run the full pass later

## Feature Surface Spec format

For the feature in question, produce this as a compact table/checklist in chat (not a giant essay):

### 1. State Matrix
Every screen tied to this feature gets checked against: empty state, loading state, error state (network fail, validation fail, server fail — these are different), success state, partial-data state, permission-denied state, zero-results-after-filter state. Read `references/state-matrix.md` for the full checklist and copy-paste-able patterns.

### 2. Actor / Permission Matrix
Who besides the primary user touches this? Owner vs. admin vs. member vs. viewer vs. removed-user vs. the "other side" of a two-party flow (e.g. someone whose invite was revoked). Read `references/actor-permission-matrix.md`.

### 3. Lifecycle / CRUD Completeness
For every entity introduced (a "project," a "team," a "key"), check: create, read, update, delete, archive, restore, rename, duplicate, transfer ownership, export, view history/audit log. Most AI output stops at create+list. Read `references/lifecycle-crud.md`.

### 4. Destructive Action Safety
Anything that deletes, revokes, or transfers needs a safety layer — confirmation modal, type-to-confirm for high-stakes actions, soft-delete/undo window before hard-delete. Read `references/destructive-actions-safety.md` for tiering (which actions need which level of friction).

### 5. Settings Taxonomy Check (only for account/workspace-level features)
Run the feature against the standard SaaS settings categories to see what it implies elsewhere: Account, Security, Notifications, Billing, Integrations/API, Team & Permissions, Danger Zone. Read `references/settings-taxonomy.md`. Mark N/A explicitly rather than silently skipping — an explicit "N/A because X" is a decision; a silent gap is a bug waiting to happen.

### 6. Competitor Surface Diff
Name 1-2 comparable real products. List 3-5 things they have in this exact feature area that this build doesn't yet. This isn't "go build all of it" — it's making the gap visible so the user chooses to close it or explicitly defer it. Read `references/competitor-diffing.md` for how to do this diff efficiently instead of vaguely.

### 7. Anti-Pattern Self-Check
Before finalizing, scan your own plan against `references/anti-patterns.md` — a list of the specific tells that make AI-built features read as "demo" rather than "production" (e.g. toasts that vanish before destructive actions are undoable, forms with no field-level validation, lists with no pagination/virtualization plan, buttons with no disabled/loading state).

## After the spec: build in this order

1. Core primitives first if they don't exist yet: toast/notification system, modal/dialog system, form validation pattern, confirm-dialog component, permission-check hook/util. Building these once means every subsequent feature reuses them instead of being bespoke — this is the actual mechanical difference between "vibe coded" and "production," more than any individual feature's polish.
2. Happy path for the feature.
3. States from the matrix, in order of likelihood a real user hits them (empty state and error state usually before exotic permission states).
4. Destructive-action safety layer.
5. Settings/taxonomy hooks if applicable.

## Calibration — don't overbuild either

This skill is about **enumeration**, not maximalism. Not every feature needs all seven sections deep. A solo dev building a v1 doesn't need audit logs and transfer-ownership flows for every entity. The point is that the *user* decides what's in scope after seeing the full list — not that you silently decided for them by only showing the happy path. When in doubt, mark something "Deferred — flag if this becomes a support request" rather than building it preemptively or omitting it silently.

## Quick reference index

- `references/state-matrix.md` — the 7 states, with concrete examples per state type
- `references/actor-permission-matrix.md` — actor types, common permission bugs
- `references/lifecycle-crud.md` — full CRUD+ checklist per entity type
- `references/destructive-actions-safety.md` — 3-tier friction model for destructive actions
- `references/settings-taxonomy.md` — standard SaaS settings categories with sub-items
- `references/competitor-diffing.md` — how to do a fast, useful competitor surface diff
- `references/anti-patterns.md` — concrete tells of "AI demo" vs "production" code
