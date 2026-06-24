# Planning

## 1. Community Choice and Reasoning

**Community:** r/LeagueOfLegends 
**Why this community is a good fit:** 
It is one of the largest gaming subreddits with a very high post volume. Posts also span a wide range of topics and intents. 

## 2. Label Taxonomy

Reddit has built-in tags that users can attach to posts, but there are a few that aren't frequently used and a few that are too broad. After reading through posts I've identified the following labels:

- **question** — asking for help, advice, or clarification (macro help, new player questions)
  - Example 1: How do you play around a team with poor macro?
  - Example 2: All football skins are back after 8 years. But why no sweeper rammus chromas available?
- **complaint** — balance issues, bugs, server problems, hitbox frustration
  - Example 1: Last hit indicator in ranked is a terrible addition
  - Example 2: north america server down
- **discussion** — opinions, design philosophy, meta debate 
  - Example 1: Renekton JG
  - Example 2: Comparing Rek'Sai in lower & higher elo brackets
- **showcase** — sharing achievements, tools, resources, or highlights
  - Example 1: MVP T1 Miss Fortune will cost 1,820 RP for 3 patches. Then it will be in the rotating Mythic Shop for 150 Mythic Essence
  - Example 2: T1 Worlds Skins Splash Arts

**Hardest edge cases:**
- A post that complains about something AND asks how to fix it (e.g. "last hit indicator is terrible, how do I turn it off?"). I'll call it a question if it ends with an actual ask, complaint if it's just venting.
- Balance/meta posts (like "Renekton JG") can sound like either a complaint or a neutral discussion depending on tone. I'll label it discussion if it invites other opinions, complaint if it's just "this is broken."
- Skin posts sometimes include opinions ("$20 is overpriced"). If the post is mainly showing off the skin, it's showcase; if the opinion is the main point, it's discussion.


## 3. Data Collection Plan

- Source: https://www.reddit.com/r/leagueoflegends/
- Target volume: at least 200 labeled examples (per rubric minimum)
- If a label is underrepresented, I will reframe the labels to be more specific or categorize the posts differently.

## 4. Evaluation Metrics

- Primary metric: macro F1 (averages F1 across all 4 labels equally).
- Secondary metrics: per-class precision/recall + confusion matrix.

Accuracy isn't enough since discussion/complaint posts will probably be way more common than showcase/question posts, so a model could score high just by nailing the common labels. Macro F1 weighs every label the same, so it actually punishes bad performance on the rare labels. The confusion matrix and per-class scores also show me which specific labels get mixed up, which one accuracy number can't.

## 5. "Good Enough" Performance Threshold

- Target: macro F1 >= 0.75 on the test set, no single label below 0.6 F1. With only 4 distinct labels this feels achievable if the model is actually learning, and the per-label floor stops it from ignoring rarer labels.
- Baseline: same base model, zero-shot prompted with the label definitions, no fine-tuning. Fine-tuning should beat this baseline's macro F1, or it wasn't worth doing.

## 6. AI Tool Plan

I plan to use AI for two things:
- **Label stress-testing:** before I lock in the label definitions, I'll feed an LLM the borderline posts from my edge cases list above and see what it picks. If it gets confused in the same ways I would, that tells me the definitions need a clearer tiebreak rule.
- **Annotation assistance:** I'll have an LLM pre-label a batch of raw posts, then go through and correct any wrong labels myself before they go in the final dataset. 
