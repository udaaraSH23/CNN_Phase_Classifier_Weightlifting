# CNN Phase Classifier for Weightlifting

This repository contains a complete pipeline for **automatic Olympic weightlifting phase classification** (Snatch & Clean-and-Jerk). It extracts frames from videos, preprocesses them into skeletons, labels them with lifting phases, and trains a CNN classifier.

---

## Features

- Frame extraction from raw videos (`.mp4`, `.mov`)
- Black margin removal
- Skeleton extraction with **MediaPipe**
- Phase mapping using manual `phase.json`
- Automatic dataset CSV generation
- CNN training with TensorFlow/Keras
- Visualization of training history
- Model saving and inference on new skeleton images

---

## Installation

Install required dependencies:

```bash
pip install opencv-python-headless mediapipe pandas scikit-learn tensorflow matplotlib
```

---

## Folder Structure

```
project_root/
│── source/                # Raw input videos
│── processed_data/        # Extracted frames, cropped frames, skeletons
│── phase_data/            # JSON files with labeled phases
│── phase_labeled_dataset.csv   # Final dataset (image paths + labels)
│── saved_model/           # Trained models (.h5)
│── README.md
│── .gitignore             # Recommended ignore file
```

---

## Pipeline

### Step 1 – Frame Extraction

```python
extract_frames(video_path, output_folder, target_fps=30)
```
Extracts frames at target FPS.

### Step 2 – Black Margin Cropping

```python
crop_black_margins(input_dir, output_dir)
```
Removes black borders from extracted frames.

### Step 3 – Skeleton Generation

```python
generate_skeleton_images(frame_folder, output_folder="skeletons")
```
Generates skeletonized images using **MediaPipe Pose**.

### Step 4 – Phase JSON Mapping

`phase.json` (placed manually in `phase_data`) defines `start_frame` and `end_frame` for each lifting phase.

Example:

```json
{
  "phases": [
    {"phase": "start", "start_frame": 0, "end_frame": 20},
    {"phase": "first_pull", "start_frame": 21, "end_frame": 60},
    {"phase": "catch", "start_frame": 61, "end_frame": 90}
  ]
}
```

### Step 5 – Dataset Creation

```python
label_all_skeletons_to_csv(processed_root, phase_root, "phase_labeled_dataset.csv")
```
Creates a CSV with image paths, phases, and movement type (`FS`, `FCJ`).

---

## Model Training

### Prepare Dataset

```python
df = pd.read_csv("phase_labeled_dataset.csv")
df['combined_label'] = df['phase_label'] + "_" + df['movement_type']
```

### Train CNN

```python
model = create_cnn_model(num_classes)
history = model.fit(train_dataset, validation_data=test_dataset, epochs=15)
```

### Save Model

```python
model.save("saved_model/cnn_pose_classifier.h5")
```

---

## Evaluation

- Training & validation accuracy/loss curves
- Final validation metrics
- Classification report with per-phase performance

---

## Inference

Load the model and predict a single image:

```python
model = load_model("saved_model/cnn_pose_classifier.h5")
load_and_predict_image("processed_data/.../skeleton_frame_0476.jpeg")
```

---

## Notes

- Only **front view videos** are processed in current implementation (`*_front_view.mp4`, `*_front_view.MOV`).
- Requires **manual phase labeling JSONs**.
- Ensure `saved_model/` is in `.gitignore` (do not push large model files to GitHub).

---

## Next Steps

- Extend to side/top view data
- Replace manual JSON with automated phase segmentation
- Experiment with advanced CNNs (ResNet, EfficientNet) or sequence models (LSTMs)

---

## Recommended `.gitignore`

```
# Python cache
__pycache__/
*.pyc

# Saved model
saved_model/

# Processed datasets
processed_data/
phase_data/

# Jupyter checkpoints
.ipynb_checkpoints/

# OS files
.DS_Store
Thumbs.db
```

