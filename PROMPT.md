# Source authority hierarchy (paste into any chat model)

Paste everything below into ChatGPT, Claude.ai, Gemini or a local model, then add your claim and
the candidate sources (paste their text, with each source's own date).

---

You decide which source governs a claim. Do not decide the claim from memory. Work only from the
sources I give you.

Step 1. State the claim in one sentence.

Step 2. Type the claim as exactly one of:
- A. Framing or policy choice (what the project decided).
- B. Present-implementation fact (what the software or system does now).
- C. Third-party claim (what someone else's published work says).
- D. Project-generated datum (a number computed from the project's own records).
- E. Claimed external outcome (an effect on people or systems outside the project).
Each type can only be settled by some sources. Say which kinds of source are eligible.

Step 3. List every source that speaks to the claim, not just the newest. For each, give its own
stated date and check it for a note saying it has been superseded.

Step 4. Rank eligible sources on this ladder (1 = highest):
1 owner decisions, then unresolved open questions
2 current-state or governance document, cross-checked against later dated decisions
3 the artifact actually built or published
4 current-cycle research, review and decision records
5 live implementation evidence (only for type B claims)
6 framing and historical-baseline analysis (not proof of live behaviour)
7 notes on possible future work (not completed work)
8 an older proposal or audit from a prior phase
9 READMEs and historical plans
Rank by tier first. Break ties by date only inside the winning tier. A newer low-tier source
never beats an older source in a higher tier that governs that claim type.

Step 5. Answer in this form:
- Claim:
- Claim type:
- Governing source (tier, locator, date):
- Other sources and why they lose:
- Wording to use (hedged if the evidence is only partial):

Step 6. If no eligible source settles it, do not pick one. Output:
OPEN - DECISION REQUIRED
- Claim:
- Source A (locator, date):
- Source B (locator, date):
- Why the ladder did not resolve it:
- Cheapest way to resolve it:
- Blocking? (yes/no):
- Owner:

Flag any source you could not date. Say so when a conclusion rests on a single source.
