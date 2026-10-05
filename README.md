# Superstition-Like Behavioral Fixation in Reinforcement Learning Agents

Code, data, and analysis for *"Superstition-Like Behavioral Fixation in Reinforcement Learning
Agents under Noncontingent Reward: A Multi-Algorithm, Multi-Schedule Empirical Study in a
Single-State Environment."*

We ask whether RL agents develop Skinner-style "superstitious" behavior, meaning stereotyped action
patterns locked to a reward schedule that is, by construction, blind to behavior. We study four
algorithms (tabular Q-learning, DQN, PPO, A2C), three noncontingent reward schedules (fixed,
variable, random interval), a contingent control, and a count-based exploration bonus, all inside a
single-state, four-action Skinner-box environment.

**Scope.** This is a bounded empirical demonstration: one abstract environment with a constant
observation, one type of exploration bonus, behavioral metrics only, and no test of the underlying
mechanism. See the paper's Limitations section before drawing broader conclusions.

## What's in this repository

```
.
├── superstitious_rl_extended_study_v3.ipynb   # full experiment: env, agents, training, analysis
├── figs/
│   ├── fig1_tierA.png                         # Tier A: algorithm comparison (entropy trajectories)
│   ├── fig2_tierB.png                         # Tier B: schedule sensitivity
│   ├── fig3_tierC.png                         # Tier C: count-based bonus
│   ├── fig4_tierD.png                         # Tier D: interval length
│   ├── fig5_dqn_ablation.png                  # Tier E: DQN hyperparameter sensitivity
│   └── fig6_a2c_adequacy.png                  # Tier F: A2C training-adequacy test
└── results/
    ├── per_seed_summary.csv                   # every per-seed value behind every table in the paper
    ├── raw_logs.pkl                           # full entropy/ritual logs for every run, every seed
    ├── statistical_tests_corrected.csv        # every test run: raw + Holm-Bonferroni-corrected p
    ├── test_traceability_table.csv            # same tests with family, m, Holm rank, multiplier
    ├── test_traceability_table.tex            # LaTeX version of the traceability table
    ├── dqn_hyperparameter_overrides.csv       # DQN override values vs. SB3 defaults (read from library)
    └── run_metadata.json                      # SB3 version, seed counts, null baselines, power results
```

## Reproducing the results

1. Open `superstitious_rl_extended_study_v3.ipynb` in Colab or Jupyter.
2. Run all cells **top to bottom, in order**. Later supplementary cells depend on variables defined
   earlier (for example the null baselines and the Tier A algorithm list). Runtime is roughly
   15 minutes for the full battery.
3. All figures, tables, and the `results/` files listed above are regenerated in place.

Stable-Baselines3 **2.9.0** was used for DQN, PPO and A2C (the notebook prints the installed version
at the top of the run). No GPU is required: the environment and networks are small enough that the
bottleneck is per-step Python/gym overhead, not compute. Note that a GPU runtime is harmless; the
notebook handles device placement for its one torch check.

### Configuration knobs (first cells of the notebook)

| Setting | Default | Meaning |
|---|---|---|
| `N_SEEDS` | 8 | seeds per condition (0 to 7) |
| `N_SEEDS_PPO_EXT` | 24 | seeds for PPO in Tiers B and C (seeds 0-7 are the original runs; 8-23 added for power) |
| `EQUIV_MARGIN` | 0.5 | equivalence margin in bits for the PPO "no effect" checks |
| `RUN_TIER_D_SCHEDULES` | `True` | run variable/random schedules at I = 5 and 20 (Tier D2) |
| `N_NULL_SIMS` | 2000 | simulations for the uniform-random-policy null baselines |

## Study design

The constant observation means the network-based agents (DQN, PPO, A2C) can only learn a bias vector
over actions, not a state-conditioned policy. Algorithm differences should be read as differences in
credit-assignment mechanics on a degenerate input.

Fixation is measured with trailing action entropy and a reward-locked ritual-strength statistic, and
is compared against a simulated **uniform-random-policy null** (mean final entropy 1.989 bits; mean
ritual strength 0.346). In the contingent condition ritual strength is true by construction, so the
informative measure there is the **target rate** (chance = 0.25).

Pre-specified design (confirmatory, 14 tests in three Holm families):

- **Tier A**: four algorithms x {noncontingent, contingent}, 8 seeds each.
- **Tier B**: Q-learning (8 seeds) and PPO (24 seeds) x {fixed, variable, random} schedule.
- **Tier C**: Q-learning (8 seeds) and PPO (24 seeds) x {count-based bonus off, on}.

Added after seeing the Tier A-C results (exploratory; corrected within their own families):

- **Tier D** (post hoc robustness): Q-learning x {I = 5, 10, 20} interval length.
- **Tier D2**: variable and random schedules at I = 5 and 20 versus fixed.
- **Tier E**: DQN hyperparameter-sensitivity ablation (buffer/target update, discount, learning rate).
- **Tier F**: A2C training-adequacy test (default rollout length; 3x steps).
- **Supplementary S1/S2**: null-baseline tests and Tier B pairwise comparisons.

All tests (Mann-Whitney U, Kruskal-Wallis, one-sample Wilcoxon, with bootstrap 95% CIs and
epsilon-squared effect sizes) are corrected with Holm-Bonferroni within metric families. In total 56
tests are logged: 14 confirmatory, 2 post-hoc robustness, and 40 supplementary. The confirmatory
families have m = 9 (final entropy), 4 (ritual strength) and 1 (ritual streak). Section 3.4 and
Appendix A of the paper, and `results/test_traceability_table.csv`, give every test with its family,
m, Holm rank, multiplier, and raw and corrected p-values.

## Summary of findings

- Tabular Q-learning shows strong fixation relative to the uniform-random null (final entropy
  0.51 vs 1.99 bits; ritual strength 0.83 vs 0.35).
- DQN and PPO show smaller but corrected-significant entropy reductions (about 0.3 bits).
- A2C as configured in the main comparison shows none, but that configuration also fails to learn the
  contingent task. With the default rollout length it learns that task, and its noncontingent entropy
  falls to about 1.76 bits (descriptive comparison only).
- Schedule type affects Q-learning at I = 5 and 10 but not at I = 20; PPO schedule and bonus
  differences are within +/-0.5 bits at 24 seeds.
- The DQN versus Q-learning contrast keeps its direction across three alternative DQN settings, but
  its magnitude varies.
- The count-based bonus has no effect that survives correction.

## Citation

If you use this code or data, please cite:

```bibtex
@article{praveen_superstition_like_rl,
  title   = {Superstition-Like Behavioral Fixation in Reinforcement Learning Agents under
             Noncontingent Reward: A Multi-Algorithm, Multi-Schedule Empirical Study in a
             Single-State Environment},
  author  = {Praveen, Gautham and Arora, Arisha and Sikder, Sayan},
  year    = {2026},
  note    = {Preprint / in submission}
}
```
## License

MIT
