# Floor Plan to Navigation Graph Extraction

This repository contains the implementation of four different approaches for extracting robot-navigable graphs from human-readable floor plans, as described in our paper "A Comparative Study of Methods for Extracting Navigation Graphs from Floor Plans."

## Overview

Converting floor plans into machine-interpretable navigation graphs is challenging due to diverse architectural styles, inconsistent annotations, and visual clutter. We implement and compare four fundamentally different approaches:

1. **Classical Computer Vision** - Geometry-based extraction using edge detection and Hough transforms
2. **U-Net Semantic Segmentation** - Deep learning approach for room segmentation
3. **LLM-based Translation** - Language model compilation from textual descriptions to formal GREBTL representation
4. **OCR + Geometry** - Hybrid approach combining text recognition with curvature-based door detection

## Repository Structure

```
.
├── classical_cv.ipynb           # Classical CV pipeline (Canny + Hough)
├── ocr+geometry.ipynb           # OCR-guided geometric analysis
├── llm_implementation/          # LLM-based GREBTL translation
└── unet_implementation/         # U-Net semantic segmentation
```

## Methods

### 1. Classical Computer Vision (`classical_cv.ipynb`)

**Approach:** Uses Canny edge detection and Probabilistic Hough Transform to extract wall segments, followed by contour-based room detection and template matching for doors.

**Best For:** Clean synthetic floor plans with continuous walls

**Limitations:** Fragments on realistic drawings with text, furniture, and dimension markers

**Results:** 25% room detection on real building (4/16 rooms), 100% on synthetic plans (6/6 rooms)

---

### 2. U-Net Semantic Segmentation (`unet_implementation/`)

**Approach:** Trains a U-Net architecture on synthetic floor plans to predict room interior masks. Doors are detected as gaps in walls.

**Best For:** Synthetic floor plans with gap-based doors matching training distribution

**Limitations:** Complete domain shift failure on real architectural drawings (thin lines, text overlays, symbolic doors)

**Results:** Near-perfect on synthetic data, fails on real floor plans

---

### 3. LLM-Based Translation (`llm_implementation/`)

**Approach:** Configures GPT-4 as a "compiler" translating structured textual floor plan descriptions into GREBTL (Generic Role-Entity-Based Thought Language) formal representation.

**System Prompt:** Enforces strict syntax rules, role pairs (`room & wall`, `wall & door`), and output templates.

**Example:**
```
Input: "Room K.3.09 has a door on the south wall connecting to corridor K.3.01"
Output: K.3.09 (room & wall) Wall K.3.09 South
        Wall K.3.09 South (wall & door) Door K.3.09-K.3.01
        K.3.09 (connected & access) K.3.01
```

**Best For:** When accurate textual descriptions exist or can be manually authored

**Limitations:** Requires 6-7 hours of manual description per floor, does not address perceptual challenges

**Results:** 100% accuracy (16/16 rooms, 14/14 connections) when given accurate descriptions

---

### 4. OCR + Geometry (`ocr+geometry.ipynb`)

**Approach:** Combines EasyOCR for room label detection with flood-fill region growing and curvature-based geometric door detection.

**Key Innovation - Curvature-Based Door Detection:**
1. Analyzes room boundaries for continuous curved arcs
2. Validates arcs via perpendicular junction detection (90° angle check)
3. Computes direction vectors from arc midpoints
4. Traces vectors to identify connected rooms

**Key Property:** Achieves 100% door detection accuracy on any pair of successfully segmented rooms

**Best For:** Real floor plans with readable labels and standard arc-based door symbols

**Limitations:** Fails when OCR misses labels, or when doors use non-standard representations (rectangles, symbols, gaps)

**Results:** 81% room detection (13/16), 79% door connections (11/14) on real building floor

---

## Quick Start

### Prerequisites
```bash
pip install opencv-python numpy matplotlib easyocr networkx
pip install torch torchvision  # For U-Net
```

### Running Classical CV
```bash
jupyter notebook classical_cv.ipynb
```
Load your floor plan image and adjust Canny/Hough parameters as needed.

### Running OCR + Geometry
```bash
jupyter notebook ocr+geometry.ipynb
```
Best results on floor plans with clear room labels (e.g., "K.3.09", "Office 101").

### Running U-Net
```bash
cd unet_implementation/
# Follow instructions in README
```

### Running LLM Translation
```bash
cd llm_implementation/
# Follow instructions in README
```
Requires OpenAI API key and manually authored floor plan descriptions.

---

## Key Findings

| Method | Real Building (Rooms) | Real Building (Connections) | Best Use Case |
|--------|----------------------|----------------------------|---------------|
| Classical CV | 25% (4/16) | N/A | Clean synthetic plans |
| U-Net | 42% (4/16) | 45% (5/16) | Synthetic plans matching training |
| LLM | 100% (16/16) | 100% (14/14) | Manual descriptions available |
| **OCR + Geometry** | **81% (13/16)** | **79% (11/14)** | **Real labeled plans** |

**Main Insight:** No single method generalizes across all floor plan types. OCR+Geometry is most practical for real-world labeled floor plans with standard architectural conventions.

---

## Dataset

**Real Building:** HW 3. Obergeschoss university floor (16 rooms, 21 doors, 14 unique connections)

**Synthetic Plans:** Randomly generated floor plans with 6 rooms, clean boundaries, gap-based doors

---

## Citation

If you use this code, please cite our paper:

```bibtex
@inproceedings{floorplan2026,
  title={A Comparative Study of Methods for Extracting Navigation Graphs from Floor Plans},
  author={[Illia Rohalskyi, Shehariyar Firdous Shaikh, Lavanya Nagaraj, David Ahmadov]},
  year={2026}
}
```

---

## License

MIT License

---

## Acknowledgments

- EasyOCR for text detection
- OpenCV for classical computer vision operations
- OpenAI GPT-4 for LLM-based translation
- GREBTL formal language framework

---

## Contact

For questions or issues, please open a GitHub issue or contact illia.rohalskyi.dev@gmail.com.
