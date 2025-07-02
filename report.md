# ⚽ Single Feed Player Re-Identification — Technical Report

## Approach & Methodology

This project tackles the challenge of re-identifying players in a single-camera sports video — a scenario typically solved using multi-camera systems or identity-labeled datasets. The goal was to build a lightweight, self-contained pipeline that could:

- Detect players frame-by-frame  
- Assign temporary IDs to each detected player  
- Re-identify players who re-enter the scene after leaving the frame  

### Pipeline Overview

1. **Player Detection**: YOLOv11 (Ultralytics) fine-tuned for sports scenes  
2. **Feature Extraction**: Appearance-based embeddings using pretrained ResNet50  
3. **Tracking**: SORT (Simple Online and Realtime Tracking) to maintain player IDs across frames  
4. **Re-identification**: Cosine similarity between appearance embeddings, aided by spatial proximity  
5. **Visualization**: OpenCV overlays showing player IDs, bounding boxes, and confidence scores

---

## Techniques Tried & Their Outcomes

| Component           | Technique                               | Outcome                                                                 |
|---------------------|------------------------------------------|-------------------------------------------------------------------------|
| Detection           | YOLOv11 via Ultralytics                 | Accurate in most cases; jitter under occlusion                         |
| Feature Extraction  | Pretrained ResNet50                     | Reliable over short time windows; struggled during fast motion         |
| Re-identification   | Cosine Similarity + Centroid Matching   | Worked in small intervals; suffered ID drift during longer gaps        |
| Tracking            | SORT                                    | Lightweight and fast; broke ID continuity on missed detections         |
| Visualization       | OpenCV Annotations                      | Effective and helped during debugging and manual inspection            |

---

## Challenges Encountered

Despite a working prototype, the system does not consistently re-identify players who leave and return to the frame. Key limitations include:

- **Occlusion and Frame Exits**: Players frequently exiting the frame broke ID continuity  
- **Appearance Similarity**: Similar-colored jerseys made it hard to distinguish players  
- **Short Video Length**: Not enough frames to build strong identity features  
- **Embedding Fragility**: Cosine similarity alone wasn't reliable for high-confidence re-identification

While functional, the current implementation struggles with ID consistency — particularly under fast motion and occlusion.

---

## Incomplete Aspects & Future Improvements

If given more time and computational resources, the system could be significantly improved with:

- **Custom ReID Training**: Fine-tune a model on sports-specific re-identification datasets  
- **Memory Bank**: Maintain historical feature vectors to match returning players over longer windows  
- **Pose & Motion Integration**: Use skeletal keypoints and motion cues in addition to appearance  
- **Adaptive Thresholding**: Dynamically adjust similarity thresholds based on frame context  
- **Pipeline Optimization**: Improve latency and resilience for longer, more complex input videos

---

## Reproducibility

This project is fully reproducible with minimal setup:

1. Clone/download the project directory  
2. Install dependencies using:
   ```bash
   pip install -r requirements.txt
   ```
3. Place your input video and name it:
   ```
   15sec_input_720p.mp4
   ```
4. Run the script:
   ```bash
   python single_feed_reid.py
   ```
   Or step through `player-re-identification.ipynb` in order.

### Output Files

- `15sec_output_reidentified_720p.mp4` — annotated video  
- `15sec_output_reidentified_720p_results.json` — tracking and ID statistics  

Dependencies include: Ultralytics, PyTorch, OpenCV, NumPy, SciPy, scikit-learn, gdown.

---

## Final Thoughts

This project reflects a constraint-aware approach to a real-world computer vision challenge. While the current system does not fully achieve robust, consistent player re-identification, it demonstrates a functional pipeline and highlights key areas for future advancement.

It stands as a working proof-of-concept: capable of detection, tracking, and partial re-ID — but requiring deeper integration and domain-specific enhancements to be production-ready.

---
