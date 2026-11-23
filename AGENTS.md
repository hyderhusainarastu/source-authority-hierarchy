# AGENTS.md

## Using this in your project

Use this skill when two or more sources disagree about the same claim, when a claim needs a
citation and several candidate sources exist, or when a status page may be out of sync with a
decisions log.

- Read `SKILL.md` and follow it in order: type the claim (section 1), rank eligible sources on the
  nine-tier ladder (section 2), break ties by date within the winning tier only (section 3).
- Never rank across tiers by recency. A newer low-tier file does not beat an older high-tier one.
- Check each source for a self-declared supersession note before trusting its headline claim.
- If no source settles the conflict, do not guess. Log an "OPEN - DECISION REQUIRED" entry
  (section 4): both sources, both dates, why the ladder did not resolve it, the cheapest
  resolution test, whether it blocks work, and an owner.
- Examples are in `examples/` and `references/worked_example.md`. They are synthetic.
- Where it writes: nothing is written by the skill itself. The contradiction ledger goes in
  whatever file the project names as its ledger (see the placeholders in SKILL.md section 0).
- There is no script and no CLI. It is a reasoning procedure.

Paste-in version for chat models without skill support: `PROMPT.md`.

## Working on this repo

- Setup: none. Plain Markdown, no dependencies.
- Tests: none. `./install.sh --dry-run --claude` shows what the installer would do.
- Keep `SKILL.md` frontmatter `description` at or under 300 characters and the examples synthetic.
- Do not add real project names, paths or records.
