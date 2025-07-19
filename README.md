# Computer Vision Pipeline: Combining Detection (YOLO), Motion Modeling (Kalman), and Appearance Features (ResNet) for Persistent Tracking

## 🌐 Overview

##### The Challenge
Objects in videos often vanish due to occlusions or detection failures, causing trackers to lose them permanently or assign new IDs incorrectly.

##### Our Solution
A lightweight, real-time tracker that:

- Predicts motion between frames using an optimized Kalman Filter

- Re-identifies objects after gaps using ResNet features

- Balances speed and accuracy: Runs at 30 FPS while minimizing ID switches

##### Best For
- Surveillance, sports analytics, and any scenario where objects get temporarily hidden.


## ✨ Key Features

1. YOLOv8x-Powered Detection
- Uses state-of-the-art YOLOv8x for precise object localization, ensuring high detection accuracy even in crowded scenes.

2. Optimized Kalman Filter
- Our custom-tuned Kalman Filter predicts motion between frames, maintaining smooth trajectories during occlusions.

3. ResNet-Based Re-Identification
- Stores and compares the last 10 appearance features to re-identify objects after detection gaps, reducing ID switches.

4. Real-Time Performance
- Lightweight design processes 30 FPS on mid-range GPUs, ideal for live applications.

5. Adaptive Thresholds
- Configurable parameters (iou_threshold, max_age, feature_threshold) for different scenarios.

6. Occlusion-Resistant Tracking
- Combines motion and appearance cues to handle temporary obstructions effectively.


## 🔧 Pipeline Architecture
[Pipeline Architecture](https://github.com/mohanapavan/multi_object_tracking_with_custom_tracker_and_resnet50_reid/blob/main/images/pipeline.png?raw=true)


## 🔄 How Tracking Works
[How Tracking Works](https://github.com/mohanapavan/multi_object_tracking_with_custom_tracker_and_resnet50_reid/blob/main/images/image.png?raw=true)



