# Human Takeover Guided Selective DAgger for MetaWorld Push-v3

This repository contains a MetaWorld `push-v3` project that improves a vision-based manipulation policy with human takeover annotations.

The setting is a wrist-camera policy for robot pushing. Instead of treating human takeover only as extra demonstrations, this project uses takeover moments as a signal that the policy has entered risky states. A takeover predictor is trained from annotated rollouts, and that predictor is then used to trigger selective DAgger relabeling.

## Project Pipeline

1. Train a wrist-camera base policy on `push-v3`.
2. Collect rollout episodes from the policy.
3. Annotate takeover segments on risky or failed episodes.
4. Train a takeover predictor from the annotated rollouts.
5. Use the predictor to trigger selective expert relabeling during DAgger.
6. Finetune the policy with the original data plus triggered expert labels.

## Annotation Demo

The annotation workflow is demonstrated in the video below.



https://github.com/user-attachments/assets/215f252f-42e6-4471-9520-22001c956745



This demo shows how risky or failed rollout episodes are reviewed and how takeover start/end intervals are labeled for predictor training.

## Main Result

The main retained result in this repository is predictor-guided selective DAgger.

| Policy | seed0 | seed1000 | seed3000 | Mean |
|---|---:|---:|---:|---:|
| Base policy | 0.44 | 0.28 | 0.46 | 0.393 |
| Selective DAgger | 0.66 | 0.54 | 0.74 | 0.647 |

This shows that takeover annotations are most useful as a state-selection signal for expert relabeling.

## Repository Structure

```text
artifacts/
  checkpoints/               saved base policy, takeover predictor, and best selective DAgger model
  summaries/                 retained experiment summaries and evaluation results
assets/
  figures/                   plots used in the project
  videos/                    rollout and annotation demo videos
data/
  human_takeover_rollouts/   annotated rollout episodes used for takeover learning
docs/
  project_report.pdf         project report
scripts/
  train_bc_wrist.py          train the base wrist-camera policy
  collect_takeover_rollouts.py
  annotate_takeovers.py      annotate takeover intervals on recorded rollouts
  train_takeover_predictor.py
  train_selective_dagger.py  main selective DAgger pipeline
  train_hil_dagger.py
  common.py
