# YOLO Model Folder

Purpose: detect multiple food items from a meal image.

## Inputs

- Single RGB image from upload/camera.

## Outputs

- Bounding boxes per detected item.
- Food class label per box.
- Confidence score per prediction.

## Used by

- `backend/app/services/inference/` for image detection.
- Frontend correction flow (users can adjust bounding boxes).

## Expected artifacts

- Quantized/optimized model files for fast inference.
- Label map (`classes`) aligned with nutrition mapping tables.
- Optional config file defining confidence/NMS thresholds.

## MVP guidance

- Prioritize robust common meal classes over long-tail classes.
- Keep confidence threshold conservative and allow manual correction fallback.
