# ⚽ Single Feed Player Re-Identification System

## Objective

Given a 15-second video (`15sec_input_720p.mp4`), the goal is to identify each player and ensure that players who leave the frame and reappear are assigned the same identity as before.

---

## Key Features

- **Advanced Player Detection**: YOLOv11 model fine-tuned for sports-specific player detection.
- **Real-Time Re-identification**: Maintains consistent player IDs across temporary disappearances.
- **256-Dimensional Feature Vectors**: Combine color histograms, texture descriptors, and geometric properties.
- **Temporal Consistency**: Player history maintained using active/inactive states and feature memory.
- **Annotated Output**: Colored bounding boxes and player statistics rendered on output video.

---

## What This System Does

- Processes a sports video (`15sec_input_720p.mp4`) from a single camera angle.
- Detects players using a fine-tuned YOLOv11 model.
- Extracts color, texture, and geometry-based features.
- Re-identifies players who leave and re-enter the frame.
- Outputs:
  - `15sec_output_reidentified_720p.mp4` — video with player IDs and overlays.
  - `15sec_output_reidentified_720p_results.json` — tracking data and statistics.

---

## Setup & Execution

### Prerequisites

- **Python**: 3.8
- **GPU**: Optional (CUDA-supported devices will improve performance)
- **Input Format**: `.mp4`, 720p recommended

### Install Dependencies

If using Kaggle (where most packages are pre-installed), run:

```python
pip install ultralytics==8.0.196 opencv-python==4.8.1.78 numpy==1.24.3 scipy==1.11.3 scikit-learn==1.3.0 torch==2.0.1 torchvision==0.15.2 
```

---

### Running the System

1. Place your input video in the root directory and rename it to:
   ```
   15sec_input_720p.mp4
   ```

2. Ensure the model weights file is named:
   ```
   best.pt
   ```

3. Open and execute the notebook:
   ```
   Player Re-identification.ipynb
   ```

4. Outputs will be generated as:
   - `15sec_output_reidentified_720p.mp4`
   - `15sec_output_reidentified_720p_results.json`

---

## Algorithm Overview

### 1. Player Detection
- **Model**: YOLOv11 (`best.pt`)
- **Thresholds**:
  - Confidence: 0.3
  - Size filters for bounding boxes

### 2. Feature Extraction (256-Dimensional)
- **Color Features**: RGB + HSV histograms (144 dimensions)
- **Texture Features**: Patch-level statistical descriptors (64 dimensions)
- **Geometric Features**: Aspect ratio and area normalization (2 dimensions)

### 3. Player Tracking States
- **Active**: Currently visible in frame
- **Inactive**: Disappeared recently but still eligible for re-identification
- **History Buffer**: Maintains last N feature vectors for robust re-matching

### 4. Re-identification Logic
- **Disappearance Tolerance**: 45 frames
- **Inactive Retention**: 150 frames
- **Similarity Matching**: Weighted (70% feature + 30% spatial)
- **Re-ID Threshold**: 0.75 for confirming re-identification

---

## Configuration Parameters

```python
max_disappeared = 45
max_inactive_time = 150
reid_similarity_threshold = 0.75
confidence_threshold = 0.3
feature_history_size = 15
spatial_weight = 0.3
feature_weight = 0.7
```

---

## Performance Metrics

| Metric              | Description                                    |
|---------------------|------------------------------------------------|
| Total Detections    | Number of players detected across all frames  |
| Re-identifications  | Players successfully matched after reappearance |
| ID Consistency      | Stability of ID assignment over time          |
| Processing Speed    | ~10–20 seconds for 15s clip (CPU), real-time on GPU |

---

## Known Limitations

- Player identities may **drift or reset** during occlusions.
- **Appearance-based features** are sometimes insufficient for similar-looking players.
- **Bounding box jitter** affects tracking stability.

---

## Recommendations for Future Work

- Train a custom **ReID model** on sports datasets.
- Integrate **pose estimation** and **temporal smoothing** for ID correction.
- Implement a **memory bank** to retain embeddings over longer windows.
- Tune parameters with longer and more diverse input videos.

---

## Project Structure

```
project-root/
├── 15sec_input_720p.mp4              # Input sports video (15 seconds)
├── best.pt                           # YOLOv11 model weights
├── Player Re-identification.ipynb    # Jupyter notebook implementation
├── README.md                         # System documentation (this file)
└── report.md                         # Technical report on methodology and results
```

---

## Final Notes

This is a working prototype of a single-camera sports Re-ID system. While partial success has been achieved in tracking and re-identification, **accurate and consistent identity retention remains a challenge**. The project lays the groundwork for further development and real-world deployment in sports analytics.

---

**Input**: `15sec_input_720p.mp4`  
**Output**: `15sec_output_reidentified_720p.mp4`, `15sec_output_reidentified_720p_results.json`
