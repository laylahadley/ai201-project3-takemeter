# TakeMeter Planning

## Community
r/stocks is a large public forum where retail investors discuss individual companies, earnings, and market moves. I classify posts into analysis, hot_take, and reaction. These distinctions matter to regulars because the community constantly debates who is doing real research versus repeating hype or panic, and confusing the two can cost real money.

Collection filter: exclude pure questions, link-only posts, bot comments, and comments under ~15 words.

## Labels

### analysis
Definition: Argues a claim using specific, verifiable evidence that would still support the claim if the opinion framing were removed.
- Example 1: "About 60% of the stocks in the S&P 500 are down more than 20% from their all-time highs."
- Example 2: "The S&P 500 has 500 companies, but a relatively small group is driving an increasing share of the index. Apple and Nvidia now account for more than 15% of the S&P 500, while the 10 largest holdings are approaching 40%. A decade ago, the top 10 were around 17%. Apple is up about 25% YTD and Nvidia about 21%, which has increased their weight as their market caps have grown.
- Uncertain: "Their P/E is 45, way above the sector average of 25. Overvalued, sell." It has a real number, but is it an argument or decoration?

### hot_take
Definition: A confident opinion or prediction with no real support, or with vague, cherry-picked, or decorative evidence.
- Example 1:  "I'm very bullish RAM names, and only modestly overweight NAND. This is because the RAM producers have enough disclosure that I can figure out their wafer starts and bit shipments (i.e. I know where their fabs are, what the capacities are, when they come on and how bits scale by node). Because I know how much and when bits will grow, I can see that demand continues to outpace supply for the next few years.
- Example 2: "I am more of a macro trends guy than a company numbers guy, here is my take. Bull Case: Compute is eating the world, to be effective compute needs storage. Micron and Sandisk are two of the last few standing, there are not enough players left to effectively create the bust cycle that has dogged RAM since we had RAM. Bear Case: all of that is true.
- Uncertain: "This company has no moat and will lose to competitors. Just look at how they missed last quarter." It gestures at evidence without specifics.

### reaction
Definition: An immediate emotional response to a specific event, with little or no argument.
- Example 1: "Amazing stock! Everything in the world is electrifying and guess who’s building the infrastructure for energy distribution"
- Example 2: "A market cap of $461B for a company that made 2.5B in FCF - SBC over the past year. Clown market."
- Uncertain: "Down 12% again. Management is clearly clueless, I'm done." The emotion is the core, but it contains a verdict.

## Decision rules
1. Evidence test: remove the opinion words. If specific, verifiable evidence remains that supports the claim, label analysis. Otherwise hot_take.
2. Emotion vs. verdict: if the main content is a feeling about an event, label reaction; if the main content is a claim, label hot_take.
3. Tiebreak: choose the label describing the majority of the post's words.

## Hardest edge case
Post: "NVDA at 40x forward earnings is insane. Every bubble looks exactly like this. Sell now."

Could be: analysis or hot_take.

Rule: Evidence test. 

## Data collection plan

I'll collect at least 200 examples from r/stocks, targeting roughly 70 each across analysis, hot_take, and reaction (a ~1:1:1 split rather than mirroring the subreddit's natural imbalance, so the model isn't trained on a skewed prior). Sources:
- Daily discussion threads and earnings-reaction threads for hot_take and reaction (high volume, fast to collect)
- DD (due diligence) posts and their top comments for analysis (lower volume, so I'll deliberately seek these out rather than sampling randomly)

If a label is underrepresented after collecting 200 total, I will not force weak examples into it. Instead I'll continue targeted collection from analysis-heavy threads (DD posts, earnings threads with numbers cited) until that label reaches a workable count, even if total examples exceed 200.

Note - Why the equal split: if hot_take and reaction dominate at 5:1 over analysis (realistic for r/stocks), the model can hit decent accuracy by mostly ignoring analysis — that inflates overall accuracy while hiding a real weakness, which is exactly what per-class metrics below are meant to catch.

## Evaluation metrics

- **Overall accuracy** for both models, as a baseline comparison point.

- **Per-class precision, recall, and F1** for both models. Accuracy alone can hide a model that's strong on hot_take/reaction (the majority classes in practice) but weak on analysis. Recall on analysis matters specifically: missing genuine analysis (mislabeling it as hot_take) is the more costly error for a tool meant to surface substantive discussion.

- **Confusion matrix** to see which label pairs the model confuses most, since my hardest edge case (analysis vs. hot_take) is the boundary I expect the most errors on.


## Definition of success

    - I'll consider the fine-tuned model successful if it achieves at least 70% overall accuracy and at least 0.60 F1 on each individual class (not just the majority classes) on the held-out test set, while outperforming the zero-shot Groq baseline on at least one metric. A model that's accurate only on hot_take/reaction but weak on analysis would not meet this bar, since analysis is the label most useful for the tool's intended purpose.

## AI Tool Plan

- **Label stress-testing:** Before annotating 200 examples, I'll give an AI tool my three label definitions and hardest edge case, and ask it to generate 5-10 borderline posts. If it produces posts I can't cleanly classify using my existing decision rules, I'll tighten the definitions before starting annotation.
- **Annotation assistance:** I will not use an LLM to pre-label examples; all 200+ examples will be hand-labeled by me, using the decision rules from planning.md. 
- **Failure analysis:** After evaluation, I'll give the list of the model's wrong predictions to an AI tool and ask it to identify patterns (e.g., consistently confusing one label pair, or failing on short posts). I'll verify any claimed pattern myself by rereading the flagged examples before including it in the reflection.


## Metric reasoning
(to be written)

## Success threshold
(to be written)

## AI Tool Plan
(to be written)