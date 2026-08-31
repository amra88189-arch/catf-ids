# CATF-IDS

**Context-Adaptive Threat Fusion — a multi-modal intrusion detection system for industrial IoT.**

**[Live demo →](https://amra88189-arch.github.io/catf-ids/)**

> Undergraduate research project, currently being prepared for publication.
> Source is not public while the paper is under review — see
> [Availability](#availability).

---

## Problem

Industrial IoT deployments generate telemetry in several forms at once: network
flows, device sensor readings, and host-level activity on the machines that
manage them. Most published intrusion detection systems consume one of these.
Systems that consume several generally assume the streams describe the same
events at the same moments, and that assumption is rarely tested.

CATF-IDS is built on [TON_IoT](https://research.unsw.edu.au/projects/toniot-datasets),
which ships network captures, IoT sensor telemetry, and Linux and Windows host
logs from the same testbed, and evaluates detection across all four.

## Results

Five-fold stateful cross-validation, 13,256-record balanced training set, all
adaptive state rebuilt at the start of each fold.

| Metric | Mean | Std dev |
| --- | ---: | ---: |
| F1-score | 0.8694 | 0.0095 |
| Precision | 0.9232 | 0.0101 |
| Recall | 0.8220 | 0.0206 |
| ROC-AUC | 0.9750 | 0.0010 |
| False positive rate | 0.0687 | 0.0108 |

Evaluated over 24,934 fused records drawn from 23 capture files.

Every figure is reported with its fold-to-fold variance, against a fixed
protocol, alongside majority-class and single-feature baselines. Detection
performance is also broken out per attack class rather than reported only in
aggregate — the classes that are hard are visible rather than averaged away.

## Engineering practice

The parts of this project I'd point to in a code review:

- **Reproducible builds by patch, not by hand.** Every change to the pipeline
  is a scripted, asserted transformation with structural post-conditions, so a
  half-applied edit fails at build time instead of silently disabling a
  component. This tooling is open-sourced separately as
  [**anchorpatch**](https://github.com/amra88189-arch/anchorpatch), along with the two
  real silent bugs from this codebase that motivated it.
- **Factorial ablation for attribution.** Component contributions are measured
  by full factorial designs over the relevant flags, not by toggling one thing
  and reading the headline number.
- **Claims traced to run output.** The written results are checked by a script
  that extracts every numeric claim from the document and verifies it against
  captured run logs, so a stale figure fails rather than ships.
- **Findings that went against the design were kept.** At least one carefully
  motivated component turned out to contribute nothing, and one turned out to
  cost detections. Both are reported.

## Stack

Python · scikit-learn · pandas · NumPy · FastAPI · pytest

## Availability

The implementation, architecture, and full experimental results are held back
until the paper is published — this is at my supervisor's direction and is
standard for unpublished work.

Happy to walk through the design, the evaluation protocol, or any part of the
code in a call or interview. Reach me at **amra88189@gmail.com**.

## Status

| | |
| --- | --- |
| Implementation | complete |
| Evaluation | complete |
| Paper | in preparation |
| Source release | after publication |

---

<sub>No license is granted for the contents of this repository. © 2026 Abdo Amra.</sub>
