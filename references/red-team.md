# Red-Team Pass

The advisors never see each other, which protects them from anchoring but leaves the Council with no error correction: a factual mistake in one memo can travel straight through synthesis unchallenged. The red team closes that gap without introducing voting — it checks the draft for defects rather than casting a sixth opinion.

Run it after synthesis, before the user sees anything, on the strongest available model (the capability floor in `independence.md`), in its own context. Skip only in Quick runs.

---

## What It Receives

* the neutral brief (§3)
* the evidence packet (§5)
* the advisor memos — **with lens labels removed**, presented as Memo A–E
* the draft conclusion from synthesis (§6)

Labels are stripped so the red team attacks reasoning rather than roles; knowing a point came from "the Contrarian" invites discounting it as that lens doing its job. It never receives the original conversation or the user's framing.

The red team is not a sixth advisor. It runs after synthesis, holds no position, and produces no fresh analysis — so the no-cross-visibility rule in §4 does not apply to it. Seeing all five memos is precisely what lets it check whether synthesis represented them faithfully.

---

## Mandate: Find Defects Only

The red team may not propose an alternative conclusion, advocate a position, rank the memos, or add new analysis. It looks for exactly five classes:

1. **Factual error** — a claim that is wrong, or upgraded beyond its evidence class in §5 (an assumption presented as verified, speculation presented as an assumption).
2. **Logical break** — non-sequitur, circular reasoning, or a conclusion the stated premises do not support.
3. **Framing leakage** — language, preferred answer, sunk-cost reasoning, or emphasis that entered from outside the neutral brief.
4. **Dropped objection** — a memo's decisive argument that synthesis silently discarded or misrepresented.
5. **Evasion** — false precision, fake consensus, manufactured disagreement, or hedging that hides the decisive variable.

---

## Output Format

A list of defects. For each: class, severity, the exact location in the draft, and the minimal correction that would resolve it.

* **Blocking** — the conclusion does not survive the defect.
* **Material** — the conclusion survives but is misstated, overclaimed, or missing something a reader would need.
* **Minor** — wording, precision, or clarity.

If nothing material is found, return "no material defects." This is a legitimate and expected result on a well-run council. Padding an empty result with invented Minor findings to look thorough is itself an Evasion defect — the exact failure the red team exists to catch.

---

## Handling

* **Blocking** defects must be corrected before output. If one cannot be resolved with the available evidence, downgrade: to UNCLEAR in Decision Mode, to NO CLEAR WINNER in Option Mode, or in Answer Mode remove the affected claim and name the resulting gap.
* **Material** defects must be corrected, or explicitly acknowledged in the output where correction is not possible.
* **Minor** defects are corrected silently.
* **Exactly one revision round.** Do not loop. A second pass on a revised draft drifts toward confirming the revision rather than finding new errors, and the cost is real while the yield collapses.

Severity reflects whether the defect is correct and consequential — never how confidently or how often it was stated. Weighing defects by count would smuggle voting back into a skill built to avoid it.

Report the result in §10, including when nothing was found.
