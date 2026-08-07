# Settings Taxonomy — Standard SaaS Categories

When a feature touches account, workspace, or team-level configuration, check it against this taxonomy. Not every category applies to every product — mark N/A explicitly with a one-line reason rather than silently omitting.

## 1. Account
Profile info, avatar, display name, email, password/auth method, connected login providers (Google/GitHub OAuth), account deletion, language/locale/timezone preference.

## 2. Security
Two-factor auth, active sessions list (with ability to revoke a specific session/device), login history, API keys/personal access tokens (with scopes and expiry), SSO configuration (for team/enterprise tiers).

## 3. Notifications
Per-channel (email/push/in-app) toggle, per-event-type toggle (not just one global on/off — users want "notify me on mentions but not on every comment"), digest frequency (real-time vs daily digest vs weekly), unsubscribe/mute per-thread or per-project.

## 4. Billing
Current plan, usage against plan limits (with a clear indicator as usage approaches the limit, not just a hard cutoff with no warning), payment method, invoice history/download, upgrade/downgrade flow, cancellation flow (this one especially: don't hide cancellation behind dark patterns — it's both an ethical issue and, in many jurisdictions now, a legal one).

## 5. Integrations / API
Connected third-party services, webhook configuration (URL, events subscribed, secret/signing key, delivery logs with retry), API key management (already partly covered under Security — decide which section owns it and cross-link), rate limit visibility.

## 6. Team & Permissions
Member list, invite flow (pending invites shown separately from active members, with resend/revoke), role assignment, custom roles/granular permissions (advanced — usually not needed until later), transfer ownership, remove member (with a decision about what happens to their created content).

## 7. Danger Zone
Conventionally a visually distinct section (often bordered/colored to stand out) grouping the account/workspace-level irreversible actions: delete workspace, transfer ownership, downgrade with data loss. Grouping these together and visually marking them as high-stakes is itself a UX pattern worth copying — it means a user has to deliberately scroll to and engage with a clearly-marked danger area rather than stumbling into a destructive action mixed in with routine settings.

## How to use this in the spec
List the 7 categories, mark each: Relevant to this feature (with what it implies) / Already exists elsewhere in the app (cross-reference) / N/A for this product with a one-line reason. This is meant to be fast — a table, not an essay.
