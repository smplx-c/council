# Independence: Tiers, Model Assignment, and Reporting

Five lenses reasoned by one model in one context are five angles on one prior. That is still useful — it catches framing errors and blind spots a single pass misses — but it is not five opinions, and presenting it as such is the most tempting lie this skill can tell.

The tiers below describe how much genuine decorrelation the run achieved. Buy as much as the environment sells; report exactly what you got.

---

## The Four Tiers

**Tier 1 — Cross-vendor.** Advisors run on models from different providers, reached through connected tools, an API gateway, or a router. Different weights, different training data, different alignment methods. This is the only tier where failure modes are meaningfully uncorrelated. Claim it only if it was actually executed on this run — not if it was merely available.

**Tier 2 — Cross-model, one family.** Advisors run on different models within the available family (for example via a delegation tool's `model` parameter: opus / sonnet / haiku). Different sizes and checkpoints decorrelate some errors, but shared lineage means shared blind spots persist. Better than Tier 3, well short of Tier 1.

**Tier 3 — One model, isolated contexts.** Five separate subagents, one lens each, no shared context. Context contamination is eliminated; the model prior is not. This is the realistic default in most environments and is a perfectly respectable run — it just must not be described as independent opinion.

**Tier 4 — Shared context.** Lenses reasoned sequentially in a single context. Separation exists by instruction only. This is the fallback when delegation is unavailable, not a target.

**Selection:** take the highest tier available on this run. Prefer Tier 2 over Tier 3 whenever a model parameter exists. In Deep runs, actively look for Tier 1 — check whether connected tools or routers can reach another provider before settling.

---

## The Capability Floor

The floor overrides tier preference. These four always run on the strongest model available:

* Advisor 1 — the Contrarian
* Advisor 2 — the First-Principles Thinker
* the synthesis step (§6)
* the red team (§6a)

They get the floor because each one is a *single point of failure*: if the Contrarian misses the fatal objection, no other lens is looking for it; if First Principles fails to notice the problem is misframed, every other memo answers the wrong question well; and synthesis and red team see everything, so their errors propagate to the whole output.

Heterogeneity is therefore applied to the Expansionist, the Outsider, and the Executor — and only where the assigned model is genuinely adequate for that lens. If a weaker model would return a shallow memo, use the stronger one and drop a tier. A decorrelated bad memo is worse than a correlated good one: it adds noise while looking like evidence.

---

## Decorrelation Hygiene

Free at every tier, and worth more than it looks:

* Never run two advisors in the same context window.
* Never let one advisor's output, position, or existence be visible to another.
* Never reuse one advisor's reasoning as a shortcut for another lens.
* Never vary the brief between advisors — only the lens varies.

---

## Reporting Wording (§10)

Use the phrasing for the tier actually reached. The second sentence is the part that matters; do not drop it.

* **Tier 1 — Cross-vendor.** "Advisors ran on models from different providers. Failure modes are meaningfully uncorrelated, but this is still five analyses, not five expert careers."
* **Tier 2 — Cross-model, one family.** "Advisors ran on different models from a single provider. Some error decorrelation; shared training lineage means shared blind spots remain. Agreement here is weak corroboration, not verification."
* **Tier 3 — One model, isolated contexts.** "Advisors ran independently with no shared context, but on one set of weights. Context contamination is eliminated; the model prior is not. Agreement here is one system reasoning from five angles."
* **Tier 4 — Shared context.** "Separation was reduced or absent. Cross-lens anchoring was mitigated by instruction only, not by architecture."

If the capability floor forced a downgrade, add one clause: for example, "Tier 3 rather than Tier 2 — the available smaller model was not adequate for the Executor lens."
