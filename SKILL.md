---
name: source-authority-hierarchy
description: "Decides which source governs a claim: type the claim, rank eligible sources on a nine-tier authority ladder, newest within tier, and log unresolved contradictions. Use when sources disagree or a claim needs a citation."
---

# Source Authority Hierarchy

A procedure for deciding **which source governs a claim** when a project accumulates layers of
documentation that disagree with each other — decision logs, status pages, compiled
deliverables, review records, analysis directories, legacy proposals, and READMEs, each updated
on its own schedule and none of them reliably telling you it has gone stale.

This skill does not decide any claim itself. It is a *decision procedure*: given a claim and a
set of candidate sources, it tells you which source(s) may settle it, in what order, and what to
do — record, not guess — when nothing settles it.

## Core principle

Two failure modes dominate multi-document projects:

1. **Picking the newest file** regardless of what kind of claim it is, when a newer *governance*
   document does not outrank an older *code/test* fact, and a newer *opinion* does not outrank an
   unresolved earlier *ruling* on a different question.
2. **Picking the most convenient file** — the one whose wording is easiest to reuse — instead of
   the one that actually has authority over that claim type.

The fix is two independent axes applied in order:

- **Axis 1 — Claim type.** What *kind* of claim is this? (§1 below). This determines which tiers
  are even eligible to settle it.
- **Axis 2 — Tier, then recency within tier.** Among the eligible tiers, rank by tier first, then
  by date *within* the winning tier (§2 below). A newer low-tier source never beats an older
  high-tier one on the claim type the high tier governs.

Never collapse these into one axis. A brand-new file in a low tier is still a low-tier source. An
old file in the top tier is still top-tier until a newer entry in that same tier supersedes it.

## When to use this skill

- Two or more sources in a project disagree about the same fact, status, or claim.
- You are about to write a claim into a deliverable (a report, a paper, a spec, a status page)
  and need to know which of several candidate sources may back it.
- A "living" status document (a project wiki page, a top-level status file, a README) needs to
  be checked for staleness against a decisions/changelog before being trusted.
- A project wants a standing, repeatable rule for logging contradictions instead of silently
  resolving them in whichever direction is easiest to write.
- Someone asks "is X still true?" in a project where X was asserted in more than one place, on
  more than one date.

## §0. Placeholders used below

This skill describes a *shape*, not a fixed set of file names. Substitute your own project's
actual documents. A concrete mapping might look like:

| Placeholder | Example concrete artifact |
|---|---|
| `{{DECISIONS_LOG}}` | `DECISIONS.md`, a changelog, a governance ledger |
| `{{OPEN_QUESTIONS_LOG}}` | `OPEN_QUESTIONS.md`, an issue tracker's "open/blocked" view |
| `{{STATUS_DOC}}` | `STATUS.md`, `PROJECT_STATUS.md`, a project wiki home page, `CONTRIBUTING.md` |
| `{{ASSEMBLED_ARTIFACT}}` | the file that is actually built/compiled/shipped — `main.tex`, `index.ts`, a build manifest — and the parts it currently includes |
| `{{REVIEW_RECORDS_DIR}}` | a directory of review sign-offs, audit reports, decision dossiers for the current work cycle |
| `{{IMPLEMENTATION_REPO}}` | the live code/test/deployment repository (may be a different repo entirely from the one holding the documents above) |
| `{{ANALYSIS_DIR}}` | a directory of framing analysis, claim/feature matrices, limitations write-ups |
| `{{FUTURE_WORK_DIR}}` | a directory describing proposed-but-not-done work |
| `{{LEGACY_BASELINE_DIR}}` | an older proposal, plan, or audit from a prior phase of the project |
| `{{CONTRADICTION_LEDGER}}` | the file where you record every conflict this procedure finds |

Keep placeholder names stable across a project so every agent brief can cite the same table.

## §1. Claim-type routing table — apply this BEFORE the tier ladder

The hierarchy is **claim-type-sensitive**. The first move on any assertion is to type it. A tier
that governs one claim type is not authority for another — a governance document can tell you
*what was decided*, but it can never substitute for *what the code actually does*, and neither
can tell you anything about *an effect on a real external person*.

| # | Claim type | Test question | Governing authority | Tiers that may NOT settle it |
|---|---|---|---|---|
| **A** | **Framing / policy / intended-direction** — what the project argues, what it is trying to do, what may be said, what is in scope | "Is this a choice the owner/author made?" | Newest explicit owner ruling in `{{DECISIONS_LOG}}`, then unresolved items in `{{OPEN_QUESTIONS_LOG}}`. **Tier 1.** | Tiers 5–9 may not overturn an owner ruling. An agent's own judgment never overrides an owner ruling. |
| **B** | **Present implementation fact** — "the system does X right now" | "Would actually running the system settle it?" | Live implementation evidence — canonical logs/status, current code, current tests, in `{{IMPLEMENTATION_REPO}}`. **Tier 5.** | **No governance statement substitutes for this** — not the status doc, not the decisions log, not any analysis directory. All of those are *dated summaries* and are outranked the moment a live check is possible. |
| **C** | **Third-party claim** — what an external published source actually says, a locator, a quotation, a citation-level gap | "Is this a claim about someone else's published work?" | A verified primary source, read directly, with a stable locator (page/section/URL/DOI). Do not cite a search snippet or an index entry as evidence. | Metadata-only surveys, discovery sweeps, and annotation directories are *pointers*, not evidence, unless they self-declare a full verified read. |
| **D** | **Project-generated empirical datum** — a count, rate, or figure computed from this project's own records | "Is the denominator one of this project's own files?" | The file that computed it, **with its recomputation status stated**, plus whatever prohibition/caveat list it carries. | Never a substitute for type B or C. Never presentable as an effect on an external subject. |
| **E** | **Claimed outcome on an external subject** — a benefit, effect, preference, or measurement involving real people or real external systems outside the project's own control | "Does this assert an effect on someone/something the project does not directly control or has not directly measured?" | **Nothing settles this** unless a completed, citable study or measurement exists. Absent that, every such claim is `unsupported_open` or `proposed_test`. | All nine tiers, in their entirety, if no completed study/measurement exists. A design argument, a plan, or an intention is not an outcome. |

**Cross-type collision rule.** A later owner ruling governs framing/policy (type A); current
code/tests govern present implementation facts (type B). **Record any conflict between them
rather than using one claim type to overwrite the other.** A type-A ruling that presupposes a
type-B fact does not make that fact true — log both halves, and route the type-B half to whoever
owns the implementation-verification lane.

If you cannot type the claim cleanly into one of A–E, it usually decomposes into more than one
sub-claim — split it and type each part separately before proceeding.

## §2. The nine tiers

Apply this ladder only within the claim type(s) a tier is eligible for (§1). Each tier below
states: what it is, what it may and may not settle, and its characteristic recency hazard — the
specific way sources at that tier tend to go stale without saying so.

---

### TIER 1 — Latest owner-authored rulings, then unresolved open questions

**What it is:** the newest dated ruling made by the actual owner/author of the project (not an
agent, not a moderator, not a reviewer) in `{{DECISIONS_LOG}}`, and — for anything not yet
ruled on — the still-open items in `{{OPEN_QUESTIONS_LOG}}`.

**Settles:** claim type A only. Also settles which *other* rows in the same log are superseded,
when a ruling says so on its own face.

**Does not settle:** claim types B, C, E. A ruling can *propose* a substantive claim; it cannot
make external evidence support it. If a ruling's own status field says something like
"proposal — awaiting confirmation" or a later row records that part of it was struck, treat only
the surviving part as binding.

**Recency hazard:** decision logs are almost always append-only — new rows get added, old rows
do not get re-statused. A row can still read `ACTIVE` while a much later row silently supersedes
it. **Read the whole log, newest row last, and check every row still marked active against every
later row before relying on it.** Rows authored by an agent, moderator, or reviewer (as opposed
to the actual owner) are not tier-1 rulings even if they sit in the same log — they are tier-2
governance at best, and any owner row can overturn them.

---

### TIER 2 — Current-state/governance doc, cross-checked against later dated decisions

**What it is:** the project's living status/governance document (`{{STATUS_DOC}}`) — the page
meant to summarize "where things stand right now."

**Settles:** the *inventory* of governance (which rulings exist, which document holds what) and
general project history.

**Does not settle:** claim type B at all. A status document is a governance statement, and no
governance statement substitutes for current code/test evidence (§1, type B). It also does not
settle anything where a later-dated entry exists in `{{DECISIONS_LOG}}` — **do not assume the
status doc's summary is newer than the last decision-log row just because you read the status
doc more recently.**

**Recency hazard — the sharpest in this ladder.** Status documents routinely interleave current
and stale sections in one file with no consistent marker: some blocks are labeled `[CURRENT]` or
`[ARCHIVED]`, others carry no staleness marker at all and simply describe an old state next to a
correctly current one. **Procedure: never quote a status document for a number or a status
without either finding a later-dated primary record, or actively confirming none exists.**

---

### TIER 3 — The artifact actually assembled by the build/compile/publish process

**What it is:** whatever a project's real entry point currently pulls in — the manuscript a
`\input`/`\include` chain actually compiles, the modules an app's entry file actually imports,
the pages a site's build manifest actually publishes.

**Settles:** what the assembled artifact currently contains and argues, and the claim
discipline the artifact is required to hold to (for example, a stated boundary like "this claims
no effect that has not been measured").

**Does not settle:** whether the artifact is *correct* — and does not settle claim type B. A
component of the artifact may itself describe an external system; that description still needs
type-B verification against tier 5, not against the artifact's own text.

**Recency hazard:** **do not treat a retired or unreferenced component as current merely because
it still exists on disk.** A commented-out `\input`, a removed import, a deleted route
registration — the file can remain, fully formed, on the filesystem for a long time after it
stopped being part of what actually ships. Check the entry point's *current* inclusion list, not
the directory listing. Conversely, watch for the assembled artifact being *newer* than the
governance records that describe it — a compiled output's timestamp can post-date every document
that claims to summarize it.

---

### TIER 4 — Current-cycle research, review, and decision records

**What it is:** the working documents produced during the current cycle of work —
`{{REVIEW_RECORDS_DIR}}`: review sign-offs, audit reports, decision dossiers, thematic
syntheses, subject-area survey notes, source/citation annotations.

**Settles:** cycle state, review verdicts, corpus/inventory counts, and what analysis was
actually performed.

**Does not settle:** claim type C (a survey or metadata-level review is not the same as a
verified read of the underlying source — treat it as a pointer unless it explicitly says it did
a full verified read) and not claim type B.

**Reconstructed-record caveat:** if a record in this tier states it is a reconstruction assembled
after the fact (for example, because the original artifact of a review was never actually filed),
treat its *finding text* as evidence but treat its *dispositions* (fixed/open/pending) as needing
a fresh check against current state — exactly as such a record should say to do, if written
carefully.

**Recency hazard:** within this tier, files of the same date can still disagree with each other.
**Prefer the file whose own header states that it recomputed a figure over one that merely cites
another file for it.** A file that says "every figure below is either recomputed by this agent
from a file on disk, or attributed to the file that computed it, with that file's recomputation
status stated" is the model to follow when writing your own tier-4 records.

---

### TIER 5 — Live implementation evidence, for present-implementation-fact claims only

**What it is:** the canonical log/status output, current code, and current tests of
`{{IMPLEMENTATION_REPO}}` — the actual running (or actually-building) system, as opposed to any
document that describes it.

**Settles:** claim type B, and *only* claim type B. **These outrank every dated research-side or
documentation-side capability summary**, no matter how recent that summary is, the moment a live
check is actually possible.

**Does not settle:** claim types A, C, E. A shipped feature does not by itself license a claim
about an effect on an external subject.

**Recency hazard:** any capability-summary document sitting outside the implementation repo is a
**dated snapshot of a system that keeps moving**. Treat every such document as
`implemented_unverified` until someone actually re-checks it against tier 5, with a fresh access
date. If you (the agent doing this analysis) do not have access to the implementation repo in
your current task, **say so explicitly** — record every capability claim you encounter as a
dated claim requiring live verification, never assert it as settled fact, and route the
verification to whoever's lane does cover that repo.

---

### TIER 6 — Framing and historical-baseline analysis, not proof of live implementation

**What it is:** `{{ANALYSIS_DIR}}` — claim/feature matrices, "what does this argument imply"
write-ups, limitations documents, and similar framing analysis, taken as a research-framing and
historical-status baseline.

**Settles:** permitted/prohibited wording for specific claims, and the general framing baseline.

**Does not settle:** claim type B — this tier's own implementation-status columns or statements
are frequently the *oldest* status data in the whole project and are outranked by tier 5 the
moment a live check exists.

**Recency hazard:** a wording rule from this tier can remain **binding by later owner ruling**
even after the file that stated it has gone stale on every other axis — separate "is this wording
rule still in force" (often yes, because tier 1 re-affirmed it) from "is this status column still
accurate" (often no). Do not let staleness on one axis contaminate your judgment on the other.

---

### TIER 7 — Directories describing possible future work, not completed work

**What it is:** `{{FUTURE_WORK_DIR}}` — proposed studies, planned experiments, roadmap notes,
anything explicitly framed as not-yet-done.

**Settles:** that no completed work of that kind exists yet, and that nothing in this directory
may be cited as if it were a finding.

**Does not settle:** anything about actual outcomes. Nothing here may upgrade a type-E claim from
`unsupported_open`/`proposed_test` to established.

**Recency + wording hazard:** language written for a proposed future study easily reads, out of
context, as a claim about a demonstrated present effect (for example, describing a hoped-for
mechanism as "superior" rather than "proposed"). Anyone lifting phrasing from this tier into a
deliverable must re-apply the tier-6 wording law — a proposed mechanism is a theoretical mapping,
never a demonstrated effect, until type-E evidence exists.

---

### TIER 8 — An older proposal or audit from a prior phase — a legacy baseline to reconcile

**What it is:** `{{LEGACY_BASELINE_DIR}}` — a proposal, plan, or audit produced before the
current cycle of work began, often in a different repository or workspace entirely.

**Settles:** nothing on its own. Every section of a legacy document requires an explicit
disposition — reuse it, revise it, reject it, defer it, or verify it against current tiers —
with an exact locator and a stated reason, before any of its content may be reused.

**Does not settle:** anything current. It predates whatever reframing, rebuild, or decision cycle
produced the current tiers 1–4.

**Recency hazard:** it is typically the *oldest* and often the *most detailed* material in the
whole project — precisely the combination that invites unearned reuse. Treat "the legacy document
already covered this in more depth" as a warning sign, not a shortcut. Copying a legacy document
without reconciling it against the current tiers is a standing, named failure mode.

---

### TIER 9 — READMEs and historical plans

**What it is:** navigation-oriented documents — top-level READMEs, early planning documents —
useful for finding your way around, not for current facts.

**Settles:** navigation and the historical shape of the process. If a superseded early plan
uniquely still supplies a definition later records cite by name (a gate definition, a phase
name), treat that specific defining section as promoted to tier-4-by-adoption wherever a later
record cites it — but nothing else in the same file gets that promotion.

**Does not settle:** any count, status, or current-state fact.

**Recency hazard:** these files are essentially never restatused. Assume every number in them is
stale; verify before use.

---

## §3. Tie-break and recency procedure — run this on every current-state assertion

**Step 0 — Type the claim** against §1's routing table. If type B and you do not have access to
`{{IMPLEMENTATION_REPO}}` in your current task, stop: record the research-side claim as dated,
not as fact, and route verification to whoever's lane covers that repo. If type E and no
completed study/measurement exists, mark `unsupported_open` or `proposed_test` and stop.

**Step 1 — Collect every locator.** Not the newest file — *every* file that speaks to the claim.
Use whatever inventory the project maintains (a file list with paths and modification times) so
you are not relying on memory of what exists.

**Step 2 — Read the date the file asserts about itself, not only its filesystem modification
time.** Two clocks commonly disagree: the date a section header claims for itself, and the date
the file was last written to disk. A file can be modified on one date to append a note that is
itself dated differently from the append event, or a heading can claim a date that predates a
line elsewhere in the same file saying the whole file was updated later. **The in-file asserted
date governs interpretation; the modification time is corroboration only, not the deciding
signal.**

**Step 3 — Rank by tier, then by date within the tier.** A newer tier-9 file never beats an older
tier-1 ruling. A newer tier-1 ruling always beats an older tier-1 ruling on a type-A claim.

**Step 4 — Apply the routing-table's governing rule in order:**
- 4a. Newest explicit owner ruling → governs intended direction and policy.
- 4b. No governance statement substitutes for current code/test evidence.
- 4c. Live implementation records outrank dated research/documentation-side capability summaries
  — for type B only.

**Step 5 — Check for a self-declared supersession before concluding a file is current.** Projects
that record supersession *inside* the superseded file (rather than by deleting it) tend to use a
small, recurring set of patterns. Check for all of them before trusting a file's headline claim:
- a boxed or highlighted status notice at the top of the file;
- an appended, separately dated note at the bottom (often literally labeled "note," "update," or
  "addendum"), which may explicitly say the body text above it was left unaltered;
- a bracketed marker inside a heading (`[ARCHIVED]`, `[SUPERSEDED]`, a "superseded as of DATE"
  callout);
- struck-through or bracketed text inside a table cell;
- a comment near the top or bottom of a structured source file (a build config, a `.tex`
  preamble) explaining why a section was disabled;
- **a terminal negative-results, limitations, or residual-gaps section that qualifies the file's
  own headline finding.** This is the pattern most likely to be missed, because it requires
  reading to the end of a long file rather than stopping once the headline claim is found. A
  file's summary section can state its *widest* finding up top and its *actual bounds* only in a
  closing section — reading only the summary and stopping produces a false reading of what the
  file actually supports. **Operational rule: a negative claim or a scope-limiting claim taken
  from any survey, audit, or discovery-style document is not established until that document's
  own negative-results/limitations/residual-gaps section (if it has one) has been read in full.**
  This applies to discovery sweeps, subject-area reviews, audit reports, and build reports alike
  — anywhere a document might state a broad top-line result and a narrower actual scope.

**Step 6 — Record the outcome in the contradiction ledger** at `{{CONTRADICTION_LEDGER}}`, with
**both** locators and both dates, whether or not a rule settles it. Record the outcome either
way — a resolved conflict is still worth a permanent record of why it resolved the way it did.

**Step 7 — If no source settles it, mark it exactly as described in §4.**

## §4. What to do when no source settles a conflict

**Mark it exactly:** `OPEN — DECISION REQUIRED` (use this literal string, or your project's
equivalent literal marker, consistently everywhere).

**Then, in the same ledger row, supply all five of:**

1. **Both locators and both dates** — file plus line/section/row for each side of the conflict.
2. **Why the ladder does not resolve it** — name the specific reason: same tier and same date;
   different claim types (the collision rule in §1 forbids using one to overwrite the other); the
   deciding record is a reconstruction; the deciding record's own text says it is not current; or
   the deciding evidence sits outside your current task's lane entirely (for example, in a repo
   you were not asked to open).
3. **A resolution test** — the single cheapest concrete observation that would settle it: a
   specific file to open, a specific count to recompute, a specific person to ask a specific
   question.
4. **The blocking status** — `blocking: true|false`, and which downstream gate/milestone it
   blocks if true.
5. **The owner** — a named party responsible for resolving it, or `OWNER` where the missing piece
   is a value or scope choice only the project's actual owner can make.

**Four things never to do:**

- Never pick the convenient side just because it is easier to write around.
- Never let the agent doing this analysis settle a genuinely open conflict substantively — record
  it and route it; do not decide it in the agent's own voice.
- Never let a later summary silently absorb an open conflict without a record — a project that
  loses a review artifact this way (someone summarizes over a gap without noting the gap existed)
  has lost real information, not just formatting.
- Never resolve a type-B conflict using only research/documentation-side sources when the actual
  implementation repo is reachable and simply has not been checked yet.

**Escalation ladder** (adapt the specific roles to your project):
findings return first to whichever worker/owner is responsible for the affected area; after
repeated failed attempts to resolve the same defect, a different specialist is assigned; genuine
disagreement between two workers routes to an independent adjudicator with no stake in either
side; anything still unresolved, or that requires a value/scope choice rather than a factual
determination, goes to the project's actual owner.

## §5. Standing cautions (recurring hazards worth checking for explicitly)

| Caution | Why it recurs |
|---|---|
| A file's main body can be superseded by its own later, separately-dated appended note — read to the end, not just the top | append-in-place is the dominant supersession pattern in most long-lived project documents |
| Any capability-summary document sitting outside the actual implementation repo is a **dated contradiction index**, not a source of fact | it was accurate the day it was written and the underlying system kept moving |
| Search/discovery-sweep documents are **search maps and dated negative-findings records only** — a "not found" result records something about the search, not about the world | a miss on one pass is evidence about the search method, not proof of absence |
| A document written throughout in conditional voice ("would," "could," "is designed to") describes an **aspirational** state, not a built one | conditional-voice documents are easy to misquote in present tense once excerpted |
| Do not infer a present capability from a pitch, concept brief, screenshot, planned-but-unbuilt issue, or historical proposal | these documents describe intent, not verified present state, even when confidently worded |
| Two clocks exist in almost every long-lived project (in-file asserted date vs. filesystem modification time) and they will disagree somewhere | mechanical edits (formatting passes, moves, re-saves) touch modification time without touching the content's actual as-of date |

## §6. Worked mini-procedure (apply end to end)

1. State the claim in one sentence.
2. Type it against §1 (A–E). If it splits into more than one type, split it and repeat this
   procedure for each part separately.
3. Identify which tier(s) in §2 are eligible for that claim type.
4. Within the eligible tiers, collect every locator that speaks to the claim (§3 Step 1).
5. For each locator, read its self-asserted date and check it for a self-declared supersession
   pattern (§3 Steps 2 and 5) before trusting its headline statement.
6. Rank by tier, then recency within tier (§3 Step 3), and apply the governing-rule order
   (§3 Step 4).
7. If settled, write the claim with the winning source as its locator, using whatever status
   vocabulary your project requires for that claim type.
8. If not settled, or if you skipped verification because a needed repo/lane was out of scope for
   your current task, write it into `{{CONTRADICTION_LEDGER}}` per §4, with all five required
   fields, and do not assert the claim as fact anywhere else in your output.

See `references/worked_example.md` for a fully worked synthetic example applying every step
above, and `examples/` for a synthetic contradiction-ledger entry
(`examples/contradiction_ledger.md`) and a synthetic tier-routing table filled in for an
illustrative project (`examples/tier_routing_table.md`).
