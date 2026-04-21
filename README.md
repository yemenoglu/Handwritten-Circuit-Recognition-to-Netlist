# A Hybrid Deep Learning Pipeline for Automated Schematic Reconstruction and Netlist Extraction from Hand-Drawn Circuits

This repository contains the official implementation of the paper: **"A Hybrid Deep Learning Pipeline for Automated Schematic Reconstruction and Netlist Extraction from Hand-Drawn Circuits"**. 

Our pipeline automatically converts hand-drawn or raw circuit diagrams into clean, standardized SVG/PNG schematics and extracts their corresponding JSON netlists using a hybrid deep learning-based object detection, Optical Character Recognition (OCR), and heuristic node-matching pipeline.

## 📌 Pipeline Architecture

The system operates in a streamlined, two-stage process within a single interactive notebook:

1. **Stage 1: Text Detection, Recognition, and Removal**
   * Detects handwritten component values and labels.
   * Recognizes the text and stores the values.
   * Digitally removes (inpaints) the text from the image, generating clean, text-free circuit diagrams to prevent false component classifications.
2. **Stage 2: Component Classification, Node Detection, and CAD Generation**
   * Processes the text-free images to locate and classify electronic components.
   * Uses morphological operations to detect wiring intersections and structural nodes.
   * Maps pins to nodes and generates a standard JSON Netlist.
   * Reconstructs a clean, perfectly aligned CAD schematic (SVG/PNG) using `svg_schematic`.

## 🚀 Quick Start (Google Colab)

The easiest and recommended way to test the system is through Google Colab, which handles all dependencies and environment setups automatically.

1. Open `notebooks/demo.ipynb` in Google Colab.
2. Run the cells sequentially:
   * **Cell 1 (OCR & Text Removal):** Downloads required YOLO/CRNN weights and processes the sample images to remove text.
   * **Cell 2 (Schematic & Netlist Generation):** Downloads the CNN classifier, processes the cleaned images, extracts the netlist, and plots the final CAD drawing.
3. All outputs (JSON netlists, standard CAD drawings, and visualizations) will be automatically saved in the `output/` directory.

## 🧠 Models & Methodologies

Our hybrid approach leverages the following core models:

* **YOLOv8:** Fast and accurate text region detection.
* **CRNN (Convolutional Recurrent Neural Network):** Recognition of handwritten labels and numerical values.
* **Custom CNN:** High-accuracy component classification (Resistors, Capacitors, Inductors, Diodes, and DC Sources).
* **Heuristic Rule-Based Engine:** A custom spatial clustering and detour-routing algorithm for exact schematic reconstruction and pin-to-node mapping.

## 📊 Demo Dataset

The repository includes a minimal demo dataset to verify the pipeline:
* **`data/` folder:** Contains 5 sample hand-drawn circuits.
* **`ground_truth_demo.txt`:** Contains the ground truth structural node coordinates and graph topology for the evaluation of the pipeline's accuracy metrics.

## 📁 Output Structure

After running the pipeline, the system automatically generates an `output/` directory containing:
* `no_text_images/`: Circuits with text removed.
* `netlists/`: Extracted circuit topologies in JSON format.
* `cad_drawings/`: Reconstructed schematic drawings in standard SVG and PNG formats.
* `visualizations/`: Side-by-side comparisons of the detection results and final CAD outputs.
