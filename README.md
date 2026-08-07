# ShipReady 🚀

A structured system and constraint framework for AI-assisted software engineering. This skill forces AI coding models (such as Claude) to design and write **production-ready, edge-case-hardened code** instead of defaulting to simple, happy-path-only prototypes.

---

## 💡 Why This Exists

When asked to build a feature, AI assistants default to the **Happy Path**—the primary user flow that looks good on the first pass. However, real-world production software (like GitHub, Stripe, Linear, and Vercel) feels polished because it handles the thousands of edge cases that arise in real usage.

As a solo developer or small team, you don't have the years of user feedback loops to discover these edge cases. **ShipReady** acts as a programmatic proxy, forcing the AI to systematically map out and code for all states, permissions, and security guardrails *before* writing a single line of feature code.

---

## ⚙️ How It Works (The Gate)

This skill establishes a strict **"Spec-First Gate."** 
When loaded, it instructs the AI: **Do not write code first.** Instead, the AI must present a compact **Feature Surface Spec** for approval. 

This spec acts as a checklist, allowing you to explicitly choose what is in scope for v1 and what is deferred, rather than the AI silently omitting features you didn't think to ask for.

---

## 🛠️ The 7 Pillars of the Spec

The Feature Surface Spec evaluates your proposed feature against seven engineering dimensions:

### 1. State Matrix 📊
Ensures every screen handles more than just the success state. It checks for:
*   **Empty States:** What does a user see before any data exists? (Calls-to-action, templates).
*   **Loading States:** Skeletons, disabled buttons, and spinners to indicate active processes.
*   **Error States:** Distinct handling for network failures, input validation failures, and server crashes.
*   **Success & Partial Data:** Success notifications and grids with only one item.

### 2. Actor & Permission Matrix 🔑
Defines who can interact with the feature and how:
*   Maps roles (Owner, Admin, Member, Viewer, Guest).
*   Solves edge cases like revoking permissions or mid-session role changes.

### 3. Lifecycle & CRUD Completeness 🔄
Expands simple "Create & List" features into complete lifecycle structures:
*   **Full CRUD:** Create, Read, Update, Delete, Archive, Restore, Rename, Duplicate, Export, and Transfer Ownership.

### 4. Destructive Action Safety ⚠️
Prevents accidental data loss by enforcing friction:
*   **Tier 1:** Undo toasts for quick recovery.
*   **Tier 2:** Confirmation dialogs for non-destructive removals.
*   **Tier 3:** Hard confirmation (e.g. typing a project name) for permanent deletes or transfers.

### 5. Settings Taxonomy Check ⚙️
Integrates workspace/account features into standard SaaS categories:
*   Determines what the feature implies for Account, Security, Notifications, Billing, and Danger Zones.

### 6. Competitor Surface Diff ⚖️
Forces a comparison against 1-2 leading real-world implementations to identify missing usability details (e.g., how Linear handles project archiving).

### 7. Anti-Pattern Self-Check 🚫
A final audit step to eliminate common "AI-generated" bugs:
*   Ensures forms have field-level validation, buttons have loading states, and tables are paginated.

---

## 📂 Directory Structure

Once extracted, the skill contains the following reference guides:
```
ShipReady/
├── SKILL.md                          # The core prompt and gate configuration
├── README.md                         # This file
└── references/
    ├── state-matrix.md               # Visual state recipes and code patterns
    ├── actor-permission-matrix.md    # Multi-tenant and permission checks
    ├── lifecycle-crud.md             # CRUD+ entity checklist
    ├── destructive-actions-safety.md # 3-Tier friction model for deletions
    ├── settings-taxonomy.md          # SaaS settings mapping check
    ├── competitor-diffing.md         # Framework for diffing against real apps
    └── anti-patterns.md              # AI code quality check list
```

---

## 🚀 How to Use

### 1. In Custom GPTs / Claude Projects
Upload the `SKILL.md` file and the `references/` folder into your custom AI project files or custom system prompt guidelines.

### 2. In Cursor / Windsurf / Copilot
Add the contents of `SKILL.md` or a path pointer to your `.cursorrules` or system prompt guidelines.

### 3. By Manual Trigger
Whenever you begin building a feature (e.g., "Add user workspace settings"), type:
> *"Load ship-ready skill and generate the spec before coding."*

---

## ⚖️ Calibration (V1 vs. Bloat)

The purpose of this system is **visibility, not over-engineering**. The spec is designed to make the gaps visible so you can make informed decisions. If a feature is too complex for v1, simply label it **"Deferred for v1"** inside the spec and proceed with a clean, scoped build.
