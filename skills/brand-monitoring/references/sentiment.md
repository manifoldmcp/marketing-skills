# Sentiment and what a mention needs

How far to trust "positive, negative, neutral", and the classification that serves a brand better. Checked October 2026.

## What the evidence says

- **Models are fine on plain polarity and weaker on everything else.** Across 13 tasks and 26 datasets, large language models performed well on simple sentiment classification but lagged on tasks needing deeper understanding or structured sentiment, such as which aspect of a product the opinion is about (Zhang et al., "Sentiment Analysis in the Era of Large Language Models", May 2023). Brand mentions are mostly the hard kind: "switched from Acme, the API was fine but support never answered" is one mention with two opinions.
- **Sarcasm is still hard.** SarcasmBench tested eleven models and found sarcasm understanding well behind sentiment (2024); on a seven-way sarcasm-type task the best macro F1 was 0.37 (Sarc7, Gemini 2.5, 2025). Social posts about brands are often sarcastic.
- **Lexicon tools are worse on social text.** Word-score methods (VADER and the like) are inspectable but blind to context, slang and mixed languages.
- **Nobody has published a benchmark on brand mentions.** Vendor sentiment accuracy claims come without a labelled test set. Treat a dashboard's sentiment share as a rough signal, not a measurement.

## Classify by what the mention asks for

A sentiment score does not say what to do. A kind does:

| Kind | Example | Action |
|---|---|---|
| Complaint | "Acme's export broke again" | Pass to support; reply if public and the community allows |
| Question | "Does Acme work with Shopify?" | Answer, or note the gap in the docs |
| Comparison | "Acme vs Beta for a small team?" | Watch; answer only with an honest comparison |
| Recommendation | "We use Acme, it's great for X" | Thank; save as proof, with permission |
| Praise | "Acme's new dashboard is lovely" | Thank; save as a testimonial, with permission |
| Churn or switch | "We moved off Acme to Beta" | Read in full; it is the most useful mention there is |
| News or launch | A post about the brand's own announcement | Count separately; it measures reach, not opinion |

- **Mixed is a valid answer.** A switch story that praises one feature and leaves over another is both; record both aspects rather than forcing one label.
- **Read the thread, not the line.** A comment's meaning often depends on what it answers. Reddit comment rows carry `post_title` and `parent_id`; read those before labelling.
- **Leave out what is not organic.** The brand's own accounts, partner posts (#AcmePartner), paid posts (`is_ad` true) and seeded "review" threads (see [monitoring](monitoring.md#reading-the-rows)) are not public opinion.
- **Report counts by kind with examples, not a percentage positive.** "Four complaints this week, three about exports, linked" is actionable; "62% positive" is not, and on a sample of twenty it moves 20 points with four posts.

## When a model labels the mentions

- Give it the thread context and the kinds above, ask for the kind, the aspect and a one-line reason, and spot-check twenty labels by hand before reporting any share. Disagreement on more than a few means the labels need a human pass.
- Keep the label with the row, so a later run can be compared on the same scheme. A changed scheme starts a new baseline.
