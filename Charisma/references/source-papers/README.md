# Source Papers

This folder holds the 10 PDFs you collected from the project. The skill points here when the user asks for empirical citations or methodology depth.

## The collected set

1. **Antonakis, Fenley & Liechti (2011)** — "Can Charisma Be Taught? Tests of Two Interventions" — *Academy of Management Learning & Education*
   - Filename in project: `Can_Charisma_Be_Taught.pdf`
   - Use for: training protocol, intervention design, d = .62 effect size, the original CLT framework.

2. **Antonakis, Fenley & Liechti (2012)** — "Learning Charisma" — *Harvard Business Review*
   - Filename in project: `lesson_2_reading_HBR_Charismatic_Leadership.pdf`
   - Use for: practitioner-friendly definitions, the Aristotle logos/ethos/pathos framing, all the worked manager examples (Joe, Lynn, Tina, Rami, Serge, Karin, Sally, Gilles, Frank, Mika, Ray) and the famous examples (MLK, JFK, Gandhi, Churchill, Bill Gates, Anita Roddick, Machida).

3. **Antonakis, Bastardoz, Jacquart & Shamir (2016)** — "Charisma: An Ill-Defined and Ill-Measured Gift" — *Annual Review of Organizational Psychology and Organizational Behavior*
   - Filename in project: `Charisma__An_IllDefined_and_IllMeasured_Gift.pdf`
   - Use for: the canonical construct definition ("values-based, symbolic, and emotion-laden leader signaling," p. 304), signaling theory, definitional clean-up.

4. **Antonakis, d'Adda, Weber & Zehnder (2022)** — "'Just Words? Just Speeches?' On the Economic Value of Charismatic Leadership" — *Management Science*
   - Filename in project: `On_the_Economic_Value_of_Charismatic_Leadership.pdf`
   - Use for: the sentence-level coding methodology, the 39% / 92% non-charismatic / charismatic proportion benchmarks, the 17% productivity finding, the up-to-19% public-good contribution finding, the 97% inter-coder agreement number.

5. **Jacquart & Antonakis (2015)** — "When Does Charisma Matter for Top-Level Leaders? Effect of Attributional Ambiguity" — *Academy of Management Journal*
   - Filename in project: `When_Does_Charisma_Matter_for_TopLevel_Leader.pdf`
   - Use for: the per-tactic concise definitions, framing/substance/delivery category structure, the U.S. presidential nominee speech coding (1916-2008), the r = .48 correlation between verbal and nonverbal tactics, the speechwriter caveat.

6. **Jensen, Rohner, Bornet, Carron, Garner, Loupi & Antonakis (2023)** — "Combating COVID-19 with Charisma: Evidence on Governor Speeches in the United States" — *The Leadership Quarterly*
   - Filename in project: `Evidence_on_governor_speeches_in_the_United_States.pdf`
   - Use for: the deep-neural-network auto-coding methodology, the coded 18-sentence governor speech vignette (used in `speech-examples.md` as Example 10), the "5,350 lives saved" projection.

7. **Tur, Harstad & Antonakis (2022)** — "Effect of Charismatic Signaling in Social Media Settings: Evidence from TED and Twitter" — *The Leadership Quarterly*
   - Filename in project: `Effect_of_charismatic_signaling_in_social_media_settings__Evidence_from_TED_and_Twitter.pdf`
   - Use for: written-text extension of CLTs, the tweet coding methodology, Figure 3 showing retweet effects across the 16–44% proportion range, the empirical license for `email-adaptations.md`.

8. **Antonakis, Avolio & Sivasubramaniam (2003)** — "Context and Leadership: An Examination of the Nine-Factor Full-Range Leadership Theory Using the Multifactor Leadership Questionnaire" — *The Leadership Quarterly*
   - Filename in project: `Context_and_Leadership_An_Examination_of_the_Nine.pdf`
   - Use for: background on the broader full-range leadership theory, the MLQ, context effects on leadership perception.

9. **Antonakis (2021)** — "Leadership to Defeat COVID-19" — *Group Processes & Intergroup Relations*
   - Filename in project: `antonakis2021leadershiptodefeatcovid19.pdf`
   - Use for: applied policy implications of charismatic leadership during crisis communication.

10. **Bastardoz & Antonakis (2019/2020)** — "Signaling Charisma" — book chapter
    - Filename in project: `Bastardoz2020Signalingcharisma.pdf`
    - Use for: the clearest exposition of the framing / substance / delivery category structure, the costly-signaling perspective, the "leader's vision needs to be appropriate and realizable" credibility constraint.

## How the skill uses these files

The skill does NOT load these PDFs into context by default — that would consume too many tokens. They are loaded only when the user asks for:

- "What's the empirical basis for X?"
- "Cite a source for this claim."
- "What did Antonakis actually find about Y?"
- "Show me the original paper on Z."

In those cases, the skill will read the relevant PDF (via project knowledge search or direct file reading), find the relevant passage, and quote sparingly (under 15 words per quote, one quote per source) with proper attribution.

When citing in user-facing output, use the canonical short form: *(Antonakis, Fenley & Liechti, 2012)* or *(Antonakis et al., 2022)*.
