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

## Quick start

Paste this into any chat model, then add your claim and the sources with their dates:

```
Decide which source governs this claim. Step 1: state the claim in one sentence. Step 2: type it
(framing/policy, present-implementation fact, third-party claim, project-generated datum, or
claimed external outcome). Step 3: list every source that speaks to it with its own date and any
"superseded" note. Step 4: rank by a nine-tier ladder (owner decisions, governance doc, built
artifact, current-cycle records, live implementation, framing analysis, future-work notes, older
proposals, READMEs); tier first, date only within the winning tier. Step 5: if nothing settles
it, do not pick one - log "OPEN - DECISION REQUIRED" with both sources, both dates, why the
ladder failed, the cheapest test, blocking status and owner.

Claim: ...
Sources: ...
```

The full self-contained version is [`PROMPT.md`](PROMPT.md).

## Works with

| Surface | How | Status |
|---|---|---|
| Claude Code | native skill (`./install.sh --claude`) | not tested |
| Codex | native skill via `.agents/skills` (`./install.sh --agents --project .`) | not tested |
| Gemini CLI | native skill via `.agents/skills`, or `./install.sh --gemini --project .` | not tested |
| Cursor | native skill via `.agents/skills`; reads `AGENTS.md` | not tested |
| Copilot | pointer block (`./install.sh --copilot --project .`) | not tested |
| ChatGPT / Claude.ai / Gemini (chat) | prompt (`PROMPT.md`) | not tested |

Installer: `./install.sh --help`. With no target flag it changes nothing. It symlinks by default,
`--copy` copies, `--uninstall` removes only what it installed, `--dry-run` previews.

Once installed, an agent should invoke it whenever two documents disagree, whenever a claim needs
a citation and more than one candidate source exists, or whenever a "living" status document
might be out of sync with a decisions log. `SKILL.md` is self-contained: no bundled script, no
external dependency.

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

## Limits

It is a reasoning procedure, not a tool: it does not read your files or verify dates for you, and
the quality of the ruling depends on the sources you supply. The nine tiers are a default; adapt
the placeholders in `SKILL.md` section 0 to your project. The examples are synthetic.

## Related

- [evidence-ledger-and-phase-handoff-convention](https://github.com/hyderhusainarastu/evidence-ledger-and-phase-handoff-convention)
- [legacy-plan-reconciliation-matrix](https://github.com/hyderhusainarastu/legacy-plan-reconciliation-matrix)
- [governance-records-ledger-conventions](https://github.com/hyderhusainarastu/governance-records-ledger-conventions)

## Keywords

source reliability, fact checking, provenance, conflict resolution, evidence ranking, research
methods, agent skill, SKILL.md

## License

MIT — see [`LICENSE`](LICENSE).

---
Part of palimnote-skills - more at https://github.com/hyderhusainarastu
