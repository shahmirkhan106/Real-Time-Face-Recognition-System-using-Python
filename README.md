# 🎭 Real-Time Face Recognition System

A real-time face recognition system built with Python that detects and identifies known faces through a live webcam feed. The system encodes reference images in advance and matches them against faces detected in each video frame.

---

## 📸 Demo

The system draws bounding boxes around detected faces and labels them with the person's name (or "Unknown" if no match is found).

---

## 🚀 How It Works

### 1. Image Encoding (One-Time Setup)
On startup, the system scans a folder of reference images (named after each person, e.g., `john_doe.jpg`). Each image is:
- Read using **OpenCV**
- Converted from BGR → RGB color space
- Passed through `face_recognition` to extract a **128-dimensional face encoding** (a numerical fingerprint of the face)
- Stored in memory alongside the person's name (derived from the filename)

### 2. Real-Time Detection Loop
Each frame captured from the webcam goes through the following pipeline:

```
Webcam Frame
     ↓
Resize to 25% (for speed)
     ↓
BGR → RGB conversion
     ↓
Detect face locations in frame
     ↓
Generate encodings for detected faces
     ↓
Compare against known encodings (face distance)
     ↓
Label best match → Draw box + name on original frame
     ↓
Display frame
```

### 3. Matching Logic
For each detected face:
- It's compared against all known encodings using `face_recognition.compare_faces()`
- The **face with the smallest Euclidean distance** (most similar) is selected as the best match
- If the best match passes the similarity threshold, the person's name is shown; otherwise "Unknown" is displayed

### 4. Performance Optimization
Frames are resized to **25% of their original size** before processing. Face coordinates are then scaled back up (÷ 0.25) for correct overlay on the original frame. This significantly speeds up detection without sacrificing accuracy.

---

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| **Python 3** | Core programming language |
| **OpenCV (`cv2`)** | Webcam capture, frame rendering, drawing bounding boxes |
| **face_recognition** | Face detection, encoding generation, and comparison |
| **dlib** | Underlying engine powering `face_recognition` (HOG + CNN models) |
| **NumPy** | Array operations, face distance calculations, coordinate scaling |

---

## 📁 Project Structure

```
face-recognition/
│
├── images/                  # Reference images (one per person)
│   ├── john_doe.jpg
│   ├── jane_smith.png
│   └── ...
│
├── simple_facerec.py        # SimpleFacerec class — encoding & detection logic
├── main.py                  # Entry point — webcam loop & display
└── README.md
```

---

## ⚙️ Setup & Installation

### Prerequisites
- Python 3.7+
- A working webcam
- CMake (required to build `dlib`)

### Install Dependencies

```bash
pip install opencv-python face_recognition numpy
```

> **Note:** `face_recognition` requires `dlib`. On some systems you may need to install CMake and build tools first:
> ```bash
> pip install cmake
> pip install dlib
> ```

### Add Reference Images
Place photos of people you want to recognize inside the `images/` folder. **Name each file after the person** — the filename (without extension) becomes the displayed label.

```
images/
├── Elon_Musk.jpg
├── Ada_Lovelace.png
```

### Run

```bash
python main.py
```

Press **`ESC`** to quit.

---

## 📌 Notes

- Works best with clear, front-facing reference images
- The 25% frame resize can be adjusted via `self.frame_resizing` in `simple_facerec.py`
- Multiple faces can be detected and identified simultaneously

---

## 📄 License

MIT License — feel free to use and modify.
