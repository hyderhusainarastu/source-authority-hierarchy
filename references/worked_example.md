# Worked example (synthetic)

**All names, dates, figures, and documents below are synthetic — invented for this example. They
do not describe any real project.**

The example project is "Aurora Notes," a fictional note-taking research tool. It has the usual
layered documentation:

- `DECISIONS.md` — the decisions log (tier 1)
- `STATUS.md` — the living status page (tier 2)
- `app/main.ts` — the entry point that actually determines what ships (tier 3)
- `reviews/cycle3_signoff.md` — a current-cycle review record (tier 4)
- the `aurora-notes-app` implementation repository — live code and tests (tier 5)
- `analysis/claim_matrix.csv` — framing/wording analysis (tier 6)
- `future_work/study_plan.md` — a proposed-but-not-run study (tier 7)
- `legacy/2025_proposal.md` — an older proposal from a prior phase (tier 8)
- `README.md` — navigation (tier 9)

## Claim under test

> "Aurora Notes supports real-time collaborative editing."

## Step 0 — Type the claim

This asserts something the running application either does or does not do right now. Test
question from §1: "would actually running the system settle it?" Yes → **claim type B**.

## Step 1 — Identify eligible tiers

Type B is settled only by tier 5 (live implementation evidence). Every other tier that mentions
this feature is, at best, a dated summary requiring verification.

## Step 2 — Collect every locator

- `STATUS.md`, line 12: *"Real-time collaborative editing: **shipped**, GA since March."*
- `reviews/cycle3_signoff.md`, §2: *"Collaborative editing is built and on behind a feature
  flag; not yet promoted to all users."* (dated one month after `STATUS.md`)
- `analysis/claim_matrix.csv`, row `RTC-01`: `implementation_status = "in progress"` (dated two
  months before `STATUS.md`)
- `legacy/2025_proposal.md`: describes collaborative editing throughout in conditional voice
  ("the app **would** allow multiple editors...") — a pre-build proposal, not a status claim.

## Step 3 — Read self-asserted dates, not just file-modification time

`STATUS.md`'s "shipped, GA since March" line sits in a block with no staleness marker, in a file
whose own top line says it was last updated two weeks ago — a classic tier-2 hazard (§2, Tier 2
recency hazard). The `cycle3_signoff.md` review is dated *after* that top-line update, despite
`STATUS.md` appearing more recently touched in the filesystem's modification-time listing (two
clocks disagree — §5).

## Step 4 — Rank by tier

`analysis/claim_matrix.csv` (tier 6) and `legacy/2025_proposal.md` (tier 8) are both outranked
by `STATUS.md` (tier 2) and `cycle3_signoff.md` (tier 4) for a type-B claim — but *none* of these
four settle it, because **no governance statement substitutes for live implementation evidence**
(§1, type B; §2 Tier 5). The only source that can settle this claim is tier 5.

## Step 5 — Check tier 5

This worked example's task did not have access to the `aurora-notes-app` implementation
repository. Per §3 Step 0 and §2 Tier 5: **do not assert the claim as fact.** Record it as a
dated research-side claim requiring live verification.

## Step 6 — Record the conflict

Because `STATUS.md` and `cycle3_signoff.md` disagree with each other (GA-to-everyone vs.
behind-a-flag-not-yet-promoted) *and* neither is tier 5, this is exactly the situation §4
describes: no source in this task's reach settles it.

### Resulting `{{CONTRADICTION_LEDGER}}` entry

```
id: CL-EX-01
claim: "Aurora Notes supports real-time collaborative editing for all users."
claim_type: B (present implementation fact)
status: OPEN — DECISION REQUIRED
locator_1: STATUS.md line 12 ("shipped, GA since March") — file top-line date: two weeks ago
locator_2: reviews/cycle3_signoff.md §2 ("built and on behind a feature flag; not yet
  promoted to all users") — dated one month after STATUS.md's top-line date
why_unresolved: both candidate sources are tier 2 / tier 4 governance-and-review documents;
  neither is tier 5 live implementation evidence, and no governance statement may substitute
  for it (§1 type B). The implementation repository was out of this task's lane.
resolution_test: check the live feature-flag configuration and current test suite in the
  aurora-notes-app repository for the collaborative-editing flag's current rollout percentage.
blocking: true — blocks any deliverable that states this capability as fact.
owner: whichever task/agent has access to the aurora-notes-app implementation repository.
```

## Step 7 — What ships in the actual deliverable

The deliverable using this claim writes, at most:

> "Aurora Notes' status documentation describes real-time collaborative editing as shipped;
> a more recent internal review describes it as behind a feature flag and not yet promoted to
> all users. This was not independently verified against the live application and should be
> confirmed before being stated as fact." *(implemented_unverified, pending live check)*

Never the flat, unhedged claim "Aurora Notes supports real-time collaborative editing."

---

## A second, quicker example — a type-A claim

**Claim:** "Aurora Notes' research argues that structured notes reduce cognitive load."

**Step 0:** "Is this a choice the owner/author made about what the project argues?" Yes →
**type A**.

**Step 1:** Only tier 1 (`DECISIONS.md`, then `OPEN_QUESTIONS.md`) settles this.

**Step 2–3:** `DECISIONS.md` row 40 (newest row touching this topic): *"OWNER-14: reframe the
thesis to a NARROWER claim — 'structured notes reduce the number of steps needed to find
information,' not 'reduce cognitive load' generally. The broader claim is STRUCK pending
evidence."* An earlier row 22 had used the broader "cognitive load" phrasing.

**Step 4:** Tier 1, newest row wins over an earlier tier-1 row on a type-A claim (§3 Step 3). No
other tier — including `analysis/claim_matrix.csv` at tier 6, even if it still uses the old
phrasing — may overturn this (§1, type A: "an agent ruling never overrides an owner ruling").

**Step 5:** No supersession-pattern check needed beyond what row 40 already states on its face;
row 40 explicitly says what it strikes.

**Outcome:** the deliverable must use the narrower framing ("reduces the number of steps needed
to find information") and must not use "reduces cognitive load" as the project's own claim,
regardless of what any older analysis document still says. If an older tier-6 document is quoted
for historical framing, it must be labeled as superseded phrasing, not current framing.
