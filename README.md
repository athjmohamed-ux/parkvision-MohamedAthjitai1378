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

## Data Plan

- **Source:** PKLot dataset (public), via Roboflow — https://public.roboflow.com/object-detection/pklot
- **Size:** 12,416 images total (8,691 train / 2,483 valid / 1,242 test), ~695,900 labeled parking space instances across 3 parking lots (PUCPR, UFPR04, UFPR05), under sunny, cloudy, and rainy conditions
- **Labels:** 2 classes — space-empty, space-occupied (bounding boxes)
- **Vehicle type:** No custom collection needed — reused COCO classes (car, bus, truck) from a pretrained model
- **License:** CC BY 4.0

## Milestone Plan (16-week term)

| Phase | Goal | Status |
|---|---|---|
| Blueprint (Week 10) | Plan approved, occupancy model tested | ✅ Done — YOLOv8n fine-tuned on PKLot, mAP50 = 0.990 on test set |
| First Working Demo (Week 11) | Pretrained model runs end to end on sample images | ✅ Done — occupancy detection + vehicle-type attempt tested |
| Make It Yours (Weeks 12–13) | Add application logic (counting, reporting, occupancy map) | Planned |
| Improve and Measure (Week 14) | Test on more images, refine vehicle-type approach or scope it out | Planned |
| Package and Present (Week 15) | Demo video, final README, final slides | Planned |

## Risks and Plan B

1. **Risk:** General-purpose vehicle detectors (COCO-pretrained) are unreliable on small crops like individual parking spaces — only ~14% of occupied-space crops produced a valid vehicle-type detection in testing, even after upscaling.
   **Plan B:** Present occupancy detection (already at 99% mAP50) as the core, guaranteed deliverable. Present vehicle-type detection as an exploratory/bonus feature, and consider fine-tuning a small classifier specifically on vehicle-type crops if time allows.

2. **Risk:** The occupancy model may not generalize as well to a parking lot outside PKLot's 3 source lots (different camera angle, lighting, or layout).
   **Plan B:** Scope the final demo to PKLot-style camera views (or a similar top-down angle), and clearly state this limitation rather than claiming universal generalization.

**Compute:** Google Colab (free tier, T4 GPU) — sufficient for all training and inference done so far, estimated cost: $0.
