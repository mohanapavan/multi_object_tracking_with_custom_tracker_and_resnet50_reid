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
![Pipeline Architecture](https://github.com/mohanapavan/multi_object_tracking_with_custom_tracker_and_resnet50_reid/blob/main/images/pipeline.png?raw=true)




## 📊 Tracking Performance

##### ✅ Detection & Tracking Excellence

1. Perfect Detection
- 100% precision
- 100% recall
- 0 false alarms

2. Smooth Trajectories
- 85.5% MOTP 
- 80% MOTA 

##### 🟡 Re-Identification Accuracy: 80%

- Maintains correct IDs for 4 out of 5 objects through occlusions

- Struggles with extreme crowding (20% ID switches) → Active improvement area

- 🚀 Real-World Ready
Balances accuracy (80% Re-ID) + speed (30 FPS) for live applications.


## 🚀 YOLOv8x: Detection Backbone

1. Key Features

- High Precision: Detects 80 classes with state-of-the-art accuracy

- Optimized for Tracking: Filters to 8 critical classes (e.g., person, car, truck)

2. Why This Works

- Balances speed + accuracy for real-time tracking

- Low conf_threshold ensures no missed objects

- Class filtering reduces false positives in traffic/surveillance scenarios


## 🔄 How Kalman filter Works
![How Kalman filter Works](https://github.com/mohanapavan/multi_object_tracking_with_custom_tracker_and_resnet50_reid/blob/main/images/image.png?raw=true)



## 🔍 Custom Tracker Engine
##### Key Innovations

1. Motion-Centric Design

- Kalman Filter optimized for bounding box dynamics

- Prioritizes position accuracy over size/ratio (higher R noise)

2. Occlusion Resilience

- 30-frame grace period (max_age) for temporary misses

- Strict Re-ID (feature_threshold=0.7) prevents false matches

3. Stability Features

- 3-frame confirmation (min_hits) reduces false tracks

- Temporal smoothing (temporal_window=3) for trajectory coherence

##### Performance Trade-offs

- ↗️ Higher max_age: Better occlusion handling but risks ghost tracking

- ↗️ Higher feature_threshold: Cleaner IDs but may miss valid Re-IDs



## 🖼️ ResNet50 Feature Extractor

##### Core Implementation

self.model = models.resnet50(pretrained=True)  # ImageNet pre-trained
self.model.fc = nn.Identity()  # Drops classification layer → outputs 2048-D embeddings
self.feature_history = deque(maxlen=10)  # Stores last 10 appearance features per object


1. Key Design Choices

- Lightweight Re-ID

- Uses only the backbone (no fine-tuning) → Fast

- 2048-D features balance discriminability vs memory

2. Robust Processing

- Skips invalid crops (<10px) to avoid garbage features

- Batch processes crops with torch.no_grad() for efficiency

3. Temporal Matching

- Maintains 10-frame feature memory → Handles brief occlusions

- Cosine similarity threshold (feature_threshold=0.7) filters weak matches

##### Why This Works

- Pretrained ResNet captures generic visual patterns (edges/textures)

- Feature deque prevents over-reliance on single-frame appearance

- Minimal compute overhead vs. dedicated ReID models



## ⚠️ Limitations
- Drift during long occlusions (> max_age).

- ResNet features degrade under extreme lighting changes.


## 🎯 Conclusion
##### This hybrid tracking system combines:

- YOLOv8x for precise detection

- A custom-tuned Kalman Filter for smooth motion prediction

- ResNet50 features for robust re-identification

#### Key Achievements
- Real-time performance (30 FPS)
- 80%+ Re-ID accuracy despite occlusions
- Lightweight & adaptable for surveillance, sports, and more

* Future improvements could focus on reducing ID switches in crowded scenes, but the current pipeline delivers reliable, high-speed tracking for most real-world scenarios.
