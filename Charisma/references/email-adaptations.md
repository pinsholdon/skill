# Email Adaptations

Read this file whenever the input is an email, a short memo, a Slack message, a LinkedIn post, or any short written form. It governs how the 9 verbal CLTs translate from speech to written text and what to do about the 3 nonverbal tactics.

---

## Empirical basis

The license for applying CLTs to written text comes from **Tur, Harstad & Antonakis (2022). "Effect of charismatic signaling in social media settings: Evidence from TED and Twitter."** *The Leadership Quarterly*, 33(5), 101476.

That paper coded TED transcripts and individual tweets using only the 9 verbal tactics. Three trained coders rated each tweet for the presence (0/1) of each of the 9 verbal tactics. Summed across tactics, the resulting "charismatic signaling score" predicted retweet counts in a Poisson regression — and predicted TED talk views in the parallel study.

The paper's own framing: "Our work also makes a contribution by extending the use of objectively-measured charismatic signaling to written communication. That we can measure charisma's impact directly in text-based communication, and this independent from nonverbal signals, suggests new research avenues should be explored" (Tur, Harstad & Antonakis, 2022).

Quantitative anchors from the Twitter study:
- 2,964 tweets from 60 tweeters (30 CEOs + 30 US Congress members).
- At the time of data collection (Dec 2011), tweets were limited to 140 characters.
- Figure 3 of the paper shows: as the proportion of charismatic signaling in tweets rises from ~0.16 to ~0.44, predicted retweets rise from ~3 to ~10 (roughly a 3× increase across the empirical range).
- The deep-learning automated coder developed by Garner et al. (2019) correlated r = .59 with average human coders on the same tweets.

The takeaway for emails: the 9 verbal tactics work in short written text, but only when used at proportions broadly comparable to the tweet study (the empirical range is roughly 16% to 44% of content carrying tactics, with diminishing returns above that).

---

## What changes in short written form

### 1. Density ceiling is lower
For emails under 200 words, do not exceed 4 distinct tactics. For emails under 100 words, do not exceed 3. The Tur et al. (2022) tweet data caps out around 44% — that's the empirical ceiling for short written content.

A 100-word email with 8 tactics reads as manipulative or AI-generated. A 100-word email with 2–3 well-placed tactics reads as a leader who knows what they're doing.

### 2. Stories must be tiny
A "story" in a speech can be a paragraph. A "story" in an email is one sentence — sometimes a fragment.

- Speech version (40 words): "Last week I got an email from a customer named Maria. She had been trying to reset her password for two hours. She finally just gave up and called her bank to switch providers. That email changed how I think about our roadmap."
- Email version (15 words): "Last week a customer named Maria gave up on us after two hours. That changed everything."

### 3. Three-part lists must be compact
- Speech version: "We will ship faster, we will listen harder, and we will measure what actually matters to the people who use this product."
- Email version: "Ship faster, listen harder, measure what matters."

### 4. Rhetorical questions go in subject lines, not bodies
A rhetorical question buried in the third paragraph of an email gets skimmed. A rhetorical question as the subject line or opening line works.

- Strong subject: *"What if Thursday was the last meeting on this?"*
- Weak placement: *"...so I want to ask all of us: what if Thursday was the last meeting on this? Anyway, here's the agenda..."*

### 5. Sentiment of the collective goes in the opening
The most powerful place for sentiment of the collective in an email is the first or second sentence. By the third paragraph, the reader has either bought in or moved on.

### 6. Moral conviction goes in the closing
Conversely, moral conviction lands best at the end of an email — as a commitment the sender carries forward into the work.

### 7. The ambitious goal + goal-can-be-achieved pair compresses
In speeches, these two tactics often take a paragraph each. In emails, they can fit in one sentence: "If we choose right, this becomes the project we point to next year." The goal and the implicit warrant ("if we choose right") fit naturally in short form.

---

## The 3 nonverbal tactics in plain text

These do not translate literally. Plain text has no voice, no face, no gesture. But each has an imperfect channel substitute:

| Nonverbal tactic | Text substitute | Use sparingly |
|---|---|---|
| Animated voice | Sentence-length variation (a very short sentence after a long one creates emphasis); selective dashes for pacing | Yes — alternating cadence is powerful, but every email cannot read like prose poetry |
| Facial expressions | Opening line tone (warmth, gravity, urgency) sets the "expression" the reader hears | Yes — opening line carries weight; subject line carries even more |
| Gestures | Strategic bold or italic emphasis on one or two key words per message; em-dashes for pointing | Yes — more than 2 instances of bold/italic per email looks like a marketing email |

**Do NOT score these as nonverbal tactics in the JSON output.** Set `nonverbal_scored: false` and the three delivery counts to `null`. State explicitly that nonverbal tactics are not scored. Mention the text substitutes only as suggestions in the rewriting rationale, never as scored items.

---

## Tactic priority by email type

This table refines the general one in `clt-definitions.md` for written short-form.

| Email type | First-choice tactics | Cap |
|---|---|---|
| Request (asking for time, input, decision) | sentiment of the collective → moral conviction → three-part list | 3 tactics, ≤ 150 words |
| Bad news (layoff, delay, decision against the recipient) | moral conviction → sentiment of the collective. Stop there. | 2 tactics, restraint |
| Call to action (rally, push, deadline) | contrast → three-part list → goal can be achieved | 3 tactics |
| Update / FYI | none required; one well-placed three-part list is plenty | 1 tactic |
| Apology | moral conviction (accepting responsibility) → sentiment of the collective → goal can be achieved (about what changes) | 3 tactics |
| Recognition / thanks | story (one sentence) → moral conviction → goal can be achieved | 3 tactics |
| Pitch / cold outreach | contrast → rhetorical question (subject line) → goal can be achieved | 3 tactics |

---

## Subject lines

The subject line is the single most important sentence in any email. CLT-wise:

- **Contrast** in a subject line is high-leverage: *"Not another standup — a different approach"*
- **Rhetorical question** in a subject line is high-leverage: *"What's stopping us from shipping by April?"*
- **Three-part list** in a subject line is risky — feels like marketing copy.
- **Metaphor** in a subject line is risky — feels coy.
- **Moral conviction** in a subject line is high-leverage when warranted: *"What we owe the team this week"*

When scoring an email, score the subject line as part of the content (it counts as a sentence). When rewriting, the subject line is fair game.

---

## What NOT to do in email rewrites

- **Do not turn an email into a speech.** A 6-paragraph email with 8 tactics is not a great email; it's a bad speech in the wrong medium.
- **Do not stack tactics on the opening line.** "I know you're busy, but what if we did things differently? Picture this: a team that ships, a team that listens, a team that measures." This is four tactics in one breath. It reads as AI.
- **Do not insert a metaphor when the email is purely transactional.** "Here are the Q3 numbers" does not need a metaphor.
- **Do not add a story to a status update.** Stories are for moments of human connection, not for routine reporting.
- **Do not insert rhetorical questions you would not say aloud to the recipient's face.** "Are we really going to keep accepting this?" sent in a Slack DM reads as passive-aggressive.

---

## Calibration anchor

A well-formed leadership email of 100–150 words typically scores in the **45–65 range** on the composite (variety 3–4 out of 9, proportion 35–50%, balance ≥ 0.85). Going much higher than that risks the human-voice guardrail. Going much lower means the email is purely informational — which is sometimes correct and sometimes a missed opportunity.

When scoring an email, calibrate against this range. A **55** on an email is excellent. A **55** on a 30-minute keynote is below average.
