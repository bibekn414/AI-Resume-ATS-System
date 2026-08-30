# ATS Resume Scorer

A web app that scores how well a resume matches a job description and returns actionable feedback. Built with FastAPI + Streamlit, using spaCy and Sentence Transformers for NLP and the Groq API for LLM-generated suggestions.

## Objective

The main objective of this project is to develop an AI-powered ATS Resume Scoring System that helps candidates understand how well their resume matches a specific job description. The system analyzes the resume using Natural Language Processing (NLP), skill and keyword matching, and semantic similarity techniques to generate an ATS score and identify areas that can be improved.

The project aims to make resume screening more understandable and useful for job seekers by providing category-wise analysis and AI-generated suggestions. Instead of only giving a score, the system provides actionable feedback to help users improve their resume's keywords, skills, content, formatting, and overall ATS compatibility.

## What it does

1. Upload a resume (PDF / DOC / DOCX) and paste a job description.
2. The backend parses the resume, extracts skills and experience, and compares them to the JD using semantic similarity.
3. You get an ATS score, a breakdown by category (formatting, keywords, content, skill validation, ATS compatibility), and LLM-written suggestions for what to improve.
4. Past analyses are saved to your account so you can revisit them.

## Tech stack

- **Frontend:** Streamlit
- **Backend:** FastAPI (Python)
- **NLP:** spaCy (`en_core_web_md`), Sentence Transformers (`all-MiniLM-L6-v2`)
- **LLM:** Groq API (Llama 3)
- **Auth + Database:** Supabase (email/password and Google OAuth)
- **PDF report export:** WeasyPrint + Jinja2

## Project structure

```
ATS_SCORER/
├── backend/              FastAPI app, NLP services, API routes
├── frontend/             Streamlit app, views, components
├── jupyter notebooks/    Research and dataset prep (not used at runtime)
├── ml model/             Exported ML artifacts
├── requirements.txt      Combined backend + frontend dependencies
└── .env.example          Template for environment variables
```

## Setup

### 1. Clone and create a virtual environment

```bash
git clone <repo-url>
cd ATS_SCORER
python -m venv venv
source venv/bin/activate         # Windows: venv\Scripts\activate
```

### 2. Install dependencies

```bash
pip install -r requirements.txt
python -m spacy download en_core_web_md
```

WeasyPrint needs system libraries on Linux:

```bash
# Fedora
sudo dnf install -y cairo pango gdk-pixbuf2 libffi

# Debian / Ubuntu
sudo apt install -y libcairo2 libpango-1.0-0 libpangoft2-1.0-0 libffi-dev
```

### 3. Configure environment variables

Copy the template and fill in your keys:

```bash
cp .env.example .env
```

You need:

- A **Supabase** project — grab `SUPABASE_URL`, `SUPABASE_KEY` (service role), and `SUPABASE_ANON_KEY` from Project Settings → API.
- A **Groq** API key from [console.groq.com](https://console.groq.com).
- (Optional) Google OAuth set up in the Supabase dashboard if you want Google sign-in.

The Streamlit frontend also reads Supabase config from `frontend/.streamlit/secrets.toml`. Copy `secrets.toml.example` to `secrets.toml` and fill it in.

### 4. Run the backend

From the project root:

```bash
uvicorn backend.main:app --reload --host 0.0.0.0 --port 8000
```

The API is now at `http://localhost:8000`.

### 5. Run the frontend

In a new terminal (with the venv activated):

```bash
streamlit run frontend/streamlit_app.py
```

The app opens at `http://localhost:8501`.


## Project Summary

The AI Resume ATS System is a web-based application that evaluates a resume against a given job description. Users can upload their resume in PDF, DOC, or DOCX format and provide the job description they are applying for. The system processes the resume, extracts relevant information such as skills and experience, and compares it with the requirements of the job description.

The application combines NLP techniques, Sentence Transformers, semantic similarity, and an LLM to perform resume analysis. It generates an overall ATS score along with a detailed breakdown covering areas such as formatting, keywords, content, skill validation, and ATS compatibility. The system also provides AI-generated suggestions to help users identify missing skills, improve their resume content, and make their resume more relevant to the target job.

The project uses a FastAPI backend and Streamlit frontend, with Supabase for authentication and storing previous analyses. It also supports generating PDF reports of the analysis. Overall, the system is designed to act as an intelligent resume evaluation and optimization tool that helps candidates prepare more targeted and ATS-friendly resumes for specific job opportunities.

