# source-authority-hierarchy

A reusable procedure for deciding **which source governs a claim** when a project has many
documents that quietly disagree with each other — a decisions log, a status page, a shipped
artifact, review records, analysis notes, an old proposal, a README — each updated on its own
schedule, none of them reliably flagging that it has gone stale.

## What it does, in plain language

Big projects accumulate layers of documentation. Left unmanaged, two things go wrong:

1. Someone trusts whichever file they happened to read most recently, even though a newer status
   page can still be wrong about something an older decision already settled.
2. Someone trusts whichever file is most convenient to quote, rather than the one that actually
   has authority over that kind of claim.

This skill fixes both by applying two independent checks, in order, to every claim:

- **What kind of claim is it?** A policy choice, a fact about what the software currently does,
  a claim about someone else's published work, a number computed from the project's own records,
  or a claimed effect on real people/systems outside the project's control? Each type has a
  different, narrower set of sources that can actually settle it.
- **Among the sources that are even eligible, which one wins?** A nine-tier ladder (owner
  decisions and open questions at the top, README files and old plans at the bottom) ranks
  eligible sources, then breaks ties by date *within* the winning tier — never across tiers.

When nothing settles a conflict, the skill also defines exactly how to record that: not a shrug,
not a guess, but a structured "OPEN — DECISION REQUIRED" entry with both sources, both dates, why
the ladder didn't resolve it, what the cheapest way to resolve it would be, whether it's blocking,
and who owns getting it resolved.

This is domain-agnostic. It was distilled from a research-proposal project's practice of
reconciling a research corpus against a live application codebase, but the same shape applies to
any project where documentation, decisions, and a running system can drift apart — software
projects, editorial workflows, policy documents, or any multi-document research effort.

## Installation

This is a portable [Agent Skill](https://www.anthropic.com/engineering/equipping-agents-for-the-real-world-with-agent-skills) following the `SKILL.md` convention. To use it:

- **Claude Code:** copy this folder into `.claude/skills/source-authority-hierarchy/` in your
  project (or your user-level skills directory to make it available everywhere).
- **Any other agent runtime that supports the `SKILL.md` convention:** copy this folder into that
  runtime's equivalent skills directory. The `SKILL.md` file is self-contained — no bundled
  script, no external dependency — so nothing else needs to be installed.

Once installed, an agent should invoke it whenever two documents disagree, whenever a claim needs
a citation and more than one candidate source exists, or whenever a "living" status document
might be out of sync with a decisions log.

## Worked usage example (synthetic)

See [`references/worked_example.md`](references/worked_example.md) for a full walkthrough using a
synthetic fictional project ("Aurora Notes," invented for illustration — not a real product). It
runs two claims end to end:

- A **present-implementation-fact** claim ("does the app support real-time collaborative
  editing?") where the status page and a review record disagree, and neither is the live
  implementation repository — so the correct output is *not* an unhedged claim, but a hedged,
  logged one.
- A **framing/policy** claim ("does the project's thesis claim a cognitive-load reduction, or a
  narrower claim?") where a newer decision-log row explicitly supersedes an older one.

[`examples/contradiction_ledger.md`](examples/contradiction_ledger.md) shows what the resulting
ledger looks like across several entries — both resolved conflicts (with the rule that resolved
them) and a genuinely open one waiting on a future decision.

A short version of the routing logic:

```
1. State the claim in one sentence.
2. Type it: framing/policy, present-implementation-fact, third-party claim,
   project-generated datum, or claimed external outcome.
3. Identify which tier(s) of the ladder are even eligible to settle that type.
4. Collect every source that speaks to the claim -- not just the newest one.
5. Read each source's own self-asserted date, and check it for a
   self-declared supersession note before trusting its headline claim.
6. Rank by tier, then by date within the winning tier.
7. If settled: write the claim, citing the winning source.
   If not settled: log it as OPEN -- DECISION REQUIRED with both locators,
   both dates, why the ladder didn't resolve it, a resolution test, blocking
   status, and an owner.
```

No script is bundled with this skill — it is a reasoning procedure an agent follows directly, not
a tool it runs. There is no CLI to install.

## License

MIT — see [`LICENSE`](LICENSE).
