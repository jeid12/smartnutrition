# Model Assets Guide

This document explains which model files are needed, how to generate them, and what must be stored alongside each model so backend inference stays stable and reproducible.

## Goals

- Make model handoff clear for all contributors.
- Prevent missing files during deployment.
- Keep MVP delivery fast with a minimal but complete artifact set.

## Folder ownership

- YOLO assets: backend/models/yolo
- Depth assets: backend/models/depth_anything

## Recommended minimal artifact set for MVP

For each production model version, keep these files:

1. Primary weight file
- Example: model.pt
- Purpose: source checkpoint for reproducibility and re-export.

2. Runtime export file
- Example: model.onnx or model.engine
- Purpose: fast inference runtime target.

3. Model manifest
- Example: manifest.json
- Purpose: declares version, expected input size, classes, and checksums.

4. Inference config
- Example: inference_config.json
- Purpose: thresholds and preprocessing settings used in production.

5. Label map (YOLO only)
- Example: classes.json
- Purpose: class index to class name mapping for nutrition logic.

6. Validation report
- Example: validation_report.json
- Purpose: accuracy and latency snapshot for release approval.

7. Calibration notes (Depth or portion pipeline)
- Example: calibration_notes.md
- Purpose: explain assumptions and known limitations.

## What else should live near models

- Release notes: what changed from previous model version.
- Smoke-test image list: small fixed set used to verify outputs.
- License notice: if model or pretrained weights have license constraints.

## Suggested versioned layout

Use versioned subfolders so rollbacks are simple.

- backend/models/yolo/v1/
- backend/models/yolo/v2/
- backend/models/depth_anything/v1/

Each version folder should include its own manifest and config.

## Example manifest fields

Use the same keys for both YOLO and depth models.

- model_name
- model_type (detection or depth)
- version
- created_at
- framework
- input_shape
- class_count (YOLO)
- classes_file (YOLO)
- runtime_format
- primary_weight_file
- runtime_weight_file
- checksum_sha256 for each artifact
- trained_on_dataset_version
- metrics_summary
- owner

## How to generate model files

## A) YOLO detection model

1. Train or fine-tune
- Train on food classes relevant to your users and geography.
- Keep dataset version fixed and documented.

2. Validate
- Record precision, recall, and mAP.
- Record latency on target hardware.

3. Export runtime format
- Export to ONNX first for portability.
- Optionally export to TensorRT engine for faster GPU deployment.

4. Create companion files
- classes.json
- inference_config.json
- manifest.json
- validation_report.json

5. Run smoke tests
- Verify known sample images produce expected classes.
- Confirm low-confidence fallback behavior is triggered when needed.

## B) Depth model

1. Start with pretrained depth model
- For MVP, pretrained is usually enough.

2. Validate on real meal images
- Check whether relative depth order is stable in your lighting conditions.

3. Export runtime format
- Export to ONNX if serving stack supports it.

4. Add portion calibration context
- Store calibration_notes.md describing how depth maps are converted to portion estimates.

5. Create companion files
- inference_config.json
- manifest.json
- validation_report.json

## Suggested naming convention

Use simple predictable names in each version folder.

- model.pt
- model.onnx
- model.engine
- manifest.json
- inference_config.json
- validation_report.json
- classes.json

## Required checks before promoting a model

1. File integrity
- Checksum exists for every artifact.
- Manifest references exact filenames.

2. Functional checks
- Model loads in backend startup path.
- Inference output schema matches service expectations.

3. Quality checks
- YOLO class confusion reviewed for high-impact classes.
- Depth output sanity checked on common meal scenes.

4. Product checks
- Recommendation service still maps detected classes to nutrition entries.
- Manual correction UX handles low confidence correctly.

## Storage and source control policy

- Large binary weights should not be committed directly in normal git history.
- Keep only small metadata files in git when possible.
- Use one of these for heavy binaries:
  - Git LFS
  - Object storage (S3 or equivalent) with manifest reference
  - Model registry

Current backend ignore rules may ignore large model binaries. Confirm your release path before training runs.

## Fast-delivery strategy

For current MVP, use this path:

1. Fine-tune YOLO.
2. Use pretrained depth model without full retraining.
3. Export ONNX for both where possible.
4. Ship with manifest plus minimal validation report.
5. Improve calibration and optimization in later iterations.

## Common mistakes to avoid

- Missing classes.json causing wrong nutrition mapping.
- Exporting runtime weights without preserving source checkpoint.
- No manifest, making rollbacks and audits difficult.
- Mixing dataset versions without documenting changes.
- Shipping without latency validation on target hardware.

## Quick release checklist

- Artifacts present in version folder.
- Manifest complete and checksums correct.
- Backend can load model at startup.
- Smoke tests pass on sample meal images.
- Low-confidence fallback path validated.
