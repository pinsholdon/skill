# Scoring Rubric — Sentence-Level Coding

This file defines exactly how to score a piece of content. Apply consistently across Mode A and the two passes of Mode C so that before/after comparisons are valid.

The method follows the coding approach Antonakis and colleagues use in their published research, particularly Antonakis, d'Adda, Weber & Zehnder (2022) "Just Words? Just Speeches?", which reports 97.02% inter-coder agreement on 41 sentences × 9 categories. Tur, Harstad & Antonakis (2022) use the same approach on TED transcripts and tweets.

---

## 1. The coding unit: the sentence

**The unit of analysis is the sentence.** This is non-negotiable — it is how Antonakis's coders work, and it is what makes scores comparable to the benchmarks published in the literature.

- Split the input into sentences. Use standard punctuation cues (period, question mark, exclamation point). Treat fragments separated by em-dashes or semicolons as separate sentences if they could stand alone.
- For each sentence, mark which of the 9 verbal tactics are present using binary indicators (0 or 1).
- A sentence can contain multiple tactics — for example, "How can you expect me to change an engine in a plane midflight?" contains both a rhetorical question (CLT2) and a metaphor (CLT1). Count both.
- Per-tactic instances are summed across all sentences to get total counts.

**For audio/video input** with verifiable nonverbal data, also code the 3 delivery tactics, but count each clearly observable, sustained instance rather than per-sentence presence.

## 2. Tactic detection rules

Use the detection cues from `references/clt-definitions.md`. Brief recap of the disambiguation rules for the ambiguous cases:

- **Metaphor**: each distinct vehicle counts once. Dead metaphors that have become ordinary verbs (we *grasped* the problem, the market *cooled*) do NOT count. A sustained metaphor — like MLK's interlocked island/ocean/bank/check sequence — counts once per distinct vehicle, even within one sentence.
- **Story**: each distinct narrative episode is one instance, regardless of how many sentences it spans. So Lynn's Eiger climb story across 3 sentences = 1 instance, marked on the first sentence of the narrative.
- **Contrast**: each parallel opposing-pair is one. A three-way opposition still counts as one contrast.
- **Rhetorical question**: each "?" in a clearly rhetorical sentence is one. Even three short questions in a row (the Anita Roddick pattern) are three instances.
- **List**: three coordinated items = 1 instance. Lists with four or more items do NOT count — Antonakis is specific that the rule is three. Lists "under the radar" (without "first, second, third" markers) still count if the three items are syntactically parallel.
- **Moral conviction**: each distinct value-statement is one. A sentence stating two related values bundled into a single commitment counts as one; two independent moral claims in separate clauses count as two.
- **Sentiment of the collective**: each acknowledgment-of-audience-experience is one instance. A long sentiment paragraph (like Rami's IT director example) counts each distinct acknowledgment.
- **Ambitious goal**: each distinct ambitious target is one. Two goals = two instances.
- **Goal can be achieved**: each warrant-of-belief is one instance.

## 3. Computing the scores

### a) Total instances
Sum all per-tactic counts across all sentences. Call this `total_instances`.

### b) Variety (the most important component)
The number of *distinct* tactics with at least one instance.

- For verbal-only text input: variety is 0–9.
- For audio/video with nonverbal: variety is 0–12.

Antonakis's training emphasizes range over repetition. The "Can Charisma Be Taught?" intervention explicitly aimed at getting trained managers to use *more different tactics*, not the same tactic more often.

Output: `variety_count` (integer) and `variety_ratio = variety_count / 9` (or `/ 12`).

### c) Proportion (canonical density metric)
The proportion of sentences that contain at least one CLT instance.

```
proportion = (sentences with ≥ 1 CLT) / (total sentences)
```

This is the metric Antonakis et al. (2022) report directly. In their field experiment:
- The **non-charismatic** speech: 39.02% (16 tactics over 41 sentences — note that "16 tactics" here is total instances, not unique sentences with tactics; the proportion reported in the paper is 39.02%).
- The **charismatic** speech: 91.67% (44 tactics over 48 sentences).

For a secondary readability metric, also compute `instances_per_sentence = total_instances / total_sentences`. This can exceed 1.0 when sentences contain multiple tactics. In Antonakis et al. (2022), the charismatic speech runs at ~0.92 instances per sentence, the non-charismatic at ~0.39.

### d) Density per 100 words (secondary, for comparability)
Some users will want a per-word metric for comparing short and long pieces. Compute `density_per_100_words = total_instances / (word_count / 100)` and report it as a secondary number, but anchor scoring on the proportion.

### e) Balance (penalty for concentration)
Penalty for over-representation of one tactic.

- Compute `max_share = (count of most-used tactic) / total_instances`.
- If `total_instances < 3`, set `balance_factor = 1.0` (too few to assess balance).
- Otherwise: `balance_factor = max(0, 1 - max(0, max_share - 0.30) / 0.70)`.

This gives: 30% or less concentration → 1.0; 40% → 0.86; 50% → 0.71; 80% → 0.29; 100% → 0.

Output: `max_share` (float), `dominant_tactic` (string with canonical name), `balance_factor` (float).

---

## 4. Composite score (0–100)

```
proportion_ratio = min(proportion / 0.90, 1.0)
overall_score = round(
    60 * variety_ratio
  + 30 * proportion_ratio
  + 10 * balance_factor
)
```

Why `proportion / 0.90`? The charismatic speech in Antonakis et al. (2022) hit 0.92, so 0.90 is the empirical ceiling for "saturated charismatic content." Above 0.90, additional density is unlikely to help and will probably trigger the human-voice guardrail.

### Interpretation bands (anchored to published benchmarks)

| Range | Verdict | Empirical anchor |
|---|---|---|
| 0–25 | Sparse | Reads as informational or transactional. Below the non-charismatic speech baseline (39% proportion) in Antonakis et al. (2022). |
| 26–45 | Functional | Near the **non-charismatic** baseline from Antonakis et al. (2022). Has rhetorical structure but limited range. |
| 46–65 | Solid | Above the trained-manager baseline; below the charismatic exemplar. One or two missing tactic types would close the gap. |
| 66–82 | Strong | Comparable to charismatic speeches in laboratory experiments. Diverse tactics, well-balanced. |
| 83–100 | Exceptional | At or near the **charismatic** speech ceiling (~92%) from Antonakis et al. (2022). Verify the human-voice guardrail. |

---

## 5. The diagnostic (Mode C only — feeds the rewriter, NOT the score)

After computing the score, produce a separate **diagnostic** object. This is what the rewriter sees in Mode C. The numerical score does NOT travel to the rewriter.

```
diagnostic:
  tactics_absent: [list of tactics with zero instances]
  tactics_present_but_thin: [list of tactics with exactly 1 instance in content >300 words]
  tactics_over_represented: [list of tactics whose individual share > 30% of total instances]
  framing_balance: [is the framing category (tactics 1-5) thin? state which framing tactics are missing]
  substance_balance: [is the substance category (tactics 6-9) thin? state which substance tactics are missing]
  highest_leverage_additions: [2-3 tactics to add, chosen by:
    - content type (see clt-definitions.md "Quick reference" table)
    - whether one category (framing vs substance) is clearly under-served
    - audience context if inferable
    - what's currently absent
    Always pair tactic 8 (ambitious goal) with tactic 9 (goal can be achieved) if recommending tactic 8.
  ]
  cautions: [notes about sensitive topic, length constraints (email), or existing over-saturation]
```

Example diagnostic for a flat all-hands draft:
```
tactics_absent: [metaphor, contrast, sentiment of the collective, ambitious goal, goal can be achieved]
tactics_present_but_thin: []
tactics_over_represented: [rhetorical question (3 of 4 instances = 75%)]
framing_balance: thin — only rhetorical questions present, no metaphor/story/contrast/list
substance_balance: absent — no moral conviction, no sentiment of the collective, no goal pair
highest_leverage_additions: [sentiment of the collective (early), contrast (sharpen the strategic frame), ambitious goal + goal-can-be-achieved (paired)]
cautions: [rhetorical questions over-saturated — do not add more]
```

The rewriter reads this and works from it. The rewriter does NOT see "overall_score: 31."

---

## 6. Edge cases

- **Content < 50 words or < 3 sentences.** Skip the composite score. Report variety and per-tactic counts only. Note "too short for meaningful scoring."
- **Content > 1500 words.** Segment into ~500-word chunks at paragraph boundaries. Score each chunk. Report per-segment scores AND an overall score computed on the full text.
- **Plain text input (no audio/video).** Use 9 as the variety denominator. State in output: "Nonverbal tactics not scored — input is text only."
- **Tweets / very short content (< 280 characters).** Follow Tur, Harstad & Antonakis (2022): code each tactic 0/1 for presence across the whole content, not per-sentence. The composite formula still works.
- **Mixed quotes from third parties.** If the user pastes a transcript containing quoted statements from another speaker, ask whether to score the whole text or only the primary speaker's lines. Default to whole text.
- **Foreign language input.** The framework is language-agnostic, but the published examples are English-centric. Apply the detection cues structurally (parallelism, opposition, comparison) rather than lexically. Flag that scoring confidence is reduced.
- **Speechwriter caveat.** When scoring a speech delivered by someone known to use speechwriters, the score reflects the team's signaling, not solely the speaker's intrinsic charisma. Antonakis raises this point in Jacquart & Antonakis (2015) for presidential speeches and concludes the signaling effect on the audience still holds.

---

## 7. Output

Always use the format in `assets/score-output-template.md`. JSON block first (machine-readable, suitable for the iterate-mode before/after comparison), then a human-readable summary.
