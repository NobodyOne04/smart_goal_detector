# smart_goal_detector

Five scikit-learn decision trees, one per SMART criterion, trained on synthetic goals. Everything
is in `smart_goals.ipynb`; results and limits are in `README.md`.

## Commands
```sh
uv sync
uv run python -m ipykernel install --user --name smart-goal-detector --display-name "Python (smart-goal-detector)"
uv run jupyter lab smart_goals.ipynb
# headless check that every cell runs:
uv run jupyter nbconvert --to notebook --execute smart_goals.ipynb --ExecutePreprocessor.kernel_name=python3 --output /tmp/run.ipynb
```

## Layout
- `smart_goals.ipynb`: data generator, features, five trees, comparison, rules, hand-written
  test set, `check_goal()`.
- `pyproject.toml` / `uv.lock`: Python 3.11, scikit-learn, pandas, matplotlib, JupyterLab.
- `docs/adr/`: decision records (`.adr`), index in `docs/adr/README.md`.

## Conventions
- Read the ADR before changing a recorded decision; commit subjects carry its number.
- Report the hand-written row as the real accuracy; the generated split is the easy one.
- Keep the trees readable (`max_depth=6`, `min_samples_leaf=30`): readability is the point.
- No deep-learning dependencies: the notebook doesn't use them.
