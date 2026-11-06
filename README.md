# smart_goal_detector

Checks whether a goal is **SMART** using five small, independent ML models,
one per criterion: **S**pecific, **M**easurable, **A**chievable, **R**elevant, **T**ime-bound.
Each is a scikit-learn decision tree, so you can read the rules it learned.

```
'Get fit.'
→ 1/5 criteria met

'Run 10 km without stopping by 30 June 2027 so I can join the company running team.'
→ 5/5 criteria met — SMART! 🎯
```

## Run it

```sh
uv sync
uv run python -m ipykernel install --user --name chatty-bot --display-name "Python (chatty-bot)"
uv run jupyter lab smart_goals.ipynb
```

Then edit the last cell: `check_goal("your goal here")`.

## How it works

The whole pipeline is in [`smart_goals.ipynb`](smart_goals.ipynb).

1. **Synthetic data.** No public SMART-goal dataset exists, so ~6,000 goals are
   generated from phrase pools. Each criterion is switched on or off
   independently, which gives every goal 5 known labels. 3% of labels are
   flipped to mimic annotator noise.
2. **Features.** Hand-crafted regex features (real deadlines, target numbers
   with dates masked out, unit-aware "too big" amounts, vague words, stated
   reasons) plus TF-IDF word n-grams.
3. **Models.** One `DecisionTreeClassifier` per criterion (depth ≤ 6, ≥ 30
   goals per leaf), compared against logistic regression and random forest.
4. **Evaluation** on generated test data and on 23 goals written by hand in a
   different style.

| Accuracy | S | M | A | R | T |
|---|---|---|---|---|---|
| Generated test split | 0.96 | 0.97 | 0.95 | 0.97 | 0.96 |
| Hand-written goals | 0.78 | 0.87 | 0.87 | 0.87 | 1.00 |

The hand-written row is the realistic one.

## Limitations

- *Achievable* and *Relevant* are judgement calls. The models only learn text
  cues (absurd amounts, "overnight", a stated "so that…" reason), not common
  sense: "Become a millionaire by next month" passes as achievable.
- Reasons phrased unlike the training data ("…to stabilize revenue") are missed.
- Numbers that look like durations ("under 2 hours") are treated as time, not
  as a measurable target.
