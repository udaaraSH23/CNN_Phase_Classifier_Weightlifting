# 📊 Phase Classifier Pipeline (Weightlifting AI Project)

## ✅ Objective
Build a machine learning model to classify lifting phases from side-view skeleton images generated from annotated videos of Clean & Jerk and Snatch techniques.

---

## 🧱 Pipeline Stages

### 1. Frame Extraction
- [ ] Use OpenCV to extract frames from each side-view video
- [ ] Store frames in `processed/` subfolder
- Naming: `frame_0001.jpeg`, `frame_0002.jpeg`, ...

### 2. Skeleton Image Generation
- [ ] Use MediaPipe to annotate pose skeletons on each frame
- [ ] Save output as `skeleton_frame_0001.jpeg` etc. in `processed/`

### 3. Phase Label Mapping
- [ ] Read corresponding `phases.json`
- [ ] Match `start_frame`–`end_frame` ranges to each skeleton image
- [ ] Generate a `labels.csv` with:
  - `image_path`
  - `phase`

### 4. Dataset Output
- [ ] One `labels.csv` per video folder
- [ ] Later: merge all into global CSV for training

---

## 🗂️ Folder Structure

```plaintext
Root/
├── Player_01/
│   └── FS/
│       └── Player01_FS_Set_01/
│           └── Player01_FS_50Kg_side_view/
│               ├── raw_video.mp4
│               ├── phases.json
│               ├── processed/
│               │   ├── skeleton_frame_0001.jpeg
│               │   └── ...
│               └── labels.csv
