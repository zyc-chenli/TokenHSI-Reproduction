# TokenHSI · Reproduction and Study

**Physics-based human–scene interaction · skill learning · transformer policies**

An unofficial reproduction fork maintained by **Yucheng Zhao**, with a concise setup guide and a map from experiments to the original implementation.

[Original repository](https://github.com/liangpan99/TokenHSI) · [Paper](https://arxiv.org/abs/2503.19901) · [Original project and demos](https://liangpan99.github.io/TokenHSI/) · [My selected work](https://zyc-chenli.github.io/)

## Overview

TokenHSI studies how one transformer policy can represent several physics-based interaction skills and adapt them to new combinations, objects, terrain and longer task sequences.

This fork is organised for studying and reproducing that workflow. The **research method, training code and original results are by Liang Pan and co-authors**. The additions here are the reproduction guide and experiment map; upstream algorithm files are unchanged.

## Start here

1. Read the [reproduction guide](docs/REPRODUCTION.md) for dependencies, external assets and a minimal first run.
2. Use the experiment table below to locate the corresponding task and script.
3. Refer to the preserved [upstream README](README.upstream.md) for the full original instructions and downloads.

## Experiment map

| Study | What to inspect | Entry point |
| --- | --- | --- |
| Single-skill policies | Path following, sitting, climbing and carrying | [`tokenhsi/scripts/single_task/`](tokenhsi/scripts/single_task/) |
| Unified foundational skills | Multi-task transformer policy | [`stage1_test.sh`](tokenhsi/scripts/tokenhsi/stage1_test.sh) |
| Task-specific evaluation | Evaluate a selected foundational skill | [`stage1_eval.sh`](tokenhsi/scripts/tokenhsi/stage1_eval.sh) |
| Skill composition | Combine locomotion, climbing or sitting with carrying | [`tokenhsi/env/tasks/comp_interaction_skills/`](tokenhsi/env/tasks/comp_interaction_skills/) |
| Object and terrain adaptation | Apply learned skills to changed environments | [`tokenhsi/env/tasks/adapt_interaction_skills/`](tokenhsi/env/tasks/adapt_interaction_skills/) |
| Long-horizon interaction | Sequence several skills in one scene | [`tokenhsi/env/tasks/longterm_task_completion/`](tokenhsi/env/tasks/longterm_task_completion/) |

## Quick run

On the configured Linux/CUDA environment, from the repository root:

```bash
# Play a single-skill path-following policy.
bash tokenhsi/scripts/single_task/traj_test.sh

# Play the unified foundational-skill policy.
bash tokenhsi/scripts/tokenhsi/stage1_test.sh

# Evaluate one skill with the original evaluation script.
bash tokenhsi/scripts/tokenhsi/stage1_eval.sh traj
```

These commands require the separately downloaded body models, motion data and checkpoints. See the [guide](docs/REPRODUCTION.md) before running them.

## Scope and provenance

- This is a **reproduction/study fork**, not the official paper repository.
- The current code uses the upstream simulated character; a physical G1/H1 port is not included.
- Body models, external datasets and trained checkpoints must be obtained from their respective providers.
- No new benchmark score or robot-deployment result is claimed by this documentation update. The simulator was not run as part of this repository refresh.
- Original copyright notices are retained. The upstream repository has no repository-wide license file at the pinned revision; this fork does not add a license over upstream code or external assets.

**Upstream revision:** [`baa01f3`](https://github.com/liangpan99/TokenHSI/commit/baa01f360ce112a7f2773b7576a25b223d0b0837).

## Citation

Please cite the original work when using TokenHSI:

```bibtex
@inproceedings{pan2025tokenhsi,
  title={TokenHSI: Unified Synthesis of Physical Human-Scene Interactions through Task Tokenization},
  author={Pan, Liang and Yang, Zeshi and Dou, Zhiyang and Wang, Wenjia and Huang, Buzhen and Dai, Bo and Komura, Taku and Wang, Jingbo},
  booktitle={Proceedings of the IEEE/CVF Conference on Computer Vision and Pattern Recognition},
  year={2025}
}
```
