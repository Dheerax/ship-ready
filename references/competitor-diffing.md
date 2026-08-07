# Competitor Surface Diffing

The point of this step is NOT "go build everything a competitor has." It's making the gap between your build and the reference product **visible and explicit**, so the user chooses what to close vs. defer — instead of the gap staying invisible until a real user hits it and complains.

## How to do this efficiently (not vaguely)

1. **Pick 1-2 real, comparable products** — not the biggest name in the space necessarily, but ones that solve a similar problem at a similar scale to what the user is building. (A solo dev's project management tool should look at Linear or Height more than at Jira/enterprise-scale tools — closer scale, more relevant surface.)

2. **Scope the diff to the SPECIFIC feature area**, not the whole product. If the user is building a "team invite" flow, look specifically at how GitHub/Linear/Notion handle team invites — pending vs accepted states, resend, revoke, role assignment at invite time, invite links vs. email invites, expiry. Don't diff the whole product against the whole product; that's too broad to be useful and turns into unfocused scope creep.

3. **List 3-5 concrete things**, not a vague "they have more features." E.g., for an invite flow:
   - They show pending invites separately from active members
   - They let you resend an invite without recreating it
   - They let you set the role at invite time, not just after acceptance
   - They support both "invite by email" and "shareable invite link"
   - They show who sent the invite (accountability/audit trail)

4. **Present as optional, not mandatory.** Each item gets a one-line note: "worth adding now / worth deferring to v2 / not relevant at our scale." The user decides — this step exists to inform the decision, not make it for them.

## Where to get this information
If you have web search available, a quick search for "[competitor] [feature] documentation" or their help-center article on that feature usually surfaces the actual UI/flow without needing screenshots. Their public API docs are also useful — API surface often reveals what operations they support (rename, archive, transfer, etc.) even faster than browsing the UI.

## Calibration note
For early-stage / solo-dev builds, most of the diff items should land in "worth deferring" — that's expected and fine. The value isn't in matching a mature product's surface area on day one, it's in the user making an informed, deliberate choice about what's missing rather than being surprised by it later when a user asks "wait, why can't I do X, doesn't [competitor] have this?"
