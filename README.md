# [A Hybrid Deep Learning Pipeline for Automated Schematic Reconstruction and Netlist Extraction from Hand-Drawn Circuits]

This repository contains the official implementation of the paper: "[A Hybrid Deep Learning Pipeline for Automated Schematic Reconstruction and Netlist Extraction from Hand-Drawn Circuits]". 
Our pipeline automatically converts hand-drawn or raw circuit diagrams into clean, standardized SVG/PNG schematics using a deep learning-based object detection and Optical Character Recognition (OCR) pipeline.

## 🚀 Pipeline Overview
Our system operates in two main stages:
1. **OCR & Text Removal (`1_OCR_and_Text_Removal.ipynb`):** Detects and recognizes text (component values/labels) using YOLO + CRNN. It then isolates the circuit by removing the text, outputting a clean `circuit_no_text` image.
2. **Circuit Analysis & SVG Generation (`2_Circuit_to_SVG.ipynb`):** Performs node and component detection on the cleaned image. Generates a Netlist and utilizes our custom routing algorithm (with Detour/Escape Routing) to render a flawless SVG/PNG schematic.

## 📁 Repository Structure
```text
├── weights/                   # YOLO, CRNN and Component Detection model weights
├── data/                      # Sample input circuit images
├── output/                    # Generated SVGs, PNGs, and Netlists
├── 1_OCR_and_Text_Removal.ipynb  # Stage 1: Text extraction and image cleaning
├── 2_Circuit_to_SVG.ipynb        # Stage 2: Netlist generation and CAD drawing
├── svg_schematic.py           # Custom SVG rendering and component classes
├── requirements.txt           # Python dependencies
└── README.md
