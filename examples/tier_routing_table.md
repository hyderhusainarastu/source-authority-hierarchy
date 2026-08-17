# Tier-routing table (synthetic example)

**Everything below is synthetic — invented for illustration. It does not describe any real
project, and none of the artifact names, dates, or figures are drawn from any actual corpus.**

This is an example of what you get when you take `SKILL.md` §0 (placeholders) and §1 (the
claim-type routing table) and fill both in for one illustrative project — call it
"Synthetic Notes App," a made-up documentation-heavy software project used only to show the
shape of the mapping.

---

## Step 1 — Map the placeholders (§0) to this project's actual files

| Placeholder | Concrete artifact in Synthetic Notes App |
|---|---|
| `{{DECISIONS_LOG}}` | `governance/DECISIONS.md` |
| `{{OPEN_QUESTIONS_LOG}}` | `governance/OPEN_QUESTIONS.md` |
| `{{STATUS_DOC}}` | `STATUS.md` (top-level project status page) |
| `{{ASSEMBLED_ARTIFACT}}` | `src/app.ts` and the module tree it currently imports |
| `{{REVIEW_RECORDS_DIR}}` | `reviews/cycle-N/` (one folder per review cycle) |
| `{{IMPLEMENTATION_REPO}}` | the live `synthetic-notes-app` code + test + deploy repository |
| `{{ANALYSIS_DIR}}` | `analysis/feature_matrix/` |
| `{{FUTURE_WORK_DIR}}` | `future_work/` |
| `{{LEGACY_BASELINE_DIR}}` | `legacy/2024_proposal/` |
| `{{CONTRADICTION_LEDGER}}` | `governance/CONTRADICTION_LEDGER.md` |

## Step 2 — Fill in §1's routing table with those concrete artifacts

| # | Claim type | Governing authority for Synthetic Notes App | Tiers that may NOT settle it |
|---|---|---|---|
| **A** | Framing / policy / intended-direction | Newest ruling in `governance/DECISIONS.md`; unresolved items fall through to `governance/OPEN_QUESTIONS.md`. | `analysis/feature_matrix/`, `reviews/cycle-N/`, and any agent's own judgment never overturn an owner ruling here. |
| **B** | Present implementation fact | Live evidence from the `synthetic-notes-app` repo itself — current code, current test run output, current deploy logs. | `STATUS.md` and `governance/DECISIONS.md` are dated summaries; neither substitutes for actually running the system. |
| **C** | Third-party claim | A verified primary source read directly (e.g., the actual external API doc being cited), with a stable locator. | `analysis/feature_matrix/`'s citation index is a pointer list, not evidence, unless a row self-declares a full verified read. |
| **D** | Project-generated empirical datum | Whichever file actually computed the figure and states its own recomputation status — for this project, `analysis/usage_counts.csv`. | Never substitutes for type B or C; never presented as an effect on an external subject. |
| **E** | Claimed outcome on an external subject | Nothing, absent a completed study — for this project no such study exists, so every such claim is `unsupported_open`. | All tiers, in their entirety, until a completed, citable study is filed. |

## Step 3 — Worked example of using the filled-in table

**Claim under review:** "Synthetic Notes App reduces the time users spend searching for a note."

1. Type it: this is an assertion about an effect on real external users → **type E**.
2. Per the filled-in row for E above, nothing settles a type-E claim for this project absent a
   completed study, and `future_work/study_plan.md` (synthetic) shows status `DEFERRED — no data
   collected`.
3. Correct handling: log it in `governance/CONTRADICTION_LEDGER.md` as `unsupported_open` and do
   not assert the time-savings claim anywhere in shipped copy — see `contradiction_ledger.md` in
   this same `examples/` directory for the entry format.

This table is the artifact you produce once, early in a project, so that every later claim can be
typed and routed by table lookup instead of by re-deriving §1 from scratch each time.
