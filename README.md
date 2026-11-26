<div align="center">

# Unicollect Guard

**Multi-Model Persian, Fingilish & English Text Moderation Engine, JSON API & AI Chat Platform**

[![Python](https://img.shields.io/badge/Python-3.11+-3776AB?style=for-the-badge&logo=python&logoColor=white)](https://www.python.org/)
[![FastAPI](https://img.shields.io/badge/FastAPI-API%20v1-009688?style=for-the-badge&logo=fastapi&logoColor=white)](https://fastapi.tiangolo.com/)
[![React](https://img.shields.io/badge/React-18-61DAFB?style=for-the-badge&logo=react&logoColor=0B1120)](https://react.dev/)
[![Transformers](https://img.shields.io/badge/Hugging%20Face-5--Model%20Ensemble-FFD21E?style=for-the-badge&logo=huggingface&logoColor=0B1120)](https://huggingface.co/)
[![License: MIT](https://img.shields.io/badge/License-MIT-8B5CF6?style=for-the-badge)](./LICENSE)

<br/>

Unicollect Guard evaluates **Persian**, **Fingilish (romanized Persian)**, and **English** text through a weighted **5-model Transformer consensus engine**, delivering fast toxicity verdicts, sentence-level scoring, automatic censoring, Fingilish detection & transliteration, an OpenAI-compatible moderation endpoint, and a sleek dark-mode web suite.

[Highlights](#highlights) • [How It Works](#how-it-works) • [API v1](#api-v1-reference) • [Quick Start](#quick-start) • [Project Layout](#project-structure) • [License](#license)

</div>

---

<div align="center">

## Highlights

</div>

| Capability | Description |
| :--- | :--- |
| **5-Model Weighted Consensus** | Combines dedicated Persian, multilingual, and English Transformer classifiers (`app/engine.py`, `app/consensus.py`) into calibrated `safe`, `suspect`, and `toxic` verdicts. |
| **Native Fingilish Pipeline** | Automatically detects romanized Persian (`app/fingilish.py`), transliterates it, and scores both original and normalized forms. |
| **Complete Public JSON API (`/api/v1`)** | Single & batch checks, OpenAI-style `/moderations`, sentence breakdown, profanity detection, smart censoring, language detection, and interactive OpenAPI `/docs`. |
| **Plum-Night Web Suite** | React 18 + esbuild front-end prebuilt into `app/static/` (Guard Panel), `app/chat_static/` (Local LLM Chat), and `pages/` (SSR-prerendered public site & live API console) with bundled `Vazirmatn` & `JetBrains Mono` fonts. |
| **Stateless & Privacy-First** | Submitted text is analyzed strictly in memory — user messages are never persisted to disk. |

---

<div align="center">

## How It Works

</div>

<div align="center">

`Input Text` ➔ **1. Language & Fingilish Detection** ➔ **2. Parallel 5-Model Inference** ➔ **3. Weighted Consensus Voting** ➔ `Final Verdict (safe / suspect / toxic)`

</div>

<br/>

| Route | Purpose |
| :--- | :--- |
| `/` | Interactive Guard Web Panel (`app/static`) |
| `/api/v1/*` | Full REST JSON API (`app/api_v1.py`) |
| `/docs` · `/redoc` | Interactive Swagger & ReDoc OpenAPI specifications |
| `/site/*` · `/chat` | Prerendered public site (`pages/`) & chat interface |
| `/health` | Service & model readiness probe |

---

<div align="center">

## API v1 Reference

</div>

| Method | Endpoint | Description |
| :--- | :--- | :--- |
| `POST` | `/api/v1/check` | Full ensemble analysis with per-model scores, verdict, and metadata |
| `POST` | `/api/v1/check/batch` | Analyze multiple texts in a single request |
| `POST` | `/api/v1/verdict` | Compact verdict-only classification (`safe`, `suspect`, `toxic`) |
| `POST` | `/api/v1/score` | Numeric toxicity score breakdown |
| `POST` | `/api/v1/sentences` | Per-sentence toxicity analysis |
| `POST` | `/api/v1/censor` | Automatic masking of flagged spans |
| `POST` | `/api/v1/moderations` | OpenAI-compatible moderation response format |
| `POST` | `/api/v1/profanity` | Lexicon & profanity match detection |
| `POST` | `/api/v1/language` | Persian / Fingilish / English language identification |
| `POST` | `/api/v1/fingilish/detect` | Detect whether input text is Fingilish |
| `POST` | `/api/v1/fingilish/transliterate` | Convert Fingilish text into Persian script |
| `POST` | `/api/v1/normalize` | Unicode & Persian text normalization |
| `GET` | `/api/v1/models` · `/api/v1/health` | List loaded ensemble models and live health status |

### Example Request

```bash
curl -X POST http://localhost:8000/api/v1/check   -H "Content-Type: application/json"   -d '{"text": "Hello! Everything looks great today."}'
```

---

<div align="center">

## Quick Start

</div>

### 1. Run the Guard API & Web Panel

```bash
git clone https://github.com/TernuxPJ/Unicollect.git
cd Unicollect

python3 -m venv .venv && source .venv/bin/activate
pip install -r requirements.txt

# Download the ensemble checkpoints
python3 scripts/download_models.py

# Start the FastAPI service
uvicorn app.main:app --host 0.0.0.0 --port 8000
```

Open `http://localhost:8000` for the Guard Panel or `http://localhost:8000/docs` for the OpenAPI explorer.

### 2. Rebuild the Front-End (Optional)

Prebuilt bundles are already committed so Node.js is not required in production. To develop or rebuild the UI:

```bash
cd web
npm install
npm run build
```

### 3. Run the Test Suite

```bash
pip install -r requirements-dev.txt
pytest
```

---

<div align="center">

## Project Structure

</div>

```text
Unicollect/
├── app/
│   ├── main.py             # FastAPI entrypoint, middleware & static mounts
│   ├── api_v1.py           # Public JSON API v1 routes
│   ├── engine.py           # 5-model Transformer inference engine
│   ├── consensus.py        # Weighted ensemble voting & verdict calibration
│   ├── fingilish.py        # Fingilish detector & transliterator
│   ├── chat_app.py         # Unicollect Chat (local LLM backend)
│   ├── static/             # Prebuilt Guard Web Panel
│   └── chat_static/        # Prebuilt Chat UI
├── pages/                  # Prerendered static public site & API docs
├── web/                    # React 18 + esbuild front-end source
├── scripts/                # Model downloaders, server runners & smoke tests
├── tests/                  # Pytest unit & API integration tests
├── LICENSE                 # MIT License
└── README.md
```

---

<div align="center">

## License

Released under the **[MIT License](./LICENSE)** — Copyright © 2026 **Muvixo**.

<br/>

<sub>Built with care by <b>Muvixo</b> · Unicollect Guard</sub>

</div>
