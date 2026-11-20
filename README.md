# From Hand-Drawn Circuits to Circuit Graphs: Symbol and Value Recognition with Projection-Based Connectivity Inference

[![Open In Colab](https://colab.research.google.com/assets/colab-badge.svg)](https://colab.research.google.com/github/yemenoglu/Handwritten-Circuit-Recognition/blob/main/notebooks/all_in_one_system.ipynb)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](LICENSE)

This repository contains the implementation that accompanies the paper **"A Hybrid Deep Learning Pipeline for Automated Schematic Reconstruction and Circuit Graph Extraction from Hand-Drawn Circuits"** (manuscript submitted to *Sensors*; the citation will be updated after publication).

The pipeline converts a scanned hand-drawn circuit schematic into

* a **JSON circuit representation** (nodes and the components connecting them, with orientation-aware classes and values), and
* an **SVG schematic** redrawn from that representation.

Connectivity is inferred by **Dynamic Symbol-Hole Pairing (DSHP)** from the positions and orientations of the detected components; wire pixels are not traced.

## Pipeline

The notebook `notebooks/all_in_one_system.ipynb` runs the complete pipeline in Google Colab.

| Stage | Cell | What it does |
|---|---|---|
| 1 | Text detection and recognition | YOLOv8n detects the handwritten labels; orientation-aware cropping (OAC) removes nearby circuit strokes before a CRNN transcribes each label. The labels are then masked in the image. |
| 2 | Components, connectivity and output | Component candidates are located on a 1000 × 750 working image and classified by a CNN into nine orientation-aware classes (resistor, capacitor, inductor and diode, each horizontal or vertical, and DC source). Values are assigned to the nearest component, DSHP infers the nodes, and the JSON circuit representation and SVG schematic are written and evaluated. |
| 3 (optional) | Pixel-skeleton baseline | Replaces only the connectivity step by a medial-axis wire skeleton on the same detections, for the comparison with DSHP reported in the paper. |
| 4 (optional) | Runtime | Measures the time of each processing stage per drawing. |

Run the cells in this order. Stages 3 and 4 require Stages 1 and 2.

## Quick start

1. Click the **Open in Colab** badge above (or open `notebooks/all_in_one_system.ipynb` in Google Colab).
2. Run Stage 1 and Stage 2. The first cell clones this repository and downloads the trained models from the [v1.0.0 release](https://github.com/yemenoglu/Handwritten-Circuit-Recognition/releases/tag/v1.0.0).
3. The results are written to `output/` (see below). Stages 3 and 4 can be run afterwards if needed.

## Evaluation metrics

The notebook reports the metrics defined in the paper:

| Metric | Definition |
|---|---|
| Component score | C / (N_GT + FP), counted per class and macro-averaged over circuits |
| Node matching score | TP / (TP + FN + FP), one-to-one matching within 40 px on the working image |
| Topological accuracy | Predicted and reference graphs are isomorphic (connectivity only) |
| Structural Graph Accuracy (SGA) | Topological accuracy, and every component has the correct orientation-aware class |
| Exact Circuit Reconstruction (ECR) | SGA, and every component has the reference value (compared after removing spaces and ignoring letter case) |
| Character error rate (CER), exact-match label accuracy | OCR metrics computed after Levenshtein pairing of predicted and reference labels |

Terminal polarity is not evaluated.

## Results reported in the paper

On the 117-circuit evaluation collection (seven drawers, scanned pencil drawings):

| Metric | Result |
|---|---|
| Component score | 94.2% |
| Node matching score | 87.9% |
| Topological accuracy | 89.7% (105/117) |
| SGA | 67.5% (79/117) |
| ECR | 50.4% (59/117) |
| OCR: CER / exact-match label accuracy | 14.76% / 67.39% |

On the same detections, the pixel-skeleton baseline recovered the connectivity of 55 circuits, compared with 105 for DSHP. On a CPU-only Colab instance, a drawing was processed in about 1.2 s; DSHP connectivity inference and value association took about 85 ms of this.

The demonstration data in this repository contain five circuits, so the notebook's numbers on them differ from the values above.

## Repository structure

```
notebooks/all_in_one_system.ipynb   complete pipeline (Stages 1–4)
data/samples/                       five demonstration drawings
data/ground_truth_demo.txt          reference node coordinates and circuit graphs for the samples
weights/                            trained models (downloaded from the v1.0.0 release)
LICENSE                             MIT License
CITATION.cff                        citation information
```

## Outputs

| Folder | Content |
|---|---|
| `output/ocr_data/` | Recognized labels and their positions (`ocr_results.json`) |
| `output/no_text_images/` | Drawings with the detected labels masked |
| `output/netlists/` | JSON circuit representations |
| `output/cad_drawings/` | Reconstructed schematics (SVG and PNG) |
| `output/visualizations/` | Detection results and side-by-side comparisons |
| `output/baseline_eval/` | Per-circuit results of the baseline comparison (Stage 3) |
| `output/runtime/` | Runtime per drawing (Stage 4) |

## Dataset

The datasets used in the paper are available at Mendeley Data: https://doi.org/10.17632/vt2j9fwscb.1

## Scope and limitations

The method assumes approximately orthogonal layouts and nine orientation-aware component classes. It was evaluated on scanned pencil drawings; photographs with perspective distortion and uneven lighting were not evaluated. Parallel components between the same two nodes are not preserved, and terminal polarity is not assessed.

## Citation

If you use this code or the data, please cite the paper (details will be updated after publication):

```bibtex
@article{handwritten_circuit_dshp,
  title   = {A Hybrid Deep Learning Pipeline for Automated Schematic Reconstruction and Circuit Graph Extraction from Hand-Drawn Circuits},
  author  = {AUTHOR LIST},
  journal = {Sensors},
  year    = {2026},
  note    = {Manuscript submitted}
}
```

## License

This project is released under the [MIT License](LICENSE).
