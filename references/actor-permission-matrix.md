# Actor / Permission Matrix

AI defaults to designing for exactly one actor: "the user." Production features almost always have multiple actors whose views and permissions differ. Enumerate them before building.

## Standard actor types to check against any feature

1. **Owner** — full control, including irreversible actions (delete workspace, transfer ownership)
2. **Admin** — most control, usually NOT including transfer-ownership or billing
3. **Member/Editor** — can create and edit their own or shared content, not manage people/billing
4. **Viewer/Read-only** — sees but can't mutate; every mutate-capable button needs a disabled variant for this actor
5. **Removed/revoked actor** — someone who WAS in the system and got kicked out or whose invite was revoked. What do they see if they try their old link? (Should be a clear "you no longer have access" state, not a raw 403 or a crash)
6. **The other side of a two-party flow** — e.g., in an invite system: what does the *inviter* see if the invite expires unaccepted? What does the *invitee* see if they click an expired/already-used invite link? AI usually only builds the sender's happy path.
7. **Unauthenticated visitor** — if any part of the feature has a public-facing surface (shared link, public profile), what does a logged-out visitor see vs. a logged-in non-member?

## Common permission bugs to check for specifically

- **Object-level permission checks missing**: user A can view user B's resource just by guessing/incrementing an ID in the URL, even though the UI never links to it (IDOR — insecure direct object reference). This is the single most common real-world permission bug in AI-generated CRUD apps. Every read/write endpoint needs to check "does THIS user have access to THIS specific resource," not just "is this user logged in."
- **Client-side-only permission checks**: hiding a delete button in the UI for viewers is not the same as blocking the delete request server-side. Both are needed; only the server-side check is actually secure.
- **Stale permission state**: user's role changes mid-session (demoted from admin to member) — does the UI reflect it on next action, or does cached client state let them still see admin-only controls until refresh?

## Output format for this section of the spec

A simple table: rows = actions in the feature, columns = actor types, cells = allowed / view-only / hidden. This makes gaps visually obvious fast — an empty or inconsistent cell is a spec bug, not a coding bug.
