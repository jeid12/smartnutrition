# Depth Anything Model Folder

Purpose: estimate depth from a single image to improve portion-size estimation.

## Inputs

- Same meal image used by YOLO detection.

## Outputs

- Relative depth map (per-pixel depth estimate).

## Used by

- `backend/app/services/inference/` to estimate food volume from detection regions.
- Portion-adjustment logic when direct ingredient weights are unavailable.

## Expected artifacts

- Depth model weights in runtime format.
- Optional preprocessing/postprocessing config.
- Version metadata for reproducibility.

## MVP guidance

- Use depth as an estimation aid, not absolute truth.
- Blend model output with user corrections (for example, rice percentage slider for bowl meals).
- Fall back to manual portion input when confidence is low.
