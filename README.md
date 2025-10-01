# UCHILE-Dust
A dataset for performing dust filtering.


<p align="center">
  <img src="/assets/teaser.png" alt="Dust Filtering Thermal Dataset" width="75%">
</p>

<h1 align="center">Dust Filtering in Thermal Imagery</h1>
<p align="center">
  <b>Thermal dataset + baselines for dust filtering / human pose in low-visibility</b><br>
  <a href="#download">Download</a> •
  <a href="#quickstart">Quickstart</a> •
  <a href="#results">Results</a> •
  <a href="#citation">Cite</a>
</p>

<p align="center">
  <a href="https://github.com/YOUR_USER/YOUR_REPO/stargazers"><img src="https://img.shields.io/github/stars/YOUR_USER/YOUR_REPO" alt="GitHub stars"></a>
  <a href="https://github.com/YOUR_USER/YOUR_REPO/actions"><img src="https://img.shields.io/github/actions/workflow/status/YOUR_USER/YOUR_REPO/ci.yml" alt="Build"></a>
  <a href="/LICENSE"><img src="https://img.shields.io/badge/license-YOUR_LICENSE-blue.svg" alt="License"></a>
  <a href="#dataset-card"><img src="https://img.shields.io/badge/data-card-green" alt="Data Card"></a>
</p>

---

## TL;DR
- **Task:** Dust filtering / robustness for thermal human pose (or detection).  
- **Modality:** LWIR thermal, **bit depth:** 16-bit (provided also as normalized 8-bit).  
- **Scale:** `YOUR_NUM` images / `YOUR_NUM` sequences / `YOUR_NUM` subjects.  
- **Annotations:** COCO-style keypoints (`17` joints) + visibility + person boxes.  
- **License:** `YOUR_LICENSE` (research only / CC BY-NC-SA, etc.).

> Demo  
> <img src="/assets/demo.gif" alt="demo" width="80%">

---

## Dataset Card
- **Name:** Dust Filtering Thermal Dataset (DFTD)  
- **Collection:** YOUR_COLLECTION_NOTES (site, camera model, conditions)  
- **Format:**  
  - Images: `/images/{split}/{seq}/{frame}.tiff` (16-bit) and `/images8/{...}.png` (8-bit)  
  - Labels: COCO JSON: `/annotations/person_keypoints_{split}.json`  
- **Splits:** `train / val / test` with subject or sequence separation  
- **Intended use:** Research on human pose/detection under dust/low-visibility  
- **Limitations:** YOUR_LIMITATIONS (domain, bias, resolution, etc.)  
- **Contact:** YOUR_EMAIL

---

## Download
- 📦 **Full dataset (16-bit TIFF):** [Link](YOUR_LINK)  
- 📦 **8-bit normalized PNGs:** [Link](YOUR_LINK)  
- 📝 **Annotations (COCO JSON):** [Link](YOUR_LINK)  
- 🔎 **Preview subset (100 imgs):** [Link](YOUR_LINK)

> Checksums in `/checksums/sha256.txt`. See **Integrity** section below.

---

## Quickstart
```bash
# 1) Clone
git clone https://github.com/YOUR_USER/YOUR_REPO.git
cd YOUR_REPO

# 2) (Optional) Create env
python -m venv .venv && source .venv/bin/activate   # Windows: .venv\Scripts\activate
pip install -r requirements.txt

# 3) Verify data layout
python tools/verify_layout.py --root /path/to/dataset

# 4) (Optional) Convert 16-bit -> 8-bit previews
python tools/convert_16_to_8.py --src /path/to/images16 --dst ./images8
