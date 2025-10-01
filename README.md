<p align="center">
  <img src="/assets/teaser.png" alt="UCHILE-Dust — Dust Filtering in LiDAR Point Clouds" width="78%">
</p>

<h1 align="center">Dust Filtering in LiDAR Point Clouds using Deep Learning for Mining Applications</h1>

<p align="center">
  <a href="#abstract">Abstract</a> ·
  <a href="#contributions">Contributions</a> ·
  <a href="#uchile-dust-dataset">UCHILE-Dust Dataset</a> ·
  <a href="#method-reduced-pointnet">Method</a> ·
  <a href="#experiments--results">Experiments</a> ·
  <a href="#quickstart">Quickstart</a> ·
  <a href="#citation">Citation</a> ·
  <a href="#license">License</a>
</p>

<p align="center">
  <a href="https://github.com/YOUR_USER/YOUR_REPO/stargazers">
    <img alt="GitHub stars" src="https://img.shields.io/github/stars/YOUR_USER/YOUR_REPO">
  </a>
  <a href="https://github.com/YOUR_USER/YOUR_REPO/actions">
    <img alt="CI" src="https://img.shields.io/github/actions/workflow/status/YOUR_USER/YOUR_REPO/ci.yml?label=CI">
  </a>
  <a href="/LICENSE">
    <img alt="License" src="https://img.shields.io/badge/license-Choose%20a%20license-blue.svg">
  </a>
  <a href="#data-card">
    <img alt="Data Card" src="https://img.shields.io/badge/data-card-green">
  </a>
</p>

---

## Authors
**Bruno Cavieres** (ORCID: 0009-0003-7152-3319) · **Nicolás Cruz** (ORCID: 0000-0003-3620-5887) · **Javier Ruiz-del-Solar** (ORCID: 0000-0003-2965-633X)  
<sub>Department of Electrical Engineering, Universidad de Chile · Advanced Mining Technology Center (AMTC), Universidad de Chile</sub>  
**Corresponding:** jruizd@ing.uchile.cl

---

## Abstract
Depth sensors (LiDAR, 3D cameras) are essential in mining for mapping, characterization, and autonomous navigation. Dust degrades these measurements by scattering/absorbing light, causing false or missed detections. We propose a **neural, point-wise dust filter** that operates **in real time** on LiDAR point clouds. The method is a **reduced PointNet++** that ingests **spatial, intensity, and temporal** cues (and odometry when available). We train and validate on both **real** and **simulated** data, and we introduce **UCHILE-Dust**, a public dataset spanning indoor/outdoor and robot-mounted captures under dusty conditions. The model achieves **SOTA-level performance vs. classical baselines** and generalizes across environments.

---

## Contributions
1. **Real-time point-wise dust filtering** on LiDAR point clouds via a **reduced PointNet++** architecture.  
2. **Temporal features** (difference vectors, magnitude, and variation-aware interpolation) and **odometry alignment** for moving platforms.  
3. **UCHILE-Dust dataset:** multi-environment (indoor/outdoor/mobile) LiDAR recordings with dust, for **training and benchmarking** dust filtering.

---

## UCHILE-Dust Dataset
**Sensor:** Ouster OS0 (multi-echo). **Formats:** PCAP (static), ROS bag (mobile).  
**Pipeline:** frame extraction → odometry alignment (mobile) → multi-echo merge → distance filtering → pre-label from dust-free references → manual annotation (labelCloud) → S3DIS-style export.

<table>
<tr><th>Subset</th><th>Recordings</th><th>Point Clouds</th><th>Total Points</th><th>% Dust</th><th>Format</th><th>Train / Val / Test</th></tr>
<tr><td>Interior 1</td><td>10</td><td>1,874</td><td>72,741,326</td><td>4.1%</td><td>PCAP</td><td>82 / 09 / 09</td></tr>
<tr><td>Interior 2</td><td>12</td><td>1,740</td><td>71,225,483</td><td>11.2%</td><td>PCAP</td><td>70 / 15 / 15</td></tr>
<tr><td>Exterior 1</td><td>10</td><td>1,529</td><td>45,311,889</td><td>3.2%</td><td>PCAP</td><td>84 / 08 / 08</td></tr>
<tr><td>Exterior 2</td><td>13</td><td>1,885</td><td>75,820,483</td><td>3.6%</td><td>PCAP</td><td>66 / 17 / 17</td></tr>
<tr><td>Carén (mobile)</td><td>13</td><td>7,089</td><td>234,929,532</td><td>8.1%</td><td>ROS bag</td><td>46 / 26 / 28</td></tr>
</table>

**Locations & conditions**  
- **Interior 1/2:** AMTC Field Robotics Lab (static sensor), glass & concrete; dust manually dispersed.  
- **Exterior 1/2:** AMTC courtyard (static sensor), open-air dust plumes.  
- **Carén:** large open field/quarry; **Panther robot** with OS0; dust via blower; **odometry available**.

> **Downloads** (fill with your links)
>
> • Full dataset (PCAP/ROS): **[Link]**  
> • Annotations (S3DIS/JSON): **[Link]**  
> • Preview subset (100 frames): **[Link]**  
> • Checksums: **[/checksums/sha256.txt]**

<a id="data-card"></a>

### Data Card (Summary)
- **Intended use:** research on dust filtering/denoising for LiDAR in mining/autonomy.  
- **Modalities:** 3D points (+ intensity, time, optional odometry).  
- **Known limitations:** class imbalance (dust ≤ 12% in subsets), reflective/transparent surfaces (glass) can be challenging.  
- **Contact:** YOUR_EMAIL

---

## Method (reduced PointNet++)
We build on PointNet++ but **shrink** sampling and MLP widths to **halve inference time** while maintaining accuracy.

**Inputs per point:** position $(x,y,z)$, intensity $g$, and temporal cues (one of: diff magnitude $m$, diff vector $\Delta \mathbf{c}$, or temporal variation-aware features $h$). When sensor moves, **odometry** aligns $t-1 \to t$ before temporal features.

### Feature Variants
- **SI:** $[x,y,z,g]$  
- **STdm:** $[x,y,z,m]$  
- **STdv:** $[x,y,z,\Delta \mathbf{c}]$  
- **STi:** $[x,y,z,h]$  
- **SITdm:** $[x,y,z,g,m]$  
- **SITdv:** $[x,y,z,g,\Delta \mathbf{c}]$  
- **SITi:** $[x,y,z,g,h]$

### Architecture deltas (vs. PointNet++)
| Block | PointNet++ | **reduced-PointNet++** |
|---|---|---|
| SA1 | 1024 pts, r=0.1, 32 nbr, MLP [32,32,64] | **512 pts**, r=0.1, 32 nbr, **MLP [16,16,32]** |
| SA2 | 256 pts, r=0.2, 32 nbr, MLP [64,64,128] | **128 pts**, r=0.2, 32 nbr, **MLP [32,32,64]** |
| SA3/SA4 | present | **removed** |
| FP2 | in:320 → MLP [256,128] | **in:96 → MLP [64,32]** |
| FP1 | in:128 → MLP [128,128,128] | **in:(32+feats) → MLP [32,32]** |
| Class head | 128→C (BN 128, Dropout 0.7) | **32→C** (BN 32, Dropout 0.7) |

➡️ **~50% lower latency** on a TITAN 12 GB GPU without performance loss (see *Performance*).

---

## Experiments & Results

### Setup
- **Optimizers/Loss:** CrossEntropy with **class weights** (inverse frequency).  
- **LR grid:** {0.01, 0.008, 0.005}; **batch 16**; **epochs 100** with **Early Stopping (patience 10)** on validation **accuracy**.  
- **Augmentations:** rotations, scaling, occlusion, noise.  
- **Baseline:** **LIDROR** (grid over μ∈{90,100,110}, R<sub>min</sub>∈{0.001,0.01}, φ∈{0.01,0.05,0.1}, N∈{4,6,8}; α=0.006134).  
- **Metrics:** Accuracy, Precision/Recall/F1 (dust class).

### Static sensors (Interior/Exterior)
- All neural variants **outperform LIDROR**; hardest sets are **Interior 1** and **Exterior 2** (low precision across methods due to imbalance).  
- Combining **temporal + intensity** typically > using either alone.

**Example (Interior 2, best variants)**  
SITdm: **Acc 0.94, Prec 0.68, Rec 0.91, F1 0.78**  
SITdv: **Acc 0.95, Prec 0.66, Rec 0.93, F1 0.77**

**Example (Exterior 1, best variants)**  
SITdm: **Acc 0.99, Prec 0.68, Rec 0.99, F1 0.81**

### Moving sensor (Carén)
- All neural variants **>> LIDROR** (baseline precision is very low).
- **Odometry alignment** generally improves results.

**Best (with odometry): SITdv → Prec 0.70, Rec 0.97, F1 0.82, Acc 0.98.**

### Cross-environment generalization
- Train/val on Interior/Exterior; **test on Carén**: performance close to in-domain Carén training → model learns **dust-specific**, not layout-specific, cues.  
- Under this setup, **odometry gains diminish** (likely because train/val didn’t use odometry correction).

### Performance
| Architecture | Avg Inference (s) |
|---|---|
| PointNet++ | 0.1409 |
| **reduced-PointNet++** | **0.0675** |

**~2.1× faster** on average, with **similar accuracy** to the original network.

---

## Quickstart

```bash
# 1) Clone
git clone https://github.com/YOUR_USER/YOUR_REPO.git
cd YOUR_REPO

# 2) (Optional) env
python -m venv .venv
# Windows: .venv\Scripts\activate
source .venv/bin/activate
pip install -r requirements.txt

# 3) Prepare data (S3DIS-style structure recommended)
# datasets/
#   images/  (frames/pcd or npy)
#   annotations/ (labels per frame)
#   splits/ (train/val/test lists)

# 4) Train (choose a variant via --features)
python train.py \
  --data_root ./datasets \
  --split_dir ./datasets/splits \
  --features SITdv \
  --use_odometry true \
  --lr 0.008 --batch_size 16

# 5) Evaluate
python eval.py \
  --data_root ./datasets \
  --split test \
  --features SITdv \
  --use_odometry true
