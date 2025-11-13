 # 🎙️ AI Lip-Sync Generator (Wav2Lip + Python Backend + Python Frontend + Colab GPU)

This project is a full working implementation of an **AI-powered lip-sync generation system** built as part of a technical assignment.

It accepts:

- **Image + Text** → converts text → audio → lip-sync  
- **Image + Audio** → directly lip-syncs the face  
- **Outputs:** A talking-face **video** synchronized to the audio

The heavy ML model **Wav2Lip** runs on **Google Colab GPU** (exposed via ngrok) due to the absence of a local GPU.  
The local machine runs:

- 🟦 **FastAPI backend** (TTS + file handling + sending inference requests to Colab)  
- 🟩 **Flask frontend** (simple UI for uploading image/text/audio)

Everything is written in **Python only**, end-to-end.

---

## 🚀 Architecture Overview

```

┌────────────────┐        ┌───────────────────────────────┐
│   Frontend     │        │            Backend            │
│  (Flask + HTML)│  --->  │    FastAPI (TTS + API)        │
└────────────────┘        │  Calls Colab GPU for Wav2Lip  │
▲                         └───────────────────────────────┘
│                                   │
│                                   ▼
│                      ┌────────────────────────┐
└──────────────────────│ Google Colab (GPU)     │
                       │ Wav2Lip Inference API  │
                       └────────────────────────┘

```

---

## ✨ Features

✔ Upload an image + text → auto-speech + lip sync  
✔ Upload an image + audio → direct lip sync  
✔ Full Python Frontend (Flask)  
✔ FastAPI backend with clean modular structure  
✔ Google Colab GPU microservice for heavy Wav2Lip inference  
✔ GTS Text-to-Speech integration  
✔ Supports both MP4 and WAV inputs  
✔ Auto video preview after generation  
✔ Clean architecture suitable for production scaling  

---

## 🗂 Project Structure

```

wav2lip-project/
│
├── backend/
│   ├── app/
│   │   ├── main.py             # FastAPI API endpoints
│   │   ├── colab_client.py     # Communication with Colab Wav2Lip server
│   │   ├── tts.py              # Text → Speech (WAV)
│   │   ├── utils.py            # File saving helpers
│   └── requirements.txt
│
├── frontend/
│   ├── app.py                  # Flask frontend server
│   ├── templates/
│   │   └── index.html          # UI for uploads
│   └── static/                 # (optional)
│
└── colab/
└── wav2lip_inference.ipynb # GPU runtime that acts like inference microservice

````

---

## 🧩 Technologies Used

- **Python 3.9+**
- **FastAPI** — backend REST API  
- **Flask** — frontend UI  
- **Wav2Lip** — lip-sync model  
- **Google Colab GPU** — for inference  
- **ngrok** — expose Colab as API endpoint  
- **gTTS + pydub** — text-to-speech  
- **requests** — backend → Colab communication  

---

# ⚙️ Setup Instructions

## 1️⃣ Clone the Repository
```bash
git clone https://github.com/<your-username>/wav2lip-project.git
cd wav2lip-project
````

---

## 2️⃣ Start the Google Colab GPU Inference Server

1. Open the notebook:
   `colab/wav2lip_inference.ipynb`

2. Run cells in order:

   * Clone Wav2Lip
   * Install requirements
   * Load checkpoint
   * Start Flask + ngrok server

3. Copy the generated public URL, e.g.:

```
http://1234abcd.ngrok-free.app
```

4. Paste this URL into:

```
backend/app/colab_client.py
COLAB_URL = "<your-colab-url>/run-wav2lip"
```

---

## 3️⃣ Install Backend Dependencies

```bash
cd backend
python -m venv .venv
source .venv/bin/activate
pip install -r requirements.txt
uvicorn app.main:app --reload --port 8000
```

Backend will start on:

```
http://localhost:8000
```

---

## 4️⃣ Start the Frontend

```bash
cd frontend
python app.py
```

Frontend will run on:

```
http://localhost:5000
```

---

# 🧪 Usage

### **Generate Lip-Synced Video (Text)**

1. Open the frontend UI
2. Upload image
3. Enter text
4. Submit → backend → colab → result video shown

### **Generate Lip-Synced Video (Audio)**

1. Upload image
2. Upload audio
3. Submit → backend → colab → result video shown

---

# 📡 API Documentation (Backend)

### **POST** `/generate-from-text`

```
Form-data:
  image: <image file>
  text: "Hello world"
Returns:
  video/mp4
```

### **POST** `/generate-from-audio`

```
Form-data:
  image: <image file>
  audio: <audio file>
Returns:
  video/mp4
```

---

# ✔️ Positive Findings (as required)

* Wav2Lip produces very accurate lip movements for frontal faces.
* Using Google Colab GPU removes hardware limitations.
* The architecture is modular and easy to extend.
* FastAPI + Flask combination kept everything Python-based.
* Easy to swap TTS or change inference engine.
* Clear separation of frontend, backend, and ML inference service.

---

# ❌ Negative Findings (as required)

* ngrok URL changes every session → must update manually in backend.
* Colab disconnects after ~90 minutes → temporary.
* GPU inference cannot be locally deployed without hardware.
* Quality reduces for non-frontal or low-light images.
* Some mouth-edge artifacts remain at certain frames.
* Latency increases if hosting Colab in free tier.

---

# 📈 Future Improvements

* Add video stabilizer
* Add face cropping / alignment
* Support multiple faces
* Use Coqui TTS for more natural emotion
* Deploy inference to a real GPU VM (AWS/GCP)
* Add queue system for multiple concurrent jobs
* Build React or Next.js frontend for professional UI

---

# 📝 License

This project is for educational and assignment purposes.

---

# 🙋 About This Project

Built as part of an **AI + React/Python Technical Assignment** to demonstrate:

* ML model orchestration
* GPU offloading
* API development
* Clean software engineering

If you found it useful, please ⭐ the repo!

```
 
