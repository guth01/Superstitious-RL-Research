# Behavioural concentration under noncontingent reward

> **A controlled study of whether reward-independent timing is associated with concentrated action policies in reinforcement-learning agents.**

| Study scope | Value |
| --- | --- |
| Research paper | [Overleaf Manuscript](https://www.overleaf.com/project/6ae05bfe65084e8de28bd992/share#18d266dd69c6eba67bee7ac9143085d186bf58c0a96ff75c) |
| Agents | Q-learning, DQN, PPO, A2C |
| Environment | One state, four actions |
| Reward schedules | Fixed, variable, random, contingent, zero, constant |
| Completed archive | 352 unique training runs |
| Core checks | Learner controls, shifted timestamps, per-seed reporting |

This repository contains the code, analyses, figures, and supporting materials for a study of behavioural concentration in reinforcement-learning (RL) agents exposed to rewards whose timing does not depend on their actions.

📄 **Research Paper:** The full research manuscript is available on [Overleaf](https://www.overleaf.com/project/6ae05bfe65084e8de28bd992/share#18d266dd69c6eba67bee7ac9143085d186bf58c0a96ff75c).

## At a glance

- **Question:** Does noncontingent reward coincide with action concentration beyond what learner dynamics already produce?
- **Approach:** Compare four RL algorithms across reward schedules and controls, then test observed reward-event concentration against circularly shifted reward timestamps.
- **Bottom line:** Q-learning shows small positive excess ritual strength in this setting; learner-level controls show that concentration alone is not evidence of a reward-specific effect.

## Contents

- [Research paper](#research-paper)
- [Research question](#research-question)
- [Experimental setting](#experimental-setting)
- [Main findings](#main-findings)
- [Measures and analysis](#measures-and-analysis)
- [Repository contents](#repository-contents)
- [Running the study](#running-the-study)
- [Reproducibility and data availability](#reproducibility-and-data-availability)
- [Limitations](#limitations)

The project asks a narrow empirical question: when an agent receives rewards that it cannot influence, does its policy become concentrated on a small number of actions, and is any of that concentration specifically associated with the timing of reward events? The study does not claim that an agent has a causal belief, and it does not identify a learning mechanism. It provides a controlled, single-state comparison of observed behaviour across several standard RL learners.

## Research paper

The full manuscript is available to read on Overleaf:
- **Manuscript URL:** [https://www.overleaf.com/project/6ae05bfe65084e8de28bd992/share#18d266dd69c6eba67bee7ac9143085d186bf58c0a96ff75c](https://www.overleaf.com/project/6ae05bfe65084e8de28bd992/share#18d266dd69c6eba67bee7ac9143085d186bf58c0a96ff75c)

## Research question

Skinner's work on response-independent reward schedules described repeated, apparently ritual-like actions in pigeons. Similar language is sometimes used for agents that repeat an action after rewards arrive independently of their choices. Repetition by itself, however, does not show that reward timing selected the action. A learner may become concentrated because of its exploration rule, tie-breaking behaviour, initialization, optimisation dynamics, or other implementation details.

This study therefore separates three related quantities:

1. **Action concentration**, measured using action entropy.
2. **Raw ritual strength**, the frequency of the modal action at recent reward events.
3. **Excess ritual strength**, raw ritual strength minus its expected value after circularly shifting reward timestamps within the same final action trace.

The shifted-timestamp comparison is important because a concentrated policy can produce high raw ritual strength even when reward timing has no special alignment with the action sequence.

## Experimental setting

Agents act in an environment with:

- one constant observed state;
- four discrete actions;
- 15,000 training steps per main run;
- a fixed noncontingent schedule, a variable-interval schedule, and an independent random schedule;
- a contingent comparison condition in which action 0 receives reward;
- zero-reward and constant-reward learner-level controls; and
- an optional count-based exploration bonus.

The main algorithms are tabular Q-learning, Deep Q-Network (DQN), Proximal Policy Optimization (PPO), and Advantage Actor-Critic (A2C). The primary A2C configuration uses `n_steps=5`; an `n_steps=200` configuration is retained as an under-updated control.

Most main conditions use eight independent random seeds. PPO schedule/bonus/null analyses use 24 seeds, and the expanded A2C null analysis uses 16 seeds. In total, the completed archive contains 352 unique training runs.

## Main findings

These findings apply to this abstract single-state environment and the tested configurations.

| Finding | Evidence | Interpretation |
| --- | --- | --- |
| Q-learning concentration | Fixed noncontingent entropy: 0.51 bits; zero-reward entropy: 0.26 bits | Low entropy can arise without reward. |
| Q-learning temporal excess | Mean excess: 0.030; 95% CI: 0.016--0.044; corrected p = 0.0313 | A small positive association under the shifted-timestamp baseline. |
| DQN, PPO, and A2C excess | No corrected-significant positive excess in the expanded fixed-schedule analysis | Raw concentration is not equivalent to reward-timing association. |
| PPO seed variation | 9 of 24 seeds exceed the uniform-policy raw-strength threshold | A subset of PPO runs is concentrated; its mean excess remains close to zero. |
| Q-learning schedule sensitivity | Fixed schedules have lower entropy at intervals 5 and 10, not 20 | Timing and reward frequency remain partly confounded. |
| Count-based bonus | No corrected effect on entropy or raw strength | No clear bonus effect in the tested conditions. |

The results are best interpreted as a bounded demonstration that RL policies can become behaviourally concentrated under zero-information reward. They do not establish a general account of superstition, a reward-locking mechanism, or a credit-assignment failure mode.

## Measures and analysis

### Final entropy

Entropy is computed over the most recent 200 actions and logged every 300 training steps. A run's final entropy is the mean of its final five logged values. Lower values indicate a more concentrated action distribution; the uniform four-action maximum is 2 bits.

### Ritual strength and shifted timestamps

Raw ritual strength is the fraction of the last 30 extrinsic reward events at which the run's modal action was taken. It is calculated in the final 3,000 training steps.

For each run, the final action sequence is held fixed while the binary reward-event mask is circularly shifted 1,000 times. The mean shifted statistic is subtracted from the observed statistic to give excess ritual strength. Confidence intervals are calculated by resampling training seeds, not by treating timestamp shifts as independent training replications.

### Statistical reporting

The analysis reports per-seed values, group means and descriptive SDs, bootstrap confidence intervals, Mann--Whitney or Kruskal--Wallis tests where appropriate, Wilcoxon tests for one-sample null comparisons, and Holm--Bonferroni correction within stated test families. The full traceability table records all 68 tests and their correction multipliers.

> **Interpretation note:** A high raw ritual-strength value can follow from a concentrated policy alone. Excess ritual strength is therefore the more specific descriptive measure of reward-event alignment used in this project.

## Repository contents

| Path | Purpose |
| --- | --- |
| `superstitious_rl_extended_study_v4.ipynb` | Main experiment, analysis, tables, and figures |
| `output/journal_revision/` | LaTeX manuscript, title page, bibliography, and figures |
| `output/journal_revision/figs/` | Manuscript-ready PDF and PNG figures |
| `results/` | Recommended location for exported per-seed data and tables |
| `README.md` | This project overview |

For archival release, include the exported result files alongside the notebook:

- `per_seed_summary.csv`
- `unique_training_runs.csv`
- `run_metadata.json`
- result tables and test-traceability table
- generated PDF/PNG figures
- full action/reward traces or the documented archive containing them

## Running the study

The notebook is designed to run in a Python 3 environment. The completed run used Python 3.13.16 with Stable-Baselines3 2.9.0, Gymnasium 1.4.0, NumPy 2.1.3, SciPy 1.16.3, pandas 2.2.3, and PyTorch 2.11.0+cpu.

1. Install the notebook dependencies.
2. Open `superstitious_rl_extended_study_v4.ipynb` in Jupyter or Google Colab.
3. Run the configuration and helper cells.
4. To reproduce all results, run the experiment cells and then the export cells.
5. If the exported results are already available, use the analysis and figure cells without rerunning training.

Training time depends on hardware and the selected tiers. Reproducing an existing figure from saved per-seed results is substantially faster than repeating the training runs.

## Reproducibility and data availability

The planned archival release should contain the notebook, source code, environment/dependency information, raw per-seed outputs, complete result tables, figures, and documentation needed to regenerate the reported analyses. A versioned Zenodo record should be used for the frozen submission archive so that the DOI identifies the exact materials underlying the manuscript.

## Limitations

This is not a test of agent beliefs or causal attribution. The environment has one constant observation and four actions, so it does not test spatial tasks, rich state representations, observable timing cues, or multi-step behavioural sequences. The reported controls reveal some learner-level sources of concentration but do not exhaust every implementation effect. The timing schedules also vary reward frequency in parts of the interval analysis. Results should therefore be read as configuration-specific empirical observations, not as a general ranking of RL algorithms.

## Citation

The research paper is available on Overleaf:
- **Manuscript:** [Overleaf Project](https://www.overleaf.com/project/6ae05bfe65084e8de28bd992/share#18d266dd69c6eba67bee7ac9143085d186bf58c0a96ff75c)

```bibtex
@article{superstitious_rl_2026,
  title   = {Superstition-Like Behavioral Fixation in Reinforcement Learning Agents under Noncontingent Reward: A Multi-Algorithm, Multi-Schedule Empirical Study in a Single-State Environment},
  author  = {Praveen, Gautham and Arora, Arisha and Sikder, Sayan},
  year    = {2026},
  note    = {Preprint / in submission. Overleaf: \url{https://www.overleaf.com/project/6ae05bfe65084e8de28bd992/share#18d266dd69c6eba67bee7ac9143085d186bf58c0a96ff75c}}
}
```

Add the Zenodo version DOI here once the archive has been published. Cite the version-specific DOI associated with the submitted materials.

## License

Add the licence selected by the authors before public release. A code licence and a data licence may be specified separately where appropriate.
