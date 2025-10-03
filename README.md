<p align="center">
  <!-- Hero image (replace with your actual path/filename) -->
<p align="center">
  <!-- 3-panel gallery of sample frames -->
  <img src="images/imagen_real_1.jpg" width="30%">
  <img src="images/pointcloud_filtrada_1.png" width="30%">
  <img src="images/pointcloud_original_1.png" width="30%">
</p>

<h1 align="center">UCHILE-Dust: A LiDAR Dust Filtering Dataset for Mining Applications</h1>



<p align="center">
  <em>Example point clouds from the Indoor, Outdoor, and Carén (mobile) subsets. Colors are illustrative (e.g., by intensity/return).</em>
</p>

## Overview
**UCHILE-Dust** is a curated dataset of **LiDAR point clouds under dusty conditions** for research on **dust filtering / de-noising** in mining and outdoor environments. Captures include **indoor**, **outdoor**, and **robot-mounted** scenarios with **odometry** when available. Data were used in:

> **Dust filtering in LiDAR point clouds using deep learning for mining applications**  
> *Bruno Cavieres, Nicolás Cruz, Javier Ruiz-del-Solar* (2025)

**Modality:** Ouster OS0 multi-echo LiDAR  

---

## Downloads

- 🗂️ **Processed frames (per-frame point clouds):** [https://drive.google.com/file/d/1sSr3hgCl_UEcZh9t2eX-1d_QqDimnwFS/view?usp=sharing]

---

## Subsets & Statistics
All captures use an **OS0** sensor. Dust is actively introduced in each recording.

| Subset      | Recordings | Point Clouds | Total Points  | % Dust | Format | Train/Val/Test |
|-------------|------------|--------------|---------------|--------|--------|----------------|
| Interior 1  | 10         | 1,874        | 72,741,326    | 4.1%   | PCAP   | 82 / 09 / 09   |
| Interior 2  | 12         | 1,740        | 71,225,483    | 11.2%  | PCAP   | 70 / 15 / 15   |
| Exterior 1  | 10         | 1,529        | 45,311,889    | 3.2%   | PCAP   | 84 / 08 / 08   |
| Exterior 2  | 13         | 1,885        | 75,820,483    | 3.6%   | PCAP   | 66 / 17 / 17   |
| Carén (mob) | 13         | 7,089        | 234,929,532   | 8.1%   | ROSbag | 46 / 26 / 28   |

<p align="center">
  <!-- Data card / infographic -->
  <img src="assets/uchile-dust-card.png"
       alt="UCHILE-Dust dataset card summarizing subsets, points, and formats"
       width="85%">
</p>

**Locations & Conditions**
- **Interior 1/2:** AMTC Field Robotics Lab (static sensor), reflective/transparent surfaces present.
- **Exterior 1/2:** AMTC courtyard (static sensor), open-air plumes / wall proximity.
- **Carén:** open field/quarry (mobile robot, OS0 + odometry), dust via blower; most realistic for navigation.

<a id="data-card"></a>

See [`LICENSE`](LICENSE) for full terms. If you need alternative terms (e.g., commercial use), contact us.

---

## Citation
If you use **UCHILE-Dust**, please cite **both** the dataset and the paper.

**Dataset**
```bibtex
@dataset{uchile_dust_2025,
  title   = {UCHILE-Dust: LiDAR Dust Filtering Dataset},
  author  = {Cavieres, Bruno and Cruz, Nicolás and Ruiz-del-Solar, Javier},
  year    = {2025},
  note    = {Dataset accompanying the paper "Dust filtering in LiDAR point clouds using deep learning for mining applications"}
}
