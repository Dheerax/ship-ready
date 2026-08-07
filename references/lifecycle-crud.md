# Lifecycle / CRUD Completeness Checklist

For every entity a feature introduces (a "project," "team," "API key," "post" — anything a user creates and later manages), AI defaults to building Create + Read(list). Production entities usually need most of this list. Not all entities need all operations — but check each one explicitly rather than silently skipping.

## The checklist

1. **Create** — with validation, with sensible defaults, with a clear success path (redirect to the new thing, not just back to the list)
2. **Read (single)** — a detail/show view, not just a row in a table
3. **Read (list)** — with pagination or virtualization once counts grow; a list that fetches all rows unpaginated is a scaling bug waiting to happen
4. **Update** — inline edit vs. separate edit page/modal (pick based on field count — 1-2 fields = inline, many fields = dedicated view)
5. **Delete (soft)** — mark as deleted/archived, hidden from default views, recoverable
6. **Delete (hard)** — actually gone, usually gated behind soft-delete + a waiting period, or an explicit "permanently delete" action separate from the everyday delete
7. **Archive / Restore** — distinct from delete: "I'm done with this but might want it back," vs delete's "get rid of this." Archived items usually still count against limits/quotas; deleted ones don't.
8. **Rename** — often forgotten as a separate concern from general "update," but worth calling out because renaming often has side effects (slugs/URLs changing, references elsewhere needing updates)
9. **Duplicate/Clone** — common request once users have built something they want to reuse as a template
10. **Transfer ownership** — who becomes responsible for this entity if the creator leaves/is removed? Orphaned entities are a common production bug
11. **Export** — CSV/JSON export of the entity or its data; often a "nice to have" that becomes a support-ticket-generator if missing (users want their data out)
12. **History / Audit log** — who changed what, when. Not needed for every entity, but expected for anything with team/shared access or compliance implications

## How to use this in the spec

List the entities the feature introduces, then for each one mark which of the 12 operations are: Included in v1 / Deferred (explicit, with a one-line reason) / Not applicable. This turns "I forgot delete" into "we decided delete isn't needed for this entity because X" — a decision instead of a gap.

## Common real gaps this catches

- Projects/workspaces that can be created but never deleted or archived (users then ask "how do I get rid of this" — a support ticket that a delete button would have prevented)
- No way to leave a team/workspace once joined, only ways to be removed by an admin
- No rename after creation, forcing delete + recreate to fix a typo in a name
- Export requested by users specifically because they're worried about lock-in — a missing export feature is a common churn trigger for data-heavy products
