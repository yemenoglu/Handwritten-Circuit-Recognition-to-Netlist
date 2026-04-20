# [A Hybrid Deep Learning Pipeline for Automated Schematic Reconstruction and Netlist Extraction from Hand-Drawn Circuits]

This repository contains the official implementation of the paper: "[A Hybrid Deep Learning Pipeline for Automated Schematic Reconstruction and Netlist Extraction from Hand-Drawn Circuits]". 
Our pipeline automatically converts hand-drawn or raw circuit diagrams into clean, standardized SVG/PNG schematics using a deep learning-based object detection and Optical Character Recognition (OCR) pipeline.

## 🚀 Quick Start (Google Colab)
The easiest way to test CircuitNet is through Google Colab.
1. Open `notebooks/CircuitNet_Demo.ipynb` in Colab.
2. Run all cells.
3. The script will automatically download the pre-trained weights and process the 5 sample images in the `data/` folder.

## 🧠 Models
- **YOLOv8:** Component & Node detection.
- **CNN:** Component classification (Resistor, Diode, etc.).
- **CRNN:** Recognition of handwritten labels/values (optional).

## 📊 Demo Dataset
The repository includes 5 sample circuits and their corresponding `ground_truth_demo.txt` for evaluation.
