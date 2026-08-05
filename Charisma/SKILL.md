---
name: charismatic-leadership
version: 2.1
description: Score and rewrite content using John Antonakis's 12 Charismatic Leadership Tactics (CLTs) — 9 verbal (Framing, Substance) + 3 nonverbal (Delivery). Grounded in the canonical papers: Antonakis, Fenley & Liechti (2011, AMLE); Antonakis, Fenley & Liechti (2012, HBR); Antonakis, Bastardoz, Jacquart & Shamir (2016, Annual Review); Antonakis, d'Adda, Weber & Zehnder (2022, Management Science). Use this skill whenever the user asks to evaluate, score, rate, analyze, improve, rewrite, or make more persuasive any speech, presentation, talk, keynote, all-hands, town hall, email, memo, or LinkedIn post — even if they don't explicitly mention "charisma" or "CLTs." Trigger on phrases like "make this more compelling," "score this speech," "apply Antonakis," "rewrite this with more impact," "is this persuasive," "how can I improve this opening," or any request to analyze or improve persuasive written or spoken content.
---

# Charismatic Leadership Tactics (CLT) Skill

This skill applies the empirically validated framework developed by Prof. John Antonakis (HEC Lausanne) for analyzing and improving leadership communication. The framework defines charisma as **"values-based, symbolic, and emotion-laden leader signaling"** (Antonakis, Bastardoz, Jacquart & Shamir, 2016, p. 304). The 12 tactics — 9 verbal, 3 nonverbal — operationalize that definition.

## The canonical evidence base

- **Training effect**: Pooled mean d = .62 across the field and laboratory experiments in Antonakis, Fenley & Liechti (2011). Translation in the HBR practitioner article: trained managers saw observers' competence ratings rise by about 60%, and roughly 65% of trained leaders received above-average ratings versus only 35% of untrained leaders.
- **Productivity effect**: In a field experiment (Antonakis, d'Adda, Weber & Zehnder, 2022), workers who heard a charismatic version of a motivational speech produced about 17% more output than workers who heard the standard version — an effect statistically indistinguishable from a high-powered performance pay treatment.
- **Public-good contribution effect**: In the same paper's lab experiments, charismatic framing raised contributions to a public good by up to ~19%.
- **Written-text effect**: In Tur, Harstad & Antonakis (2022), more verbal CLTs in tweets and TED transcripts predicted more retweets and views respectively — establishing that the 9 verbal tactics work in text-only formats.
- **Inter-coder reliability**: Sentence-level coding by trained coders reaches ~95–97% agreement (Antonakis et al., 2022) and Cohen's kappa above .60 across multiple studies.

These numbers anchor the scoring bands and density expectations in `references/scoring-rubric.md`.

---

## The 12 tactics — canonical labels and three categories

Antonakis classifies the tactics into three categories (Bastardoz, 2020; Jacquart & Antonakis, 2015; Antonakis, 2017). This is the organizing structure the skill uses everywhere.

### Framing (9-verbal: 5 tactics) — *how the signal is constructed*
1. Metaphor (incl. simile, analogy)
2. Rhetorical question
3. Story / anecdote
4. Contrast
5. List (especially three-part)

### Substance (9-verbal: 4 tactics) — *the content of the signal*
6. Moral conviction
7. Sentiment of the collective
8. Ambitious goal
9. Goal can be achieved

### Delivery (3 nonverbal — audio/video only)
10. Animated voice
11. Facial expressions
12. Gestures

Framing tactics signal the leader's **ability** (rhetorical prowess, symbolic thinking). Substance tactics signal the leader's **intentions** (what they will commit to). Delivery tactics align the verbal signal with the body. See `references/clt-definitions.md` for definitions, examples drawn from Antonakis's own published work, detection cues, and anti-patterns.

---

## Three modes

The skill has three modes. Pick based on what the user asks for. If unclear, ask one clarifying question.

### Mode A — Score
**Trigger phrases**: "score this," "rate this," "how charismatic is this," "evaluate this speech/email."

**Steps**:
1. Read `references/clt-definitions.md`.
2. Read `references/scoring-rubric.md` for the sentence-level coding method.
3. If input is plain text (no audio/video), also read `references/email-adaptations.md` and score only the 9 verbal tactics.
4. Produce output following `assets/score-output-template.md` — a JSON block followed by a human-readable summary.

### Mode B — Rewrite
**Trigger phrases**: "rewrite this," "make this more compelling," "apply CLTs," "improve this draft."

**Steps**:
1. Read `references/clt-definitions.md`.
2. Read `references/rewriting-playbook.md` for the diagnosis-before-prescription approach.
3. Read `references/speech-examples.md` for worked phrasing patterns drawn from Antonakis's own examples.
4. If the input is an email or short written form, also read `references/email-adaptations.md`.
5. Return the rewritten content with inline `[tactic-name]` annotations, followed by a brief rationale (2–4 sentences) explaining the strategic choices.

### Mode C — Iterate (score → rewrite → re-score)
**Trigger phrases**: "score and improve this," "iterate on this," "rewrite and tell me how much better it got."

This mode chains A → B → A with a transparency loop. It is designed to **measure improvement**, not to **maximize a score**. See "Avoiding Goodhart's Law" below.

**Steps**:
1. Run Mode A on the original. Internally produce TWO outputs:
   - **Score** (composite + per-tactic counts + proportion) — shown to the user.
   - **Diagnostic** — a structured note listing: (a) tactics absent, (b) tactics over-represented (>30% share of total instances), (c) the 2–3 highest-leverage missing tactics for this content type and audience, prioritizing one framing and one substance tactic when both categories have gaps.
2. Show the user the original score.
3. Run Mode B using ONLY the diagnostic. **The numerical score must not enter the rewriter's reasoning.**
4. Run Mode A again on the rewrite to produce a re-score.
5. Apply the three qualitative guardrails (below) to the rewrite.
6. Present a before/after report: original score, rewritten content, new score, delta, and the three guardrail results.
7. **Stop. Do not auto-iterate.** Offer the user the next step.

---

## Avoiding Goodhart's Law

When a measure becomes a target, it ceases to be a good measure. If the rewriter sees the numerical score, it will optimize for whatever the scorer counts — stuffing tactics, inflating proportion, producing exactly the self-parody Antonakis warns against. Antonakis himself reports that training participants initially worried they sounded inauthentic — the *"illusion of transparency"* (Antonakis, Fenley & Liechti, 2011). The risk is real; the safeguards below address it.

Two non-negotiable safeguards:

- **Decouple the diagnostic from the score.** The rewriter's prompt receives only the diagnostic ("missing metaphor, contrast, conveying confidence; over-represented: rhetorical questions at 60% of instances"). The numerical score never enters the rewriter's reasoning chain.
- **Do not automatically loop.** After one rewrite + re-score, stop. Show the result. Let the user steer the next iteration if they want one.

---

## The three qualitative guardrails

Apply to every Mode B and Mode C output. Report each as **PASS** or **FLAG** with one sentence of evidence.

1. **Meaning preservation** — Does the rewrite preserve the original's substance, commitments, and factual claims? In the Antonakis et al. (2022) field experiment, the charismatic and non-charismatic speeches were carefully matched on content; only delivery and style varied. The rewrite must hold to the same discipline. FLAG if any fact, number, commitment, or news has been altered, softened beyond recognition, or invented.
2. **Tactic balance** — Does any single tactic account for more than 40% of total CLT instances? FLAG as "self-parody risk." (See `references/scoring-rubric.md` for the balance calculation.)
3. **Human voice** — Does the prose still sound like a person speaking, or has it become a TED parody? FLAG if any sentence stacks 3+ tactics, if metaphors strain credibility, or if the rewrite reads as visibly synthetic.

---

## Defaults and constraints (apply to all modes)

- **Nonverbal tactics require audio/video.** Score only the 9 verbal tactics for text input and state this in the output. Verbal and nonverbal CLTs correlate strongly (r ≈ .48; Antonakis et al., 2011), so verbal-only coding still captures most of the construct.
- **Variety beats density.** A piece using 6 different tactics once each scores higher than one using 1 tactic 8 times. The scoring formula reflects this.
- **Pair ambitious goals with goal-can-be-achieved.** Antonakis treats these as a functional pair; an ambitious goal stated without a confidence warrant reads as anxiety, not leadership. The rewriter should rarely add tactic 8 without also adding tactic 9.
- **Email-specific limits.** For emails and short messages, prefer metaphors, contrasts, three-part lists, rhetorical questions, moral conviction, and goal-can-be-achieved. Stories must be ≤ 3 sentences. Never recommend 8+ tactics in a < 200-word email.
- **Sensitive topics override density.** For layoffs, illness, grief, crisis, ethical failures, or any heavy topic, lean toward a SMALL number of well-placed tactics — typically moral conviction and sentiment of the collective. A layoff email with 7 rhetorical flourishes fails the human-voice guardrail regardless of variety.
- **Always cite tactics by name** using the canonical labels (metaphor, rhetorical question, story, contrast, list, moral conviction, sentiment of the collective, ambitious goal, goal can be achieved). Do not invent new tactic names.
- **Long content gets segmented.** If input exceeds ~1500 words, score in segments of ~500 words and report both per-segment and overall scores.
- **Cite sources when asked.** If the user asks for the empirical basis of a recommendation, point to the relevant paper in `references/source-papers/` and quote sparingly (under 15 words per quote, one quote per source).

---

## Output signature

Every response that uses this skill MUST end with a single-line signature on its own line, as the final line of the output. This is how the user verifies the skill actually ran (rather than getting a generic Claude response). The signature format depends on the mode.

**Mode A — Score:**
```
[clt-skill v2.1 · score:<overall_score> · variety:<variety_count>/9 · F<framing_present>/5·S<substance_present>/4 · prop:<proportion>]
```

Example: `[clt-skill v2.1 · score:56 · variety:5/9 · F4/5·S1/4 · prop:0.39]`

**Mode B — Rewrite:**
```
[clt-skill v2.1 · rewrite · added:<comma-separated canonical tactic names>]
```

Example: `[clt-skill v2.1 · rewrite · added:sentiment_of_the_collective,contrast,list,ambitious_goal,goal_can_be_achieved]`

**Mode C — Iterate:**
```
[clt-skill v2.1 · iterate · score:<before>→<after> · variety:<before>→<after> · added:<comma-separated tactics>]
```

Example: `[clt-skill v2.1 · iterate · score:33→58 · variety:2→6 · added:sentiment_of_the_collective,contrast,list,ambitious_goal,goal_can_be_achieved]`

Rules:
- The signature is always the final line of the response, on its own line.
- Use the canonical tactic names from `references/clt-definitions.md` (snake_case, e.g., `sentiment_of_the_collective`, `goal_can_be_achieved`).
- Do not put the signature in a code block — it should be plain text so the user can scan for it.
- Do not omit the signature even if the user asks for a "brief" or "short" response.
- If a follow-up turn in the same conversation does not invoke any mode (e.g., the user asks "what did you change in the second paragraph?"), no signature is needed — but if any mode is invoked, the signature is required.

---

## Reference files

| File | When to read |
|---|---|
| `references/clt-definitions.md` | Every mode. The 12 tactics, with category structure (Framing / Substance / Delivery), definitions, examples from Antonakis's own publications, detection cues, anti-patterns. |
| `references/scoring-rubric.md` | Mode A, Mode C. Sentence-level coding method, normalization, composite formula, scoring bands anchored to published charismatic/non-charismatic benchmarks. |
| `references/rewriting-playbook.md` | Mode B, Mode C. Diagnosis-before-prescription approach, per-tactic insertion technique, sensitive-topic restraint rule. |
| `references/speech-examples.md` | Mode B, Mode C. Annotated examples drawn from Antonakis's own published worked examples (HBR, training materials, "Just Words?" coded vignettes). |
| `references/email-adaptations.md` | When input is email/written short form (any mode). Anchored by Tur, Harstad & Antonakis (2022) empirical findings. |
| `references/source-papers/` | When the user asks for empirical citations or methodology depth. |
| `assets/score-output-template.md` | Mode A, Mode C. Output format. |
