# Reproduction Guide

This guide organises the existing TokenHSI entry points. Use the original [upstream instructions](../README.upstream.md) for download links and the complete training sequence.

## 1. Environment

The upstream setup specifies:

| Component | Version / requirement |
| --- | --- |
| Python | 3.8 |
| PyTorch | 2.0.0 |
| Torchvision / Torchaudio | 0.15.0 / 2.0.0 |
| CUDA runtime | 11.8 |
| Simulator | NVIDIA Isaac Gym Preview 4 |
| Optional rendering dependency | PyTorch3D 0.7.7 for the long-horizon demo |

Use a compatible Linux machine with an NVIDIA GPU. These instructions target the original Isaac Gym codebase, not Isaac Lab.

```bash
conda create -n tokenhsi python=3.8
conda activate tokenhsi
conda install pytorch==2.0.0 torchvision==0.15.0 torchaudio==2.0.0 pytorch-cuda=11.8 -c pytorch -c nvidia
pip install -r requirements.txt
```

Install Isaac Gym from NVIDIA's official distribution and follow its installation instructions. Do not substitute the simulator without adapting the task code.

## 2. External assets

Download assets from the providers linked in the [original README](../README.upstream.md). They are not bundled by this fork.

| Asset | Expected location / configuration |
| --- | --- |
| SMPL body models | `body_models/smpl/SMPL_FEMALE.pkl`, `SMPL_MALE.pkl`, `SMPL_NEUTRAL.pkl` |
| Preprocessed motions and objects | Follow the original dataset layout and YAML files in `tokenhsi/data/` |
| Dataset source paths | Edit `tokenhsi/data/dataset_cfg.yaml` only if preprocessing source data |
| Single-skill checkpoints | `output/single_task/ckpt_<task>.pth` |
| Unified stage-one checkpoint | `output/tokenhsi/ckpt_stage1.pth` |

The `output/` folder contains local runtime files and is ignored by Git. Review the particular script before running it: stage-two experiments require their corresponding checkpoints and configuration.

## 3. Minimal first run

Start with the single-skill path-following script from the repository root:

```bash
bash tokenhsi/scripts/single_task/traj_test.sh
```

The upstream script launches `HumanoidTraj`, loads the path-following checkpoint and creates 16 simulation environments. First confirm that the model, motion data and checkpoint paths resolve. Then move to the unified policy:

```bash
bash tokenhsi/scripts/tokenhsi/stage1_test.sh
```

## 4. Evaluation and adaptation

For the original stage-one evaluation, specify one of `traj`, `sit`, `climb` or `carry`:

```bash
bash tokenhsi/scripts/tokenhsi/stage1_eval.sh traj
```

This script uses 512 headless environments and seed 0. It is more demanding than the small visualisation run. The stage-two scripts in `tokenhsi/scripts/tokenhsi/` cover composition, object variation, terrain variation and long-horizon completion.

For any new run, record the upstream revision, checkpoint, task, seed, environment count and simulator versions alongside the result. Compare matching evaluation settings; upstream paper numbers are not results of this fork.

## 5. Code reading order

1. `tokenhsi/run.py`: task and training entry point.
2. `tokenhsi/env/tasks/basic_interaction_skills/`: individual interaction tasks.
3. `tokenhsi/env/tasks/multi_task/`: unified foundational-skill task.
4. `tokenhsi/learning/transformer/`: transformer policies, training and skill composition.
5. `tokenhsi/env/tasks/adapt_interaction_skills/` and `comp_interaction_skills/`: task adaptation and composition.

## Validation status

The documented paths and commands were checked against the pinned source revision. Simulator installation, GPU inference and training were not performed during this documentation refresh. This fork does not include a validated physical robot port or a new experimental benchmark.
