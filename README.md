# 🛡️ ZoneSentinel – Smart Zone-Based Intruder Detection

**ZoneSentinel** is a real‑time computer vision system that detects people in a video stream and flags any person entering a predefined restricted zone. It uses **YOLOv8** for object detection and **Streamlit** for an interactive web interface. The system saves snapshots of intruders and provides detection statistics.

---

## ✨ Features

- ‍🧑 **Person Detection** – YOLOv8 identifies people in every frame.
- 🚧 **Restricted Zone** – Define a polygon area; anyone entering it is marked as an intruder.
- 📸 **Snapshot Logging** – Automatically saves an image when an intruder is detected.
- 📊 **Live Statistics** – Shows total people detected, intruder count, and snapshots saved.
- 🎬 **Processed Video Output** – Annotated video with bounding boxes, zone overlay, and counters.
- 🌐 **Web Interface** – Built with Streamlit; upload any video, view results, and download processed video.

---

## 🧠 How It Works

1. **User uploads a video** (MP4/AVI) via the Streamlit web UI.
2. **Each frame** is passed to a YOLOv8 model (pre‑trained on COCO) to detect persons.
3. For every detected person, the system checks if the person's **foot position** lies inside the user‑defined polygon zone.
4. If inside → red bounding box + `INTRUDER` label + snapshot saved.
5. If outside → green bounding box + `Person` label.
6. Counters are updated across all frames.
7. The annotated video and statistics are displayed in the browser, with an option to download the processed video.

---

## 🛠️ Tech Stack

| Component | Technology |
|-----------|------------|
| Object Detection | YOLOv8 (Ultralytics) |
| Image Processing | OpenCV |
| Web Framework | Streamlit |
| Language | Python 3.11 |
| Video Codec | XVID / AVI (for reliable Windows export) |

---

## 📁 Project Structure

```

ZoneSentinel/
├── app.py                # Streamlit frontend
├── detection.py          # Core detection & zone logic
├── requirements.txt      # Python dependencies
├── README.md             # This file
└── intruder_*.jpg        # Snapshots (auto-generated)

```

---

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone https://github.com/your-username/ZoneSentinel.git
cd ZoneSentinel
```

2. Install Dependencies

It is recommended to use a virtual environment:

```bash
python -m venv venv
source venv/bin/activate        # Linux/Mac
venv\Scripts\activate           # Windows
```

Then install required packages:

```bash
pip install -r requirements.txt
```

3. Run the App Locally

```bash
streamlit run app.py
```

Your browser will open at http://localhost:8501.

4. Upload a Video

· Click "Choose a video..." and select an MP4/AVI file.
· Wait for processing (a progress indicator shows status).
· View the annotated video, statistics, and download the result.

---

🎯 Customising the Restricted Zone

The restricted zone is defined in detection.py using normalised coordinates (relative to frame size). To change it, locate this block:

```python
zone = np.array([
    [int(width * 0.3), int(height * 0.6)],
    [int(width * 0.7), int(height * 0.6)],
    [int(width * 0.7), int(height * 0.9)],
    [int(width * 0.3), int(height * 0.9)]
], dtype=np.int32)
```

Adjust the four points (x,y multipliers) to match your scene. For example, [0.3, 0.6] means 30% from the left edge, 60% from the top edge.

---

☁️ Deploy to Streamlit Cloud

1. Push the repository to GitHub.
2. Go to share.streamlit.io, sign in with GitHub.
3. Click "New app", select your repo, branch main, and set Main file path to app.py.
4. Click Deploy. Your app will be live at https://your-app-name.streamlit.app.

Note: Processing on the cloud uses a CPU – it will work but may be slower than on a local GPU machine.

---

*** Example Output

Input Video Processed Output Snapshot
(raw footage) Green boxes for safe persons, red boxes + "INTRUDER" inside zone intruder_20250506_143022.jpg

Statistics displayed on the Streamlit dashboard:

· Total People Detected – cumulative detections across all frames.
· Total Intruders – number of frames where a person was inside the zone.
· Snapshots Saved – number of intruder images saved.

---

*** Troubleshooting

Problem Likely Fix
streamlit: command not found Use python -m streamlit run app.py
Processed video is 0 bytes Change codec inside detection.py to cv2.VideoWriter_fourcc(*'XVID') and save as .avi (already done in the code).
No snapshots appear Ensure you have write permission in the working directory. Snapshots are saved as intruder_*.jpg.
Low detection accuracy The default YOLOv8n model works well for standard upright persons. Try yolov8s.pt for better accuracy (slower).

---

🙏 Acknowledgements

· Ultralytics YOLOv8 – object detection model.
· Streamlit – web application framework.
· OpenCV – image and video processing.


Built as a portfolio project – demonstrates real‑time zone‑based intruder detection using YOLO and OpenCV.
