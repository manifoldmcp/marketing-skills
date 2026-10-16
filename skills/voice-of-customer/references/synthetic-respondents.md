# Synthetic respondents

AI personas, synthetic users, "digital twins" and AI focus groups: a model answering as if it were a customer. Users ask for them because they are instant and free. What the 2024 to 2026 studies say they can and cannot do.

## The evidence

| Study | Setup | Result |
|---|---|---|
| Park and others, Stanford (November 2024, revised June 2026) | Agents built from two-hour interviews with 1,052 Americans | Matched people's General Social Survey answers at 83% to 86% of how well the same people matched themselves two weeks later; agents given demographics alone reached 74% |
| Peng, Gui, Toubia and 20 others, Columbia ("Digital Twins as Funhouse Mirrors", September 2025, revised April 2026) | 19 pre-registered studies, 164 outcomes, twins built from each person's answers to over 500 questions | Twins were only modestly better than a generic model and correlated with their humans at about r = 0.20. Five distortions: too little individuation, stereotyping, representation bias, ideological bias, hyper-rationality |
| Maier and others, PyMC Labs and Colgate-Palmolive (October 2025) | 57 personal-care concept surveys, 9,300 human responses | Asking a model for a 1 to 5 rating gave unrealistic distributions. Asking for a written answer and mapping it to the scale by embedding similarity reached 90% of human test-retest reliability, with realistic distributions |
| Wang, Morgenstern and Dickerson (Nature Machine Intelligence, 2025) | 4 models against 3,200 people across 16 identities | Models misportray and flatten identity groups; prompting with an identity tends to essentialise it |
| NN/g, Rosala and Moran (June 2024) | Three past studies re-run with synthetic users | Synthetic users said they finished every course (a real learner had finished 3 of 7), praised forums real learners found contrived, listed needs with no priority, and liked every concept |
| Netzer and Sambandam (September 2026) | Review of evaluation methods; 106 to 108 attitude questions, N = 3,063 | Aggregate accuracy looks good even when the model knows little about the person and can hide no individual differentiation at all. Proposes screening questions before trusting twins on them |

The pattern: models reproduce **averages and directions** for well-documented populations and familiar products. They fail on **individuals, variance, minorities and niche buyers**, they agree too readily, and they are more rational and more positive than people. A headline accuracy figure can be matched by a model that knows almost nothing about the person: ask which measure a vendor quotes and what a no-information baseline scores on it.

## Use

- **Fine:** drafting an interview guide or survey and piloting its wording; generating hypotheses and candidate objections to then look for in real posts; orienting in an unfamiliar category; screening many concepts down to a few for real testing, with written answers rather than numeric ratings.
- **Not fine:** validating a concept, a price or a positioning; sizing demand; personas for a niche B2B role; quotes. A synthetic quote presented as a customer's is a fabricated testimonial, which the FTC's rule bans in US marketing ([customer language](customer-language.md#consent-and-the-law)).
- **Always:** label synthetic output as such in the deliverable, keep it out of counts, and check every claim it produces against real voices from [sources](sources.md). A synthetic persona that contradicts what buyers say in public is wrong, not insightful.

**Unknown:** how well any of this holds for B2B buyers; nearly all published tests are US consumers and general attitudes.
