# Teralit — Skin Disease Detection System

> **Coding Camp 2026 × DBS Foundation** · Team CC26-PSU247 · Theme: *Healthy Lives & Well-being*

Teralit is a full-stack health application that lets users upload images of skin conditions for AI-powered classification, and then chat with an AI agent about their results. The project is organized as a monorepo using Git submodules, with four separate modules covering the frontend, backend, AI model inference, and data analysis.

---

## Coding Camp Project Introduction

### Project Information

| Item | Description |
|------|-----------|
| **Capstone Project Team ID** | CC26-PSU247 |
| **Project Title** | Teralit - Sistem Pendeteksi Penyakit Kulit |
| **Selected Theme** | Healthy Lives & Well-being |

### Project Contributors

| ID | Name | Role | Status |
|-----------|------|--------|--------|
| CACC589D6Y0463 | Satrio Wibowo | AI Engineer | Active |
| CACC589D6Y0466 | Henoch Kurniawan | AI Engineer | Active |
| CDCC014D6Y1395 | Putu Krisna Udayana | Data Scientist | Active |
| CFCC014D6Y1840 | I Gusti Bagus Rahajeng Danishwara Dipa Pracheta | Full-Stack Web Developer | Active |
| CFCC014D6Y1851 | I Gede Parama Sathiyam Yuda Yana | Full-Stack Web Developer | Active |
| CDCC014D6Y1872 | I Gede Liyang Anugrah Oktapian | Data Scientist | Active |

---

## Repository Structure

```
Teralit---Capstone-Project/
├── ai-model/           # YOLOv8 + Keras skin disease classifier
├── front-end/          # React + Vite web application
├── back-end/           # Node.js + Express REST API
├── data-analysis/      # Python data analysis & Streamlit dashboard
└── .gitmodules
```

Each folder is a Git submodule pointing to its own repository:

| Module | Repository |
|---|---|
| `ai-model` | [satriosukacoding/AI-Teralit](https://github.com/satriosukacoding/AI-Teralit) |
| `front-end` | [dash4k/teralit-frontend](https://github.com/dash4k/teralit-frontend) |
| `back-end` | [dash4k/teralit-backend](https://github.com/dash4k/teralit-backend) |
| `data-analysis` | [Liyang-A-O/Streamlit-Teralit](https://github.com/Liyang-A-O/Streamlit-Teralit) |

---

## Modules

### AI Model (`ai-model`)

A deep learning–based skin disease classifier deployed as a Gradio web app on Hugging Face Spaces.

**Tech Stack:** Python 3.13, TensorFlow/Keras, YOLOv8 (Ultralytics), PyTorch, Gradio 6, scikit-learn, pandas

**Architecture:**

| Component | Detail |
|---|---|
| Backbone | YOLOv8m Classification (`yolov8m-cls.pt`) |
| Keras Wrapper | MobileNetV2 + Channel Attention (SE Block) |
| Custom Layer | `ChannelAttentionLayer` — Squeeze & Excitation |
| Custom Loss | `FocalCrossEntropyLoss` — handles class imbalance |
| Input Size | 224 × 224 × 3 |

**Dataset:**
- Format: COCO JSON (from Roboflow)
- Split: 70% Train / 15% Validation / 15% Test (stratified)

**Usage (Gradio):** Upload a skin image → the model returns the top-3 predicted conditions with confidence scores.

**Usage (Python):**
```python
import keras
model = keras.models.load_model("best_model.h5", ...)
```

---

### Frontend (`front-end`)

A modern single-page application built with React and Vite.

**Tech Stack:** React 19, Vite 8, Tailwind CSS 4, React Router 7, React Hot Toast, React Icons

**Features:**
- Responsive UI for uploading skin images and viewing AI classification results
- Session management and chat interface for AI-assisted follow-up
- Client-side routing and toast notifications

```bash
cd front-end
npm install
cp .env.example .env   # Fill in your values
npm run dev            # Start dev server at http://localhost:5173
npm run build          # Build for production
```

---

### Backend (`back-end`)

A REST API server handling authentication, session management, image uploads, and AI integration.

**Tech Stack:** Node.js (ESM), Express 5, PostgreSQL, Redis, JWT, Multer, Swagger, Resend

**Features:**
- User registration, login, and JWT authentication (access + refresh tokens)
- Session management (create, list, update, delete)
- Image uploads per session
- AI classification results stored per session
- AI agent chat powered by Ollama
- Swagger UI for API documentation
- Database migrations via `node-pg-migrate`
- Email notifications via Resend

**Prerequisites:** Node.js >= 18, PostgreSQL, Redis, a running Model API service, a running Ollama service

```bash
cd back-end
npm install
cp .env.example .env   # Fill in your values
npm run start
```

Key environment variables:

| Variable | Description |
|---|---|
| `APP_URL` | Application URL |
| `HOST` / `PORT` | Server host and port |
| `DATABASE_URL` | PostgreSQL connection string |
| `REDIS_URL` | Redis connection string |
| `JWT_ACCESS_SECRET` | Secret for access tokens |
| `JWT_REFRESH_SECRET` | Secret for refresh tokens |
| `MODEL_API_URL` | URL of the AI classification service |
| `OLLAMA_API_URL` | URL of the Ollama chat service |
| `RESEND_API_KEY` | API key for email notifications |

---

### Data Analysis (`data-analysis`)

Exploratory data analysis and a Streamlit dashboard for the skin disease dataset.

**Team members:**
- CDCC014D6Y1395 — Putu Krisna Udayana (Data Scientist)
- CDCC014D6Y1872 — I Gede Liyang Anugrah Oktapian (Data Scientist)

**Contents:**
- `analisis_data.ipynb` — main analysis notebook
- `dashboard/dashboard.py` — Streamlit dashboard script
- `output/` — cleaned annotations, class distribution CSVs
- `Skin_desease_(Perbaikan)_dataset.coco/` — COCO-format dataset with images and annotation JSON

**Live Dashboard:** https://app-teralit-fmn6grajjkiy2gntdy6sjt.streamlit.app/

```bash
cd data-analysis
pip install -r requirements.txt
jupyter notebook analisis_data.ipynb    # Run analysis
python dashboard/dashboard.py           # Run dashboard locally
```

---

## Getting Started (Full Stack)

### 1. Clone with submodules

```bash
git clone --recurse-submodules https://github.com/Liyang-A-O/Teralit---Capstone-Project.git
cd Teralit---Capstone-Project
```

If you already cloned without `--recurse-submodules`:

```bash
git submodule update --init --recursive
```

### 2. Set up the backend

```bash
cd back-end
npm install
cp .env.example .env
# Edit .env with your database, Redis, and API credentials
npm run start
```

### 3. Set up the frontend

```bash
cd front-end
npm install
cp .env.example .env
# Edit .env with your database, Redis, and API credentials
npm run dev
```

### 4. Run the AI model (optional, for local inference)

```bash
cd ai-model
pip install -r requirements.txt
python app.py
```

### 5. Run the data analysis dashboard (optional)

```bash
cd data-analysis
pip install -r requirements.txt
python dashboard/dashboard.py
```

## License

This project was developed as a capstone for **Coding Camp 2026 × DBS Foundation**. See individual submodule repositories for license details.
