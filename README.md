# ParkVision — Parking Occupancy & Vehicle Type Detection

**Author:** Mohamed Athj
**Course:** ITAI 1378 — Computer Vision and AI
**Tier:** 1 — justified because the core deliverable (occupancy detection) uses a fine-tuned pretrained YOLOv8 model without needing custom data collection, keeping scope realistic for one term while still producing a fully working, measured system.
## Problem

Parking lot managers (airports, campuses, shopping malls) have no real-time, automated view of how many spaces are occupied versus empty, and no way to know what type of vehicle (car, bus, truck) is using each space. This limits their ability to optimize space allocation and apply differentiated pricing (e.g., charging buses differently than personal cars), and forces reliance on manual, error-prone monitoring.

## Solution

ParkVision analyzes parking lot camera images to detect which spaces are occupied or empty, and identifies the vehicle type on occupied spaces. The system combines a fine-tuned occupancy detector with a general vehicle-type detector.

**Flow:** Parking lot image → YOLOv8 (fine-tuned on PKLot) detects space-empty / space-occupied → YOLOv8 (pretrained on COCO) attempts vehicle-type classification on occupied spaces → Occupancy map + vehicle type report

## Technical Approach

- **Technique:** Object detection (two-stage pipeline)
- **Model 1 — Occupancy:** YOLOv8n, fine-tuned on the PKLot dataset (2 classes: space-empty, space-occupied)
- **Model 2 — Vehicle type:** YOLOv8n, pretrained on COCO (80 classes, using car/bus/truck), applied to crops of occupied spaces
- **Framework:** Ultralytics YOLOv8, PyTorch, Google Colab (T4 GPU)
- **Why this approach:** Fine-tuning a small pretrained model on PKLot is fast and cheap (a few minutes on a free GPU) while reaching very high accuracy, since the dataset already provides labeled occupancy annotations. Reusing a pretrained COCO model for vehicle type avoids collecting and labeling new data, keeping the project within Tier 1 scope.
