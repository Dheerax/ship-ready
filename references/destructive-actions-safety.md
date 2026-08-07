# Destructive Action Safety — 3-Tier Friction Model

Not every destructive action deserves the same friction. Matching friction to stakes is itself a design skill — too much friction on low-stakes actions annoys users; too little on high-stakes actions causes real damage and erodes trust. AI defaults to either no confirmation at all, or the same generic "Are you sure?" for everything. Neither is right.

## Tier 1 — Low stakes, easily reversible
Examples: removing an item from a list view, un-favoriting, archiving (not deleting) something.
- Friction: none, or a simple undo toast ("Removed. Undo") for a few seconds after the action
- No modal needed — modals for low-stakes actions train users to click through confirmations mindlessly, which defeats the purpose when a real high-stakes one appears

## Tier 2 — Medium stakes, reversible with effort or a time window
Examples: deleting a single item that goes to a "trash" for 30 days, removing a team member (they can be re-invited), disconnecting an integration.
- Friction: confirmation modal with a clear description of what happens ("This will be moved to trash and permanently deleted in 30 days")
- Include a way to reverse it before the window closes (a "Trash" or "Recently deleted" view)

## Tier 3 — High stakes, irreversible or hard to undo
Examples: permanently deleting a workspace/account, revoking all API keys, transferring ownership, deleting a resource with no soft-delete/trash step, actions with billing/financial consequences.
- Friction: type-to-confirm (user must type the resource's name or a confirmation phrase, not just click a button) — this is the GitHub repo-delete pattern, and it exists specifically because a single click on a scary-looking button is something users do accidentally
- Explicitly state consequences that aren't obvious (e.g. "This will also cancel your active subscription" or "All 14 team members will lose access immediately")
- Consider requiring re-authentication (password/2FA re-entry) for the very highest stakes actions (changing account email, deleting the account, changing billing)

## Anti-patterns to avoid regardless of tier
- A confirmation modal with no information beyond "Are you sure?" — restate what specifically will happen and what can't be undone
- Destructive action and its confirm button both styled the same as a normal button (the confirm button in a destructive modal should visually read as dangerous — typically red/warning styling — and the safe/cancel option should be the visually default one)
- Toasts for undo that disappear before a user could reasonably read and click them (5 seconds minimum, ideally longer or persistent until dismissed)
- Silently cascading deletes (deleting a project also deletes its 40 associated tasks) without telling the user what else goes with it

## Output format for this section of the spec
For each destructive action in the feature, assign a tier and note the specific friction mechanism. One line per action is enough.
