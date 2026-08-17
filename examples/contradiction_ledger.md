# Contradiction ledger (synthetic example)

**All entries below are synthetic — invented for illustration. They do not describe any real
project.**

This is an example of what `{{CONTRADICTION_LEDGER}}` looks like once a project has run a few
claims through the procedure in `SKILL.md` §3–§4. Each row records a conflict whether or not it
was resolved.

---

## CL-01 — RESOLVED

- **Claim:** Whether the "collaborative editing" feature is available to all users.
- **Claim type:** B (present implementation fact)
- **Sources:** `STATUS.md` line 12 vs. `reviews/cycle3_signoff.md` §2.
- **Resolution:** Checked the live implementation repository directly (tier 5). Feature flag
  rollout was 12% at time of check. Neither prior document was correct in isolation.
- **Governing rule:** Tier 5 settles type-B claims; both prior sources were outranked the moment
  a live check was possible (§2 Tier 5; §1 type B).
- **Final wording used:** "implemented but not yet available to all users (12% rollout as of
  synthetic-check-date)."

## CL-02 — RESOLVED

- **Claim:** Whether the project's thesis claims a "cognitive load" reduction or a narrower
  "fewer steps to find information" claim.
- **Claim type:** A (framing/policy)
- **Sources:** `DECISIONS.md` row 22 (older, broader wording) vs. row 40 (newer, narrower
  wording, explicitly striking row 22's phrasing).
- **Resolution:** Row 40 governs — newest owner ruling on a type-A claim beats an older one
  (§3 Step 3).
- **Final wording used:** the narrower claim only.

## CL-03 — OPEN — DECISION REQUIRED

- **Claim:** Whether the synthetic "reader trust" study described in `future_work/study_plan.md`
  produced any usable result.
- **Claim type:** E (claimed outcome on an external subject)
- **Locator 1:** `future_work/study_plan.md`, "Status" line: `DEFERRED — no data collected.`
- **Locator 2:** none — no completed study exists anywhere in the corpus.
- **Why unresolved:** by definition, type E claims with no completed study are `unsupported_open`
  (§1 type E; §2 Tier 7). This is not really "unresolved" so much as settled-negative: there is no
  outcome to report.
- **Resolution test:** none available until a study is actually run and its results filed.
- **Blocking:** true — blocks any deliverable sentence asserting a reader-facing benefit.
- **Owner:** OWNER (a future scope/funding decision, not a factual determination).

## CL-04 — RESOLVED

- **Claim:** The exact count of source documents in the project's citation corpus.
- **Claim type:** D (project-generated empirical datum)
- **Locator 1:** `README.md` line 7: *"64 source documents."* (tier 9, never restatused)
- **Locator 2:** `sources/manifest.csv`: 91 data rows (tier 4, the file that actually computed
  the count, with a header comment stating it is recomputed on every manifest change).
- **Resolution:** `manifest.csv` states its own recomputation status and wins by the tier-4
  preference rule (prefer the file that states its own recomputation status) over the
  never-restatused tier-9 README. Logged per §3 Step 6 — "record the outcome either way," even
  when a rule does settle it cleanly.
- **Governing rule:** tier 4 (manifest, self-declared recomputed) over tier 9 (README, never
  restatused).
- **Final wording used:** "91 source documents (per the current manifest; the top-level README's
  '64' figure is stale)."
