# State Matrix

For every screen or component tied to a feature, check it against these 7 states. AI defaults to building only #4 (success). Production software handles all 7.

## 1. Empty state (zero data, not an error)
First-time user, or user cleared everything out. This is a **conversion moment**, not an afterthought — it should explain what the thing is and prompt the first action, not just say "No items."
- Bad: `<p>No projects</p>`
- Good: icon/illustration + "You haven't created a project yet" + primary CTA button + optionally a link to docs/example

## 2. Loading state
Distinguish **initial load** (skeleton screens, not spinners, for anything with known layout) from **action-in-progress** (button shows spinner + disables itself, doesn't just hang).
- Skeleton for list/table initial load
- Inline spinner + disabled state for button actions
- Never let a user double-submit a form because the button didn't disable

## 3. Error state — split into three, they need different UI
- **Validation error** (client-side, before submit): inline, field-level, shown on blur or submit attempt — never just a top-of-page red banner listing "3 errors" with no field indication
- **Network/server error** (submit failed): toast or inline banner with a retry action, not a dead end. Preserve the user's input — never clear a form because the save failed
- **Not found / gone** (resource deleted by someone else, expired link): distinct page/state, not a generic error — "This project was deleted" reads very differently from "Something went wrong"

## 4. Success state
The happy path AI builds by default. Still worth checking: does success give feedback (toast, visual confirmation) or does the UI just silently update? Silent success reads as broken to users who aren't sure their action registered.

## 5. Partial-data state
Data that's mid-sync, mid-migration, or has some fields present and others null/pending (e.g. an OAuth-imported profile with no avatar yet, a webhook that hasn't fired yet). AI often assumes full objects; production data is messy and incomplete more often than not.

## 6. Permission-denied state
User can see something exists but can't act on it (viewer role hitting an edit button), vs. user can't see it exists at all (404 vs 403 — these should usually look identical from a security standpoint to avoid leaking existence, but internally the code path differs). Also: what does a disabled button look like, and does it explain *why* it's disabled (tooltip: "Only owners can delete this")?

## 7. Zero-results-after-filter state
Different from state 1 (empty state) — the user HAS data but their filter/search returned nothing. This needs "no results match 'xyz' — clear filters" not the same empty-state illustration as a brand-new account.

## Quick self-check when speccing a screen
Ask: "If I open this screen right now with (a) no data, (b) a slow connection, (c) a failed request, (d) as a lower-permission user, (e) with a search filter active that matches nothing — does each look intentional, or does it look broken?"
