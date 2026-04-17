# Backend Models

This folder stores model assets used by the backend inference pipeline.

## Subfolders

- `yolo/`: food object detection model(s).
- `depth_anything/`: monocular depth estimation model(s) for portion volume estimation.

## How they are used together

1. `yolo` detects food items and returns bounding boxes + class labels.
2. `depth_anything` estimates relative depth for the same image.
3. The inference service combines detection area + depth cues to estimate portion size.
4. Nutrition and recommendation services convert that into calories/macros and profile-aware nudges.

## What belongs here

- Model weights/checkpoints (when needed locally).
- Exported runtime formats (`.onnx`, `.engine`, `.tflite`, etc.).
- Version metadata files (for reproducibility).

## What should not go here

- Training datasets.
- Personal or sensitive user data.
- Temporary experiment logs (store in dedicated artifact/log locations).

## Notes

- Keep production-serving model versions pinned and documented.
- Prefer small, optimized artifacts for fast CPU/edge inference in MVP.
