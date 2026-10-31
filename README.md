<div align="center">

<h1><img src="https://raw.githubusercontent.com/moTcream/moTcream.github.io/main/AerialS2T/figures/web/logo-176.webp" width="52" height="52" align="absmiddle" alt="AerialS2T logo"> AerialS2T</h1>

### Motion–View Coupling in Vision-Language-Action Models<br>for Aerial Search-to-Track

Jiaxu Tian<sup>&#42;</sup> · Ruibiao Zhu<sup>&#42;</sup> · Chaowei Wang<sup>&#42;</sup><br>Tingyu Zhang · Jingjun Sun · Hongguang Zhang · Shan Gao<sup>†</sup>

<sup>&#42;</sup> Equal contribution &nbsp; <sup>†</sup> Corresponding author

[![arXiv coming soon](https://img.shields.io/badge/arXiv-coming_soon-B31B1B?style=flat&logo=arxiv&logoColor=white)](#to-do-list) [![Dataset](https://img.shields.io/badge/Dataset-AerialLIFT--10K-E5A323?style=flat&logo=huggingface&logoColor=white)](https://huggingface.co/datasets/moTcream/AerialLIFT-10K) [![Model](https://img.shields.io/badge/Model-MoViC-238B87?style=flat&logo=huggingface&logoColor=white)](https://huggingface.co/moTcream/MoViC) [![Code coming soon](https://img.shields.io/badge/Code-coming_soon-596775?style=flat&logo=github&logoColor=white)](#to-do-list)

[Overview](#overview) · [To-do list](#to-do-list) · [Dataset](#aeriallift-10k) · [Method](#movic) · [Results](#results)

</div>

<p align="center">
  <a href="https://motcream.github.io/AerialS2T/">
    <img src="https://raw.githubusercontent.com/moTcream/moTcream.github.io/main/AerialS2T/figures/web/teaser-1600.webp" width="100%" alt="AerialS2T task: take off, search for an initially unseen moving target, acquire it, and sustain tracking under a language instruction.">
  </a>
</p>

## Overview

**AerialS2T** studies language-guided search and tracking of moving targets. A UAV starts without the target in view, receives an instruction describing its appearance and initial location, and must find it before continuing to follow it. Both flight motion and camera pitch are controlled by the policy.

We provide **AerialLIFT-10K**, a dataset of complete search-to-track demonstrations and a closed-loop benchmark, and **MoViC**, a VLA framework that uses target motion forecasts and future camera-view geometry to refine flight and camera actions.

## To-do list

- [x] Publish the project page and synchronized video–trajectory visualizations.
- [ ] Publish the paper on arXiv.
- [ ] Add dataset format documentation and data preparation scripts.
- [ ] Add MoViC checkpoint loading and inference examples.
- [ ] Release training code and configurations.
- [ ] Release closed-loop evaluation scripts and benchmark setup instructions.

Training and evaluation code will be added to this repository. Dataset and model resource links are available below.

## AerialLIFT-10K

**Aerial Language-Instructed Flight Trajectories** contains expert demonstrations collected in a CARLA-based simulation environment. Each demonstration includes synchronized observations, UAV poses, and joint flight and camera action labels.

| Demonstrations | Flight hours | Action-labeled steps | Maps | Weather settings | Target models |
| :---: | :---: | :---: | :---: | :---: | :---: |
| 10,127 | 106.3 | 1.90M | 6 | 12 | 93 |

<p align="center">
  <img src="https://raw.githubusercontent.com/moTcream/moTcream.github.io/main/AerialS2T/figures/web/dataset-1600.webp" width="100%" alt="AerialLIFT-10K dataset overview, target diversity, action statistics, and target acquisition times.">
</p>

→ [AerialLIFT-10K on Hugging Face](https://huggingface.co/datasets/moTcream/AerialLIFT-10K)

## MoViC

**Motion–View Coupling** extends a base VLA policy with two components:

- **Recurrent Target Forecasting** retains target information across observations and predicts target position distributions at future waypoint times.
- **Projective Action Decoding** projects those forecasts into the camera views determined by the initial actions. The resulting geometry guides residual updates to flight and camera control.

The policy predicts eight waypoints over a 1.6 s horizon and replans every 0.2 s.

<p align="center">
  <img src="https://raw.githubusercontent.com/moTcream/moTcream.github.io/main/AerialS2T/figures/web/model-1600.webp" width="100%" alt="MoViC architecture with Recurrent Target Forecasting and Projective Action Decoding.">
</p>

→ [MoViC on Hugging Face](https://huggingface.co/moTcream/MoViC)

## Results

Selected results on **unseen maps** (Town05 and Town07). ADE is the open-loop average displacement error in meters; the remaining metrics are closed-loop percentages. Bold indicates the best value in each column.

| Policy | ADE ↓ | E2E-SR ↑ | AR ↑ | TCR ↑ |
| :--- | ---: | ---: | ---: | ---: |
| SmolVLA | 2.738 | 1.40 | 42.20 | 22.36 |
| π<sub>0</sub> | 2.832 | 2.00 | **45.60** | 17.43 |
| π<sub>0.5</sub> | 2.020 | 10.20 | 31.60 | 26.02 |
| MoViC | **1.602** | **16.60** | 41.40 | **31.66** |

E2E-SR: end-to-end success rate · AR: acquisition rate · TCR: tracking continuity ratio.

See the [full open-loop and closed-loop results](https://motcream.github.io/AerialS2T/#results) for both seen and unseen maps.
