````markdown
# 🛰️ Q-Sentinel: EO/IR Threat Detection Module

## Overview

**Q-Sentinel** is an advanced **drone detection and threat assessment system** that integrates **EO (Electro-Optical)** and **IR (Infrared)** imaging to identify and analyze potential military or aerial threats in real time.  

This module extends the base YOLOv8 detection pipeline by introducing a **vision-language reasoning layer** powered by **Google Gemini**, enabling contextual scene understanding — determining whether detected objects indicate an actual threat.

---

## 🚀 Run Instructions

* Run `npm run dev` in `agni-eye-sentinel`
* Run `python app.py` in `backend(LLM)\\backend`
* Run `python -m streamlit run st.py` in `agni-eye-sentinel`
* This project relies on Gemini's vision model capabilities to analyze aerial imagery and classify as **threat** / **non-threat**

---

## 🔑 API Key Usage (Gemini Integration)

This module relies on the **Gemini API key** for sending EO/IR frames or detections for multimodal reasoning.

### 1. Set up your Gemini API Key

Create an environment variable:

```bash
export GEMINI_API_KEY="your_api_key_here"
````

On Windows (PowerShell):

```bash
setx GEMINI_API_KEY "your_api_key_here"
```

### 2. Usage in Code

The key is securely loaded from environment variables to prevent hard-coding sensitive credentials:

```python
import os
from google import genai

GEMINI_API_KEY = os.getenv("GEMINI_API_KEY")

if not GEMINI_API_KEY:
    raise EnvironmentError("Missing Gemini API key. Please set GEMINI_API_KEY.")

client = genai.Client(api_key=GEMINI_API_KEY)
```

### 3. Vision Reasoning Example

```python
response = client.models.generate_content(
    model="gemini-1.5-pro-vision",
    contents=[
        "Analyze the EO/IR image and determine if it indicates a potential threat. "
        "Consider objects, formations, and context.",
        {"mime_type": "image/jpeg", "data": image_bytes},
    ],
)

threat_assessment = response.text
print("Gemini Threat Analysis:", threat_assessment)
```

---

## ⚙️ Installation & Setup

### Clone the Repository

```bash
git clone https://github.com/<your-username>/q-sentinel-eo-ir.git
cd q-sentinel-eo-ir
```

### Install Dependencies

```bash
pip install -r requirements.txt
```

### Environment Variables

Create a `.env` file:

```
GEMINI_API_KEY=your_api_key_here
MODEL_PATH=weights/yolov8_eoir.pt
```

---

## 🧠 System Architecture

```
EO/IR Camera Feed
        │
        ▼
  YOLOv8 Detector (tanks, camps, assets)
        │
        ▼
  Gemini Vision Model → Contextual Analysis
        │
        ▼
  Threat Classifier → Final Assessment
```

---

## 🧩 API Endpoints

| Endpoint         | Method | Description                                              |
| ---------------- | ------ | -------------------------------------------------------- |
| `/analyze/image` | POST   | Accepts EO/IR image and returns detection + threat level |
| `/analyze/video` | POST   | Performs frame-wise detection and analysis on videos     |
| `/status`        | GET    | Health check endpoint                                    |

### Example Request

```bash
curl -X POST http://localhost:8080/analyze/image \
  -F "file=@thermal_feed.jpg"
```

### Example Response

```json
{
  "detections": ["tank", "military_vehicle"],
  "threat_level": "HIGH",
  "reasoning": "Detected multiple armored units in formation."
}
```

---

## 🔬 Tech Stack

| Component             | Technology                     |
| --------------------- | ------------------------------ |
| Object Detection      | YOLOv8                         |
| Vision-Language Model | Gemini 1.5 Pro Vision          |
| Backend Framework     | Flask / FastAPI                |
| Visualization         | Streamlit Frontend             |
| Data Source           | DOTA + Custom EO/IR Dataset    |
| Environment           | Python 3.10+, CUDA-enabled GPU |

---

## 🛡️ Security Notes

* Never commit your API key to version control.
* Rotate your Gemini key periodically.
* EO/IR frames may contain sensitive imagery — store securely.
* Always use HTTPS when sending API requests.

---

## 📈 Future Enhancements

* Support for **multi-frame temporal reasoning** in Gemini
* Integration with **geospatial metadata** (GPS + thermal maps)
* Improved **threat classification model** using ensemble reasoning
* Web-based **operator dashboard** with live EO/IR stream overlays

---

## 📜 License

This project is licensed under the **MIT License**.
© 2025 Q-Sentinel Project. All rights reserved.

```
```
