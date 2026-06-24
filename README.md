# takemeter

(TODO — one-line project description: fine-tuned classifier for r/Smite discussion posts)

## Label Taxonomy

- **question** — asking for help, advice, or clarification (macro help, new player questions)
  - Example 1: (TODO — link/quote a real post)
  - Example 2: (TODO — link/quote a real post)
- **complaint** — balance issues, bugs, server problems, hitbox frustration
  - Example 1: (TODO)
  - Example 2: (TODO)
- **discussion** — opinions, design philosophy, meta debate (Nocturne ult post fits here)
  - Example 1: (TODO)
  - Example 2: (TODO)
- **showcase** — sharing achievements, tools, resources, or highlights
  - Example 1: (TODO)
  - Example 2: (TODO)

**Hardest edge case:** 
- https://www.reddit.com/r/leagueoflegends/comments/1ubnv66/top_lane_sucks_how_can_i_carry_my_games/: this post could fall under complaint or question because the poster is doing both in one post. I decided to categorize this under question because the complaint being made is context for the question that the poster is asking. 
- https://www.reddit.com/r/leagueoflegends/comments/1ubpf4w/hit_plat_now_feel_flat/: this post could fall under complaint or question. It complains in the title about "feeling flat" but then ends the post with a question to the commmunity. I decided on labeling it discussion because of the final question directed at the community.
- https://www.reddit.com/r/leagueoflegends/comments/1udwgyi/why_the_last_hit_indicator_lowers_the_level_of/: this post's title makes it seem like a discussion post or a question post but the body reads like a complaint. I decided on labeling as a complaint because it doesn't ask a question to the community in the body. 


## Annotated Dataset

- **Source:** (TODO — subreddit, API/tool used, date range scraped)
- **Labeling process:** (TODO — who labeled, single-pass vs. reviewed, how disagreements were resolved)
- **Label distribution:** (TODO — table of counts per label, total examples; confirm no label exceeds 70% and total ≥ 200)

| Label | Count | % of dataset |
|---|---|---|
| question | TODO | TODO |
| complaint | TODO | TODO |
| discussion | TODO | TODO |
| showcase | TODO | TODO |

**Difficult examples:**
1. (TODO — post text/summary) — Decision: labeled as ___ because ___
2. (TODO) — Decision: labeled as ___ because ___
3. (TODO) — Decision: labeled as ___ because ___

## Fine-Tuning Pipeline

- **Base model:** (TODO)
- **Training platform:** (TODO — Colab / local / HuggingFace AutoTrain)
- **Key training decision:** (TODO — e.g., chosen epoch count/learning rate/batch size, with the reasoning or observation that led to it — not "used the default")

## Baseline Comparison

- **Baseline approach:** For the baseline, I used zero-shot classification with a prompted LLM (Groq) on the same 36-post test set used to evaluate the fine-tuned model. The model was given a system prompt that defined each of the four labels with a one-sentence description and one example post per label, then asked to output only the label name. No training data was shown to the model.
I ran two versions of the prompt. The second added tiebreaker rules to help distinguish complaint from discussion, but both scored nearly identically, so the final baseline uses the second version.
- **Results:** Baseline accuracy: 0.556 (evaluated on 36/36 parseable responses)

Per-class metrics (baseline):

| Label | Precision | Recall | F1 | Support |
|---|---|---|---|---|
| discussion | 0.57 | 0.29 | 0.38 | 14 |
| showcase | 0.89 | 0.89 | 0.89 | 9 |
| complaint | 0.17 | 0.17 | 0.17 | 6 |
| question | 0.50 | 1.00 | 0.67 | 7 |
| **accuracy** | | | **0.56** | 36 |
| **macro avg** | 0.53 | 0.59 | 0.53 | 36 |
| **weighted avg** | 0.57 | 0.56 | 0.53 | 36 |

## Evaluation Report and Error Analysis

**Per-class metrics (fine-tuned model):** accuracy 0.556

| Label | Precision | Recall | F1 | Support |
|---|---|---|---|---|
| discussion | 0.55 | 0.79 | 0.65 | 14 |
| showcase | 0.67 | 0.44 | 0.53 | 9 |
| complaint | 0.40 | 0.33 | 0.36 | 6 |
| question | 0.60 | 0.43 | 0.50 | 7 |
| **accuracy** | | | **0.56** | 36 |
| **macro avg** | 0.55 | 0.50 | 0.51 | 36 |
| **weighted avg** | 0.56 | 0.56 | 0.54 | 36 |

**Fine-tuned vs. baseline (same test set):**

| Model | Accuracy |
|---|---|
| Zero-shot baseline (Groq) | 0.556 |
| Fine-tuned DistilBERT | 0.556 |

Fine-tuning improvement: 0.000

**Confusion matrix:**

![Confusion matrix](confusion_matrix.png)

**Wrong prediction analysis:**
1. (TODO — post text/summary, true label, predicted label) — Explanation: (tie to data ambiguity, label boundary, or model behavior)
2. (TODO) — Explanation: ___
3. (TODO) — Explanation: ___

**Reflection — gap between intended and captured behavior:** (TODO — describe a specific failure pattern: a confused label pair, a post type the model struggles with, or a distributional issue. Avoid generic statements like "needs more data.")

## AI Usage and Spec Reflection

**AI tool use:**
1. I asked Claude to help me label the 200+ scraped reddit posts. I reviewed the labels and read every post to double check if the label was accurate or not. I made several changes as a result and noted any edge cases.

2. I asked 


**Spec reflection:** (TODO — one way the spec/rubric helped guide the work, one way the implementation diverged from it and why.)
