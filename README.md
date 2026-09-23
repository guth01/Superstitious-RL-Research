# Superstitious Behavior in Reinforcement Learning Agents

Code, data, and analysis for *"Superstitious Behavior in Reinforcement Learning Agents: A
Multi-Algorithm, Multi-Schedule Replication of Skinner's Noncontingent Reinforcement Paradigm."*

We test whether RL agents develop Skinner-style "superstitious" behavior — stereotyped action
patterns locked to a reward schedule that is, by construction, blind to behavior — across four
algorithms (tabular Q-learning, DQN, PPO, A2C), three noncontingent reward schedules (fixed,
variable, random interval), and a curiosity-bonus moderator, inside a single-state Skinner-box
environment.

## What's in this repository

```
.
├── superstitious_rl_extended_study_v2.ipynb   # full experiment: env, agents, training, analysis
├── superstitious_rl_paper_v2.tex              # paper (Springer Nature sn-jnl format)
├── fig_tierA_entropy.png                      # Tier A entropy trajectories (algorithm comparison)
├── fig_tierB_entropy.png                      # Tier B entropy trajectories (schedule sensitivity)
├── fig_tierC_entropy.png                      # Tier C entropy trajectories (curiosity moderator)
├── fig_tierD_entropy.png                      # Tier D entropy trajectories (interval sensitivity)
└── results/
    ├── per_seed_summary.csv                   # every per-seed value behind every table in the paper
    ├── raw_logs.pkl                           # full entropy/ritual logs for every run, every seed
    ├── statistical_tests_corrected.csv        # every test run, raw + Holm-Bonferroni-corrected p
    └── dqn_hyperparameter_overrides.csv       # DQN override values vs. SB3 defaults
```

## Reproducing the results

1. Open `superstitious_rl_extended_study_v2.ipynb` in Colab or Jupyter.
2. Run all cells top to bottom. Runtime is roughly [fill in — e.g. "~2 hours on CPU"] for the
   full battery: Tiers A–D across four algorithms, eight seeds per condition.
3. All figures, tables, and the `results/` files listed above are regenerated in place.

No GPU is required or particularly helpful — see the notebook's early cells for why (the
environment and networks involved are small enough that the bottleneck is per-step Python/gym
overhead, not compute).

## Study design

Four experimental tiers, each with 8 seeds per condition:

- **Tier A** — all four algorithms × {noncontingent, contingent} reward, asking whether entropy
  collapse under noncontingent reward is algorithm-dependent.
- **Tier B** — Q-learning and PPO × {fixed, variable, random} schedule, asking whether the effect
  is schedule-sensitive as the animal literature predicts.
- **Tier C** — Q-learning and PPO × {curiosity bonus off, on}, asking whether exploration pressure
  dilutes or compounds ritual formation.
- **Tier D** — Q-learning × {I=5, I=10, I=20} interval length, a post-hoc robustness check on
  Tier B's finding that fixed-interval scheduling drives the effect.

Every statistical test across all four tiers (Mann-Whitney U / Kruskal-Wallis, with bootstrap 95%
CIs and epsilon-squared effect sizes) is corrected for multiple comparisons via Holm-Bonferroni,
applied within metric-specific families. See `results/statistical_tests_corrected.csv` for the
full test log, and Section 3.4 / 4.6 of the paper for methodology and a summary of which findings
survive correction.

## Citation

If you use this code or data, please cite:

```bibtex
@article{praveen_superstitious_rl,
  title   = {Superstitious Behavior in Reinforcement Learning Agents: A Multi-Algorithm,
             Multi-Schedule Replication of Skinner's Noncontingent Reinforcement Paradigm},
  author  = {Praveen, Gautham and Arora, Arisha and Sikder, Sayan},
  year    = {2026},
  note    = {Preprint / in submission}
}
```

A Zenodo DOI for this exact release will be added here once minted — see the paper's Data
Availability statement for the archived version.

## License

MIT

## Contact

Corresponding author: Gautham Praveen (gautham.praveen2023@vit.ac.in)
