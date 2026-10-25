# Resume Screening & Candidate-Job Matching System

An end-to-end, high-performance machine learning and NLP platform designed exclusively for **Resume ↔ Job Description Matching & Candidate Ranking**.

> [!IMPORTANT]
> ### ⚠️ Synthetic Dataset Limitation Notice
> This project is trained, normalized, and benchmarked on the **Candidate-Job Matching Synthetic Dataset** (source: [`michaelozon/candidate-matching-synthetic`](https://huggingface.co/datasets/michaelozon/candidate-matching-synthetic)).
> - **Nature of Data**: The dataset is **completely synthetic** and was created specifically for research into candidate-job matching algorithms, semantic retrieval, and skills gap analysis.
> - **Real-World Limitation**: It **must not** be described as real-world resume or job posting data. It contains synthetic profiles with modeled distributions of seniority, experience, and skills.
> - **Ground Truth**: The dataset includes 75,000 ground-truth candidate-job relationships (2,500 jobs $\times$ 30 relevant candidates) with binary relevance labels. No artificial continuous scores have been fabricated.

---

## 🎯 Project Focus & Core Features

- **Strict Focus on Resume ↔ Job Matching**: Dedicated entirely to semantic and skill-based compatibility between candidate profiles and job requirements. No resume classification or job-category prediction.
- **Dual-Branch Matching Engine**:
  1. **Skill Matching**: Must-have skills (75% weight) and nice-to-have skills (25% weight) coverage scoring against a 73-skill verified lexicon.
  2. **Semantic Text Similarity**: Sublinear TF-IDF (unigrams + bigrams) cosine similarity between resume narratives and job descriptions.
  3. **Multi-Objective Composite Score**: Configurable weighted blend between skill alignment and text relevance.
- **Deep Skill Gap Analysis**:
  - Instant classification of candidate skills into **Matched Skills**, **Missing Required Skills (Critical Gap)**, and **Missing Preferred Skills (Desirable Gap)**.
- **Ground-Truth Benchmark Evaluation**:
  - Quantitative IR evaluation across the 75,000 ground-truth pairs: **Precision@10**, **Precision@30**, **Recall@10**, **Recall@30**, **Recall@50**, **Recall@100**, **MRR**, and **MAP@30**.
- **Interactive MATCHIX Modern Full-Stack Web Application**:
  - **Dark Glassmorphic UI**: Inspired by high-performance ML model evaluation dashboards.
  - **Candidate Ranking**: Query 10,000 candidates against any of the 2,500 benchmark jobs OR custom user job descriptions.
  - **Interactive Pipeline Architecture**: Live visual execution and node inspector of the Dual-Branch Matching Engine.
  - **Upload Support**: Parse and score external PDF, DOCX, and TXT resumes.
  - **1-on-1 Deep Match**: Granular breakdown of individual matches and 3-column skill gap diagnostic.
  - **Benchmark Analytics**: Retrieval curves and score distributions.

---

## 📐 System Architecture

```
Resume (PDF/DOCX/TXT/Corpus)
    ↓
Text Extraction & NLP Normalization (Clean, Stop-words, Lowcase)
    ↓
Skill Extraction (73-Skill Normalized Lexicon)
    ↓
Candidate Representation (Summary + Experience Bullets + Skills)
    ↓
                ┌────────────────────────────────┐
Job Description │                                │
    ↓           │                                │
NLP Normalization │                              │
    ↓           │                                │
Job Representation ──────────────────────────────┤
                                                 ↓
                                          Matching Engine
                                                 ↓
                                   ┌─────────────┴─────────────┐
                                   ↓                           ↓
                            Skill Matching              Text Similarity
                                   ↓                           ↓
                              Skill Score                 TF-IDF Vector
                            (Must vs. Nice)                Cosine Sim
                                   └─────────────┬─────────────┘
                                                 ↓
                                          Overall Match
                                                 ↓
                                        Skill Gap Analysis
                                                 ↓
                                         Candidate Ranking
```

---

## 📁 Project Structure

```
Resume screening and candidate classification/
├── server.py               # Flask REST API backend server
├── run_fullstack.bat       # One-click launcher for frontend + backend
├── run.bat                 # Interactive Windows launcher menu
├── check.bat               # Direct diagnostic health check batch file
├── health_check.py         # Automated end-to-end system verification
├── smoke_test.py           # Unit and pipeline smoke tests
├── config.py               # Paths, weights, and hyperparameters
├── requirements.txt        # Python package dependencies
├── frontend/               # Modern React + Vite Web UI (Port 5173)
│   ├── src/                # Components (Dashboard, Pipeline Architecture, etc.)
│   └── package.json
├── data/
│   ├── dataset_profile.json # Comprehensive JSON profile of the synthetic dataset
│   ├── raw/                 # Downloaded raw parquet files (resumes, jobs, matches)
│   └── processed/           # Normalized candidates.parquet, jobs.parquet, matches.parquet
├── models/
│   ├── skills_vocab.json    # Complete 73-skill lexicon
│   ├── matching_vectorizer.joblib  # Trained TF-IDF feature extractor
│   └── candidate_tfidf.joblib      # Precomputed candidate matrix (10k resumes)
├── outputs/
│   ├── matching_benchmark.json     # Quantitative IR evaluation metrics
│   ├── matching_benchmark.csv
│   ├── retrieval_metrics_bars.png  # Retrieval performance chart
│   └── score_distribution.png      # Score distribution visualization
└── src/
    ├── preprocessing.py    # Schema normalizers & text cleaners
    ├── data_loader.py      # Dataset loader & processor
    ├── matching_engine.py  # Dual-branch matching engine & ranking logic
    ├── benchmark.py        # IR evaluation & visualization generator
    └── file_utils.py       # PDF/DOCX/TXT extraction
```

---

## 🚀 Quick Start

### ⚡ One-Click Run (Windows)
Double-click **`run.bat`** (or execute `.\run.bat` in Command Prompt / PowerShell) to open the interactive menu:
- `[1]` Run System Health & Diagnostics Check
- `[2]` Launch Web Application (Streamlit UI)
- `[3]` Run Quick Smoke Test
- `[4]` Run Ground-Truth Benchmark Evaluation (75,000 Pairs)
- `[5]` Install / Update Dependencies

Or run **`check.bat`** anytime for an instant operational check.

---

### Manual Setup (All Platforms)

1. **Install Dependencies**:
   ```bash
   pip install -r requirements.txt
   ```

2. **Prepare Normalized Dataset**:
   ```bash
   python -m src.data_loader
   ```

3. **Run Benchmark Evaluation**:
   ```bash
   python -m src.benchmark
   ```

4. **Launch Web Application**:
   ```bash
   streamlit run app.py
   ```
   Access the UI at `http://localhost:8501`.
