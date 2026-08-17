# Council

**Council pressure-tests your decision instead of confirming it — by stripping your framing out before the analysis begins.**

A Claude skill: five separate advisor workstreams, an adversarial review pass, and a verdict that names its own tipping point and next step.

---

## The problem it solves

Ask a language model a question and you get a cooperative answer. It adopts your framing, fills gaps charitably, and hands back the best version of *your* idea. For most tasks that is exactly right.

For decisions it is the failure mode: **you get confirmation and mistake it for scrutiny.**

Council intervenes at the four points where that happens.

---

## How Council intervenes

**Your framing is removed before anyone thinks.** The decision is rewritten in neutral language — without your preferred answer, without the justification you use to defend your current approach, without whatever Claude recommended to you earlier. Only that version reaches the advisors and the synthesis. This is the skill's single largest effect: otherwise *how* you ask determines what you hear.

**Five lenses look for different things, separately.** Contrarian, First Principles, Expansionist, Outsider and Executor run in isolation and never see each other. Each hunts something else — avoidable downside, a misframed problem, missed upside, an assumption nobody questions anymore, practical feasibility. A single pass typically finds one or two of them.

**Evidence is separated from opinion.** Facts are sorted into four classes: verified, reasonable assumption, material unknown, speculation. Your own judgments ("this market will grow", "the client is happy") count as assumptions no matter who asserts them. That makes visible how much of the verdict is actually carried.

**No verdict without a challenge.** A red team receives the draft and the anonymized memos and hunts for factual errors, logical breaks, framing that crept back in, dropped objections, and evasion. Exactly one revision round.

---

## What you get back

**An output you can decide on.** Not "it depends," but: what it depends on, which condition would reverse the verdict, what to do tomorrow, and what not to do yet.

**A report on the run itself.** Every run ends with a block disclosing how independent the advisors actually were, whether synthesis ran in isolation, whether a workstream failed, and what the red team found. Five lenses on one model are five angles on one prior — the skill says so itself rather than selling agreement as corroboration.

---

## What it costs and what it can't do

A run takes longer and costs substantially more tokens than an ordinary answer. For small, cheap, easily reversible calls that is overkill — use the Quick depth, or skip it entirely.

The skill does not replace domain expertise. It does not replace data you don't have; it only names it as unknown. It is not legal, medical, or financial advice. And it is not a vote: agreement among the advisors is not proof, and a single minority position can determine the verdict.

---

## Installation

**Claude Code** — put the folder in your skills directory:

```bash
git clone https://github.com/smplx-c/council.git ~/.claude/skills/council
```

**Cowork / Claude Desktop** — download `council.skill` from the [latest release](https://github.com/smplx-c/council/releases/latest) and drop it into a chat. The file card shows **Save skill**, provided your organization permits skill creation.

---

## Invoking it

Call it directly with the slash command and put the case after it:

```
/council Should we migrate to a new database now or after launch?
Two engineers, no dedicated DBA, launch in eight weeks.
```

This is the reliable path: nothing has to be recognized, and the language you write in makes no difference.

It also fires on its own, without the slash command, when you open with one of these:

> "ask the council" · "council" · "stress-test this" · "poke holes in this" · "what am I missing" · "talk me out of this" · "A or B?"

Those phrases are registered in English only, and implicit triggering is a judgment call rather than a guarantee. If you write in another language — or simply want to be sure — use `/council`.

It works best with context rather than a bare question. Instead of "should I use Postgres?", give it: what you're building, what is already fixed, what bothers you about the plan, and what it would cost to be wrong.

```
Council — A or B? Have the advisor memos rank each other the way Karpathy's
llm-council does, or keep them isolated and add a red-team pass afterwards?
```

```
Stress-test this: I decided last week to postpone a feature. Here was my
reasoning. Does it hold up?
```

---

## Three modes — by the shape of the answer

The skill picks the mode itself and states it in the output. Modes differ in the **shape of the answer**, which is why each has its own output format.

| Mode | For | Ends in |
|---|---|---|
| **Decision** (default) | Should I do X? Does this plan hold up? Does my earlier conclusion still stand? | **VERDICT** — STRONG YES through STRONG NO, plus the tipping point |
| **Option** | Which of these alternatives? | **CHOICE** — the pick, what it costs, and the condition under which the runner-up wins |
| **Answer** | What is true here? How does this work? What is the strongest approach? | **COUNCIL ANSWER** — the answer itself, first and directly usable |

Diagnosis ("why did this fail") and forecasting ("will X happen") run as Answer Mode; reviewing an existing document or plan runs as Decision Mode.

Option Mode can tell you outright that the best move isn't on your list — a well-run comparison between three wrong options is more dangerous than none at all, because its rigor makes the result feel earned.

## Three depths — by effort

Depth changes the **effort**, not the shape; that is why it is a parameter rather than a mode.

* **Quick** — three advisors (Contrarian, First Principles, Executor), no red team. For cheap, easily reversible calls. Cannot issue a STRONG verdict.
* **Standard** — five advisors plus the red team. The default.
* **Deep** — adds: the highest achievable model independence, external verification of *every* pivotal fact, and a pre-mortem ("it is a year later and this went badly — what happened?").

For irreversible, expensive, safety- or legally-relevant decisions, the skill escalates Quick to Standard on its own and says so.

---

## Reading the output

**Evidence Strength** tells you what the verdict stands on: *Strong* = supported, *Moderate* = one load-bearing assumption is open, *Weak* = the verdict rests on unverified material.

**Robustness** tells you how stable it is: *Robust* = holds across several realistic scenarios, *Sensitive* = hangs on one uncertain assumption, *Fragile* = small changes reverse it.

**Tipping Point / Switching Condition** is the most useful line in the whole output. It tells you what to watch after you decide.

**The report at the end** states mode and depth, the independence tier reached, whether synthesis ran in isolation, any failed workstreams, and the red-team result. A Tier 3 run with "no material defects" is a good run — but still one system reasoning from five angles, not five independent opinions. If a failure is reported there, the verdict is capped accordingly.

---

## Repository layout

```
council/
├── SKILL.md                      rules that apply to every run
├── README.md                     this document
└── references/
    ├── advisors.md               the five lenses, per mode
    ├── independence.md           independence tiers, model assignment, reporting
    ├── red-team.md               mandate, defect classes, severities
    ├── output-decision.md        Decision Mode output format
    ├── output-option.md          Option Mode output format
    └── output-answer.md          Answer Mode output format
```

SKILL.md holds only what applies to every run. Everything step-specific lives in `references/` and is loaded at the point where it is needed — as a numbered mandatory step, not as a reference mentioned in passing.
