# Anti-Patterns — "AI Demo" Tells vs Production Tells

Concrete, checkable signals — not vibes — that distinguish demo-grade from production-grade output. Scan your own plan/code against this list before calling a feature done.

## UI/UX tells

- **No disabled/loading state on buttons that trigger async actions** → users double-click, causing duplicate submissions/records
- **Forms that clear on failed submit** → user loses their input and has to retype everything
- **Generic error messages** ("Something went wrong") with no actionable next step or retry
- **No empty states** — a blank screen or a bare "No data" with nothing else on first use
- **Unbounded lists with no pagination/virtualization plan** — works fine with 10 items in a demo, breaks or lags with 10,000 in production
- **Success with no feedback** — an action completes but nothing visually confirms it, so users click again unsure if it worked
- **Same-styled confirm and cancel buttons on destructive modals** — no visual signal for which action is dangerous
- **Toasts/confirmations that vanish too fast to read**, especially ones tied to undo actions

## Data/logic tells

- **Client-side-only validation or permission checks** with nothing enforced server-side — trivially bypassed via direct API calls
- **IDOR-shaped endpoints**: fetching `/api/resource/:id` with no check that the requesting user actually owns/can-access that specific id
- **No handling for "the resource this refers to was deleted by someone else"** — stale references crash or silently misbehave instead of showing a clear state
- **Hardcoded assumption of complete data** — code that assumes every field is populated, breaks on the first record with a null optional field
- **No rate limiting or abuse consideration** on anything public-facing (signup forms, invite endpoints, public API routes)

## Structural tells (the ones that compound over time)

- **Bespoke UI per feature instead of shared primitives** — every new modal, toast, or form is built from scratch instead of reusing a system; this is the single biggest reason AI-built apps stay feeling inconsistent as they grow, because polish applied to one screen never propagates to the others
- **No shared permission-check utility** — every endpoint reimplements "is this user allowed" slightly differently, guaranteeing inconsistent bugs
- **Settings/config values hardcoded in components** instead of centralized, making later changes (a copy tweak, a limit change) require hunting across files

## How to use this list
Before considering a feature done, read through this list once against what you actually built. Anything that matches → either fix it or explicitly note it as a known/accepted gap for v1. The goal is that gaps are chosen, not accidental.
