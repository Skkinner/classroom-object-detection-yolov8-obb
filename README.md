# Classroom Object Detection (YOLOv8 OBB)

Detects **laptops, phones and glasses** left on classroom desks, using **YOLOv8 Oriented Bounding Boxes (OBB)**. Rotated boxes fit objects lying at an angle on a desk much better than normal boxes do.

## Files

| File | What it is |
|---|---|
| `classroom_object_detection_yolov8_obb.ipynb` | Full pipeline: load the dataset, train 3 experiments, evaluate them, and show predictions |
| `Classroom Object Detection.v1i.yolov8-obb.zip` | Dataset exported from Roboflow in YOLOv8 OBB format |

## Dataset

- **43 real photos** taken with phone cameras in our classroom, annotated in Roboflow
- **3 classes:** Glasses, Laptop, Phone
- **146 labelled objects:** Laptop 71, Phone 61, Glasses 14
- **Split:** 30 train / 10 validation / 3 test images (near-identical burst photos kept in the same split to limit leakage)
- **Preprocessing:** auto-orient, resized to 640×640
- **Augmentation:** flips, small rotations, brightness/colour changes, and light blur, applied by YOLO during training

## Experiments

All runs: 640 px images, batch 8, up to 100 epochs, early stopping (patience 30), seed 42.

| Experiment | Model | Optimizer | Purpose |
|---|---|---|---|
| A | yolov8n-obb (nano) | auto (AdamW) | Baseline |
| B | yolov8n-obb (nano) | SGD, lr0 = 0.005 | Effect of optimizer / learning rate |
| C | yolov8s-obb (small) | auto (AdamW) | Effect of a bigger model |

## Results

Validation set (10 images, 27 objects). The best model was chosen by validation mAP@50–95.

| Experiment | Precision | Recall | mAP@50 | mAP@50–95 |
|---|---|---|---|---|
| A: nano, default | 0.767 | 0.547 | 0.656 | 0.464 |
| B: nano, SGD | 0.602 | 0.579 | 0.644 | 0.460 |
| **C: small, default (best)** | **0.787** | **0.696** | **0.691** | **0.475** |

Best model (C) on the validation + test images:

| Class | Found | Missed | False alarms |
|---|---|---|---|
| Laptop | 13 / 14 | 1 | 6 |
| Phone | 9 / 17 | 8 | 11 |
| Glasses | 2 / 4 | 2 | 0 |

**Takeaways:** laptops are detected reliably. Phones are harder because they are small and often appear in groups. Glasses have very few examples (14 in total), so their results are uncertain. The test set has only 3 images, so its scores should not be over-interpreted.

## How to run

1. Open the notebook in **Google Colab**.
2. Go to **Runtime → Change runtime type → GPU**.
3. Click **Runtime → Run all**.
4. When the file picker appears, upload `Classroom Object Detection.v1i.yolov8-obb.zip`.

The notebook installs `ultralytics` itself, trains all 3 experiments, evaluates them, and saves the best model as `best_obb.pt`.

## Limitations

- A small dataset (43 images) from a single classroom, so the model may not generalise to other rooms or lighting.
- Classes are imbalanced: Glasses has only 14 examples.
- Validation and test sets are small (10 and 3 images), so the metrics vary a lot between runs.


## 👥 Team

| Name | GitHub |
|---|---|
| Fahad Al-Nahi | [@Skkinner](https://github.com/Skkinner) |
| Daniyah ALeid | [@DAleid](https://github.com/DAleid) |
| name | [@username](https://github.com/username) |
| name | [@username](https://github.com/username) |
