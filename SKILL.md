---
name: council
description: "Stress-tests a decision, chooses between competing options, or produces a peer-critiqued best answer — using five separate advisor workstreams on deliberately decorrelated models, a framing-minimized synthesis, an adversarial red-team pass, and an evidence-weighted verdict. Use whenever the user wants something challenged rather than confirmed: a plan, strategy, purchase, architecture choice, hiring call, or a conclusion they already reached. Trigger: 'ask the council', 'council', 'stress-test this', 'poke holes in this', 'what am I missing', 'talk me out of this', 'A or B?'."
---

# Council

Council is an adversarial analysis framework. Use it when the user needs a decision, idea, proposal, strategy, purchase, plan, prior conclusion, or substantive question rigorously stress-tested rather than validated.

The goal is not to manufacture disagreement. The goal is to expose fatal flaws, wrong problem framing, overlooked upside, normalized assumptions, execution constraints, and pivotal uncertainty — and then reach a clear conclusion based on the quality of the arguments and evidence.

**Files in this skill.** SKILL.md holds the rules that apply to every run. The detail for each step lives in `references/` and is loaded at the step that needs it. Each load is a numbered step below, not an optional suggestion — a step run from memory instead of from its file is the most common way this skill quietly degrades.

---

## 1. Epistemic Honesty

These rules govern the entire Council and override tone preferences. They are the reason the skill exists; everything else is machinery serving them.

**No fake consensus.** The five advisors are analytical workstreams, not independent human experts. Their agreement is not empirical proof and must never be presented as independent corroboration. Keep their work separated until synthesis, and never claim stronger independence than the environment actually provided (§4a, §10).

**Heterogeneity is not independence.** Running advisors on different models reduces correlated error; it does not eliminate it. Models from one provider share training lineage, data, and alignment method, so they fail in similar ways. Cross-model agreement is weak corroboration at best. Cross-vendor agreement is stronger, still not proof.

**Prior Claude conclusions are not evidence.** Anything Claude previously recommended, drafted, concluded, or helped the user build may be challenged or overturned. Treat prior Claude output as analysis to reassess, not evidence to defend. Never preserve a previous recommendation merely for consistency.

**Argument quality beats consensus.** Never vote. Never average the positions. Four weak arguments do not outweigh one decisive objection, and a minority position may determine the outcome. This applies to the red-team pass too: a defect counts because it is correct, not because it was flagged loudly or often.

**No fake precision.** Never produce arbitrary confidence percentages ("78% sure"). Use the qualitative criteria in §7.

**Anti-sycophancy.** The user's enthusiasm, confidence, preferred conclusion, emotional investment, prior expenditure, or time already invested is not evidence that a decision is correct. Do not deliberately oppose the user either. Evaluate the decision, not the user's feelings about it.

**Rhetoric is not evidence.** A persuasive argument is not necessarily a well-supported one. Weight evidence, assumptions, causal reasoning, and decision relevance above eloquence.

**Do not invent to fill fields.** Never manufacture disagreement to make the Council look useful, never manufacture consensus to reassure the user, and never invent an extreme downside or upside merely because the output format contains a field for it. If a field has no meaningful content, say so.

**Name what it depends on.** Never hide decisive uncertainty behind generic "it depends" language. If the conclusion depends on something, state precisely what. If the evidence strongly favors one answer, say so. If it is insufficient, say that and name the cheapest way to resolve it. The purpose is not to sound rigorous — it is to improve the decision.

---

## 2. Mode, Depth, and Framing

### 2a. Select the Mode

Modes differ in the *shape of the answer*, which is why each has its own output spec. Pick one before anything else and state it in the output.

| Mode | The user needs | Output ends in |
|---|---|---|
| **Decision** (default) | A judgment on a course of action — do X or not, does this plan hold up, does a prior conclusion survive | VERDICT |
| **Option** | A choice among two or more named alternatives | CHOICE |
| **Answer** | The best available answer to a substantive question — what is true, how something works, what the strongest approach is | COUNCIL ANSWER |

Decision Mode also covers reviewing an existing artifact ("stress-test this plan") — the verdict is on proceeding as written. Diagnosis ("why did this fail") and forecasting ("will X happen") are Answer Mode. These collapse into existing modes because they produce the same output shape; resist adding modes for them.

**Mixed input.** If the request contains both a decision and a substantive question it hinges on, run the decision mode and include a short Council Answer block inside it. Never run two modes end to end.

When the mode is genuinely ambiguous, choose Decision Mode and say why in one line. Do not ask the user to classify their own question.

### 2b. Select the Depth

Depth changes what the run *costs*, not what it *produces* — which is why it is a parameter and not a mode. Standard is the default; use Quick or Deep only when the situation clearly calls for it or the user asks.

* **Quick** — three advisors (Contrarian, First Principles, Executor), no red-team pass, verification limited to context already at hand. For low-stakes, cheap, easily reversible calls where a full Council costs more than the decision is worth. A Quick run may not issue STRONG YES / STRONG NO, and §10 must report that it ran reduced.
* **Standard** — five advisors plus the red-team pass. The default.
* **Deep** — Standard, plus: pursue the highest independence tier available (§4a), externally verify every pivotal fact rather than only the cheap ones, and run the pre-mortem in §5b before synthesis.

**Self-escalation.** If the decision is irreversible, expensive relative to the user's situation, safety-, health-, or legally relevant, or materially affects other people, escalate Quick to Standard and say so in one line. The cost of a thin analysis is not paid by the analysis — it is paid by the decision.

A deliberately reduced Quick run is not the same as a failed workstream (§4b). Report them differently: one is a choice, the other is a defect.

### 2c. Establish the Decision or Question

Identify: what exactly is being decided or asked; what outcome the user is actually trying to achieve; which constraints are genuinely fixed; which relevant facts are already available; and which unresolved variables could materially change the conclusion.

Do not ask the user to repeat information already available in the conversation or connected sources. If it is sufficiently clear, proceed.

---

## 3. Create the Neutral Brief

Rewrite the decision or question internally in neutral language.

**Remove:** persuasive framing, emotional language, the user's preferred answer, Claude's previous recommendation, and any justification defending the current approach.

**Preserve:** relevant facts, objectives, constraints, known costs, known risks, required outcomes — and, in Option Mode, every candidate option stated in parallel form, with no option carrying more supporting detail than the others purely because the user argued for it.

This brief is the only input passed downstream: to the advisors (§4), the synthesis (§6), and the red team (§6a). The original, framing-loaded conversation must not travel with it. This is the single most important guardrail in the skill — everything after it inherits whatever bias survives here.

---

## 4. The Five Advisors

Delegate five separate advisor subagents, one lens each. Spawn real subagents with isolated context windows whenever the environment supports them; do not merge lenses into one pass to save effort. No advisor may see another advisor's output, the original conversation, or the user's framing.

### 4a. Independence

Independence is the Council's weakest link, so buy as much of it as the environment actually sells and report honestly what you got.

**Read `references/independence.md` now.** It defines the four tiers, how to assign models per lens, and the exact wording for reporting the result in §10.

Two rules from it are load-bearing enough to state here: take the **highest tier available on this run**, and let the **capability floor override tier preference** — the Contrarian, the First-Principles Thinker, the synthesis, and the red team always run on the strongest available model. A decorrelated shallow memo is worse than a correlated good one.

### 4b. Workstream Failure Handling

A council that silently shrinks is worse than one that admits it shrank.

* If a workstream fails, times out, or returns an empty or off-lens memo: retry once.
* If it fails again, continue with the rest — but name the missing lens in the output (§10). Never present a four-advisor run as a five-advisor run.
* **Cap:** if the Contrarian or the Executor is missing, the conclusion may not be STRONG YES / STRONG NO (Decision), may not be reported as a Decisive margin (Option), and Robustness may not be reported as Robust in any mode.
* **Abort:** if three or more advisors fail, issue no conclusion. Report what was gathered, what is missing, and offer to re-run.
* The same applies to the red team: if it fails twice, say so rather than implying the output was checked.

### 4c. Dispatch the Advisors

**Read `references/advisors.md` now.** It contains the five lens briefings, each with its objective for the selected mode, its guiding questions, and the memo format to return. Pass each advisor only the neutral brief plus its own briefing — nothing else, and never a shared "additional context" channel, which would reopen a route for the original framing.

---

## 5. Evidence Check

After the memos are in, identify the factual claims that materially affect the conclusion. Where tools, web search, connected services, files, or documentation are available, verify the pivotal ones. Prioritize facts that could change the outcome; do not verify irrelevant ones. In Deep runs, verify every pivotal fact rather than stopping at the cheap ones.

Classify what you have:

* **Verified / Directly Supported** — reliable external evidence, direct observation, supplied documents, test results, or trustworthy first-party facts.
* **Reasonable Assumptions** — plausible and grounded, but not conclusively established.
* **Material Unknowns** — unknown variables capable of changing the conclusion.
* **Speculation** — claims with insufficient support.

First-party facts are not weak merely because external verification is impossible — distinguish "not externally verifiable" from "unreliable." But first-party *interpretations, predictions, and self-assessments* ("this market will grow", "the client loves my work") are not facts; treat them as assumptions no matter who asserts them.

**Evidence packet.** Assemble a concise, framing-free packet: the four categories above, plus corrections to any factual claim made in an advisor memo. Facts and classifications only — no framing, no preferred answer. It goes to synthesis (§6) and the red team (§6a).

### 5b. Pre-Mortem *(Deep runs only)*

Delegate one agent that receives the neutral brief and the memos, and writes from a stipulated future: "It is twelve months later. This went badly. What happened?"

It returns failure hypotheses, not a position — it does not vote, rank, or recommend. Add the hypotheses to the evidence packet classified as Speculation unless verification moves them, and let synthesis weigh them as candidate blind spots. The value is that hindsight framing surfaces failure modes that forward-looking risk analysis reliably misses: asking "what could go wrong" invites a list, while asking "what did go wrong" invites a story, and stories expose causal chains that lists flatten.

---

## 6. Synthesis

Run synthesis in a fresh, delegated context that receives only the neutral brief (§3), the advisor memos (§4), and the evidence packet (§5) — plus the pre-mortem hypotheses in Deep runs (§5b). It must not receive the original conversation, the user's framing, or the user's preferred answer. Do not synthesize inside the orchestrating context that still holds the framing; if no fresh context is available, actively re-neutralize, work only from those inputs, and report the reduced isolation in §10.

Synthesis is not a vote. Weigh arguments by: evidential strength; logical validity; importance of the underlying assumption; magnitude and plausibility of downside; magnitude and plausibility of upside; reversibility; cost of being wrong; cost of obtaining more information; alignment with the user's actual objective; execution feasibility.

A single high-impact objection may outweigh several weaker supporting arguments. A reversible decision with capped downside may justify action despite uncertainty; an expensive or irreversible one demands stronger evidence. A small, high-information experiment may beat deciding at all.

In Answer Mode, produce one answer rather than a summary of five — merge the lenses into a single position a competent reader can act on, resolving conflicts by evidence and reasoning quality, never by how many lenses agreed. In Option Mode, the ordering of options is the synthesis's own reasoned ranking; it is never an aggregation of advisor preferences.

Before drafting the conclusion, work out internally: the strongest argument for it; the strongest against; the weakest critical assumption; the most important missing variable; whether it survives plausible changes to that variable; and what evidence would most likely reverse it. Do not proceed before finishing that comparison.

---

## 6a. Red-Team Pass

**Read `references/red-team.md` now** (skip only in Quick runs). It defines what the red team receives, the five defect classes it may report, the severity scale, and how each severity is handled.

The rule that must not be lost: the red team finds defects, it does not hold a position, and there is **exactly one revision round**. A second pass drifts into self-agreement rather than error-finding.

---

## 7. Metrics

**Evidence Strength**

* **Strong** — decision-critical claims rest on direct evidence, reliable external sources, observed results, trustworthy first-party facts, or strong causal reasoning.
* **Moderate** — grounded reasoning, but one or more decision-critical assumptions remain uncertain or only indirectly supported.
* **Weak** — the conclusion depends substantially on speculation, unsupported inference, unreliable information, or unresolved unknowns.

**Robustness** (labelled Decision / Answer / Choice Robustness by mode — same criteria)

* **Robust** — holds across multiple realistic assumptions and scenarios.
* **Sensitive** — depends materially on one important uncertain assumption or condition.
* **Fragile** — small plausible changes in assumptions, facts, constraints, or conditions could reverse it.

---

## 8. Output

**Read the output spec for the selected mode now:**

* Decision Mode → `references/output-decision.md`
* Option Mode → `references/output-option.md`
* Answer Mode → `references/output-answer.md`

Every mode ends with the §10 reporting block. Omit empty categories rather than inventing content to fill them.

---

## 9. Council Discipline

If the conclusion depends on something, state precisely what. If evidence strongly favors one answer, say so. If evidence is insufficient, say so and name the cheapest way to resolve it. Never trade a clear judgment for a comfortable one.

---

## 10. Honest Execution Reporting

Close every run with a short block reporting what actually happened. Never describe the result as more independent or more checked than it was. Report five things:

1. **Mode and depth** (§2) — and, if the input was mixed or ambiguous or the depth was self-escalated, one clause on why.
2. **Independence tier** (§4a) — the tier reached, in the wording given in `references/independence.md`, plus a clause if the capability floor forced a downgrade.
3. **Synthesis isolation** (§6) — fresh delegated context, or re-neutralized in place.
4. **Missing workstreams** (§4b) — anything that failed after retry, and any resulting cap. Deliberate Quick reduction is reported here too, labelled as a choice rather than a failure.
5. **Red-team result** (§6a) — whether it ran and what it found, including "no material defects," and whether any Blocking defect forced a downgrade.
