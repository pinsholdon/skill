# Score Output Template

Every Mode A output and both score passes in Mode C use this format. JSON block first (machine-readable, used to compute the before/after delta in Mode C), then a human-readable summary.

The tactic keys in the JSON use Antonakis's canonical labels and the three-category structure from `references/clt-definitions.md`.

---

## Format

````
```json
{
  "content_type": "speech | email | memo | linkedin_post | tweet | other",
  "word_count": <int>,
  "sentence_count": <int>,
  "nonverbal_scored": <true | false>,
  "framing_tactic_counts": {
    "metaphor": <int>,
    "rhetorical_question": <int>,
    "story": <int>,
    "contrast": <int>,
    "list": <int>
  },
  "substance_tactic_counts": {
    "moral_conviction": <int>,
    "sentiment_of_the_collective": <int>,
    "ambitious_goal": <int>,
    "goal_can_be_achieved": <int>
  },
  "delivery_tactic_counts": {
    "animated_voice": <int | null>,
    "facial_expressions": <int | null>,
    "gestures": <int | null>
  },
  "total_instances": <int>,
  "sentences_with_tactic": <int>,
  "proportion": <float 0-1, 2 decimal places>,
  "instances_per_sentence": <float, 2 decimal places>,
  "density_per_100_words": <float, 2 decimal places>,
  "variety_count": <int>,
  "variety_ratio": <float, 2 decimal places>,
  "framing_present": <int 0-5>,
  "substance_present": <int 0-4>,
  "dominant_tactic": "<canonical tactic name>",
  "max_share": <float, 2 decimal places>,
  "balance_factor": <float, 2 decimal places>,
  "overall_score": <int 0-100>,
  "band": "Sparse | Functional | Solid | Strong | Exceptional"
}
```

## Summary

**Overall: <score>/100 — <band>**

<one-paragraph qualitative assessment: what's working, what's missing, calibrated against the content type. Reference the empirical anchor — e.g., "this sits below the non-charismatic baseline from Antonakis et al. 2022 (39% proportion)" — when useful.>

**Category balance:**
- Framing: <int>/5 tactics present
- Substance: <int>/4 tactics present
- <which category, if either, is under-served>

**Tactic breakdown:**
- Strongest: <tactic> (<count> instances) — <one-line note>
- Missing or thin: <list of absent or thin tactics> — <one-line note on highest-leverage to add>
- <If applicable> Over-represented: <tactic> at <share>% — <warning if > 40%>

**Diagnostic** (used by the rewriter — Mode C only):
- Tactics absent: <list using canonical labels>
- Tactics over-represented (> 30% share): <list with shares>
- Framing balance: <thin / adequate / saturated>, with <which framing tactics missing>
- Substance balance: <thin / adequate / saturated>, with <which substance tactics missing>
- Highest-leverage additions for this content type: <2-3 tactics, pair ambitious_goal with goal_can_be_achieved>
- Cautions: <sensitive-topic, length, or saturation notes>
````

---

## Notes on filling the template

- `sentence_count` is the number of sentences after splitting on punctuation. Fragments separated by em-dashes or semicolons that could stand alone count as separate sentences.
- `sentences_with_tactic` is the number of sentences containing at least one CLT instance. The `proportion` field is `sentences_with_tactic / sentence_count`.
- `total_instances` is the sum of all per-tactic counts. It can exceed `sentence_count` when sentences contain multiple tactics.
- `nonverbal_scored` is `false` for plain text input. In that case, set the three delivery counts to `null` and use 9 as the variety denominator.
- `variety_count` is the number of *distinct* tactics with at least 1 instance.
- `variety_ratio` = `variety_count / 9` for text, `/ 12` for audio/video.
- `framing_present` is the number of framing tactics (out of 5) with ≥ 1 instance.
- `substance_present` is the number of substance tactics (out of 4) with ≥ 1 instance.
- `max_share` = `(count of most-used tactic) / total_instances`. If `total_instances < 3`, set `balance_factor = 1.0`.
- `balance_factor` = `max(0, 1 - max(0, max_share - 0.30) / 0.70)`. Round to 2 decimals.
- `overall_score` = `round(60 * variety_ratio + 30 * min(proportion / 0.90, 1.0) + 10 * balance_factor)`.
- `band` from the table in `references/scoring-rubric.md`:
  - 0–25 Sparse
  - 26–45 Functional
  - 46–65 Solid
  - 66–82 Strong
  - 83–100 Exceptional
- The **Diagnostic** block appears in Mode C only. In Mode A, omit it.

---

## Example output for a 110-word, 7-sentence all-hands draft

````
```json
{
  "content_type": "speech",
  "word_count": 110,
  "sentence_count": 7,
  "nonverbal_scored": false,
  "framing_tactic_counts": {
    "metaphor": 0,
    "rhetorical_question": 1,
    "story": 0,
    "contrast": 0,
    "list": 0
  },
  "substance_tactic_counts": {
    "moral_conviction": 1,
    "sentiment_of_the_collective": 0,
    "ambitious_goal": 0,
    "goal_can_be_achieved": 0
  },
  "delivery_tactic_counts": {
    "animated_voice": null,
    "facial_expressions": null,
    "gestures": null
  },
  "total_instances": 2,
  "sentences_with_tactic": 2,
  "proportion": 0.29,
  "instances_per_sentence": 0.29,
  "density_per_100_words": 1.82,
  "variety_count": 2,
  "variety_ratio": 0.22,
  "framing_present": 1,
  "substance_present": 1,
  "dominant_tactic": "rhetorical_question",
  "max_share": 0.50,
  "balance_factor": 1.00,
  "overall_score": 33,
  "band": "Functional"
}
```

## Summary

**Overall: 33/100 — Functional**

This all-hands opening sits just below the non-charismatic baseline from Antonakis et al. (2022) (39% proportion). It communicates the decision clearly but lacks the rhetorical structure that turns a directive into a rallying point. Two tactics in 7 sentences — one rhetorical question, one moral conviction — give the piece a faint pulse but little variety.

**Category balance:**
- Framing: 1/5 tactics present (only rhetorical question)
- Substance: 1/4 tactics present (only moral conviction)
- Both categories under-served, with substance the larger gap given this is a strategic-pivot announcement

**Tactic breakdown:**
- Strongest: rhetorical question (1 instance) — well-placed but isolated
- Missing or thin: metaphor, story, contrast, list, sentiment of the collective, ambitious goal, goal can be achieved
- Balance is healthy only because total instances are too few to over-concentrate

**Diagnostic** (for the rewriter):
- Tactics absent: metaphor, story, contrast, list, sentiment of the collective, ambitious goal, goal can be achieved
- Tactics over-represented: none (total too low)
- Framing balance: thin — only rhetorical question present; recommend adding contrast (Antonakis notes this is one of the easiest framing tactics to add)
- Substance balance: thin — only moral conviction present; recommend adding sentiment of the collective and the ambitious goal + goal-can-be-achieved pair
- Highest-leverage additions for all-hands strategic pivot: sentiment of the collective (early), contrast (sharpen the strategic frame), ambitious goal + goal-can-be-achieved (paired)
- Cautions: original is 110 words; keep total tactics ≤ 5 — adding all 7 missing tactics would push toward parody

[clt-skill v2.1 · score:33 · variety:2/9 · F1/5·S1/4 · prop:0.29]
````

---

## Mode C — before/after report format

In Mode C, after running the second score pass on the rewrite, present this comparison block at the end:

```
## Before / After

|  | Original | Rewrite | Δ |
|---|---|---|---|
| Overall score | <int> | <int> | <±int> |
| Variety | <int>/9 | <int>/9 | <±int> |
| Proportion | <float> | <float> | <±float> |
| Framing tactics present | <int>/5 | <int>/5 | <±int> |
| Substance tactics present | <int>/4 | <int>/4 | <±int> |
| Balance | <float> | <float> | <±float> |
| Band | <band> | <band> | — |

## Qualitative guardrails on the rewrite

- Meaning preservation: PASS | FLAG — <one sentence>
- Tactic balance: PASS | FLAG — <one sentence>
- Human voice: PASS | FLAG — <one sentence>

## Next step

The rewrite added: <list of tactics added with canonical names>. The rewrite deliberately did not add: <list, with one-line reason for each>.

Want me to push harder on a specific tactic, swap one of the additions, or back off somewhere? Or is this where it should land?

[clt-skill v2.1 · iterate · score:<before>→<after> · variety:<before>→<after> · added:<comma-separated tactics>]
```
