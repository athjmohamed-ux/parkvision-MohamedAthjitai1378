# ParkVision — Parking Occupancy & Vehicle Type Detection

**Author:** Mohamed Athji
**Course:** ITAI 1378 — Computer Vision and AI
**Tier:** 1 — justified because the core deliverable (occupancy detection) uses a fine-tuned pretrained YOLOv8 model without needing custom data collection, keeping scope realistic for one term while still producing a fully working, measured system.
## Problem

Parking lot managers (airports, campuses, shopping malls) have no real-time, automated view of how many spaces are occupied versus empty, and no way to know what type of vehicle (car, bus, truck) is using each space. This limits their ability to optimize space allocation and apply differentiated pricing (e.g., charging buses differently than personal cars), and forces reliance on manual, error-prone monitoring.

## Solution

ParkVision analyzes parking lot camera images to detect which spaces are occupied or empty, and identifies the vehicle type on occupied spaces. The system combines a fine-tuned occupancy detector with a general vehicle-type detector.

**Flow:** Parking lot image → YOLOv8 (fine-tuned on PKLot) detects space-empty / space-occupied → YOLOv8 (pretrained on COCO) attempts vehicle-type classification on occupied spaces → Occupancy map + vehicle type report
