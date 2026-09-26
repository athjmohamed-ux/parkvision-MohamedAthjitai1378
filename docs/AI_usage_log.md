# AI Usage Log

## Tool: Claude (Anthropic)

**What the author (Mohamed Athji) did directly:**
- Set up the GitHub repository structure and managed all commits.
- Created and ran the entire Google Colab notebook: downloaded the PKLot dataset, installed YOLOv8, and launched training.
- Diagnosed and fixed a training performance issue (switched Colab runtime from CPU to GPU after noticing training was too slow).
- Ran and verified all model results personally — training metrics, validation on unseen test data, and vehicle-type detection experiments (including testing a fix by upscaling image crops).
- Interpreted the metrics (mAP50 = 0.990, mAP50-95 = 0.854, ~4.8ms inference speed) and used the actual test results to decide that vehicle-type detection should be documented as a risk rather than promised as a guaranteed feature.
- Made all final technical and scope decisions for the project (Tier selection, what to keep as core vs. bonus scope).

**How Claude was used:**
- As a planning and coding assistant: helped structure the project around the course rubric, suggested code snippets to try, and helped explain metrics and error messages along the way.
