# Single Feed Player Re-Identification System

## 🎯 Objective
Given a 15-second video (`15sec_input_720p.mp4`), identify each player and ensure that players who go out of frame and reappear are assigned the same identity as before.

## 🚀 Key Features

- **Advanced Player Detection**: Used pretrained YOLOv11 model fine-tuned for sports scenarios
- **Real-time Re-identification**: Maintains consistent player IDs when players go out of frame and return
- **Enhanced Feature Extraction**: 256-dimensional feature vectors combining color, texture, and geometric properties
- **Temporal Consistency**: Sophisticated tracking algorithm with inactive player pool management
- **Visual Enhancement**: Visualization with colored bounding boxes and player statistics

## 📋 Requirements

### System Requirements
- Python 3.8
- GPU support optional (CUDA-compatible for faster processing)

### Dependencies
```bash
pip install -r requirements.txt
```

## 🎬 Usage

### Quick Start

1. **Place your video**: Name it `15sec_input_720p.mp4` in the project directory

2. **Run the system**:
   ```bash
   python single_feed_reid.py
   ```

3. **Output files**:
   - `15sec_output_reidentified_720p.mp4` - Video with consistent player IDs
   - `15sec_output_reidentified_720p_results.json` - Detailed tracking statistics


## 🧠 Algorithm Overview

### 1. Player Detection
- **YOLOv11 Model**: Fine-tuned for sports player detection
- **Confidence Filtering**: 0.3 threshold for optimal detection
- **Size Validation**: Filters unrealistic detections

### 2. Feature Extraction (256-dimensional)
- **Color Features**: RGB + HSV histograms (144 features)
- **Texture Features**: Patch-based statistical descriptors (64 features)
- **Geometric Features**: Aspect ratio and normalized area (2 features)

### 3. Player Tracking States
- **Active Players**: Currently visible in frame
- **Inactive Players**: Temporarily disappeared, available for re-identification
- **Player History**: Complete tracking timeline

### 4. Re-identification Logic
- **Disappearance Tolerance**: 45 frames before moving to inactive
- **Inactive Pool**: 150 frames retention for re-identification
- **Similarity Matching**: Combined feature (70%) + spatial (30%) similarity
- **Re-ID Threshold**: 0.75 confidence for successful re-identification

## 5. Performance Metrics

- **Total Detections**: Player detections across all frames
- **Re-identifications**: Successful player re-identifications after disappearance
- **ID Consistency**: Maintenance of consistent IDs throughout video
- **Processing Speed**: Real-time processing (typically >20 fps)

## 6. Model Information

**YOLOv11 Model**: Fine-tuned for sports player and ball detection
- **Auto-download**: From Google Drive link
- **Model file**: `player_detection_model.pt`
- **Classes**: Person (player) detection optimized

##  Configuration Parameters

```python
max_disappeared = 45          # Frames before moving to inactive
max_inactive_time = 150       # Frames before complete removal
reid_similarity_threshold = 0.75  # Re-identification confidence
confidence_threshold = 0.3    # Detection confidence threshold
feature_history_size = 15     # Feature vector history length
spatial_weight = 0.3         # Weight for spatial distance
feature_weight = 0.7         # Weight for feature similarity
```

##  Project Structure

```
├── single_feed_reid.py          # Main re-identification system
├── requirements.txt             # Python dependencies
├── README.md                   # This documentation
├── 15sec_input_720p.mp4        # Your input video (place here)
└── best.pt    # Auto-downloaded model
```

##  Expected Results

- **Consistent Player IDs**: Same player maintains same ID throughout video
- **Successful Re-identification**: Players who leave frame and return get same ID
- **Robust Tracking**: Handles occlusions and temporary disappearances
- **Real-time Performance**: Processes 15-second video in ~10-20 seconds


##  Getting Started

1. **Install dependencies**: `pip install -r requirements.txt`
2. **Place your video**: Name: `15sec_input_720p.mp4`
3. **Run system**: `python single_feed_reid.py`
4. **Check output**: `15sec_output_reidentified_720p.mp4`

The system is specifically designed for dynamic sports scenarios where players frequently move in and out of frame.


**Input**: `15sec_input_720p.mp4`  
**Output**: `15sec_output_reidentified_720p.mp4`