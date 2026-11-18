# ShlokaSense 🕉️

An AI-assisted Bhagavad Gita exploration project combining emotion classification with semantic verse retrieval. A user describes a concern; the backend predicts an emotion and retrieves relevant Bhagavad Gita verses using Sentence-BERT embeddings and a FAISS similarity index.

> **Group-project attribution:** This is an independently maintained portfolio copy of the group project originally published at [ShreyVyas123/Shlokasense1](https://github.com/ShreyVyas123/Shlokasense1). The original project and its contributors are acknowledged. This repository does not claim sole authorship of the original work. Please add the full group-member list and individual contribution details after confirming them with the team.

## What is in this repository

- FastAPI backend with root, health-check, and prediction endpoints
- DistilBERT-based emotion classification pipeline
- Sentence-Transformers (all-MiniLM-L6-v2) query embeddings
- FAISS vector similarity search for verse retrieval
- Scripts for dataset cleaning, emotion-label preparation, synthetic query generation, train/validation/test splitting, model training, and evaluation

## High-level flow

1. The API accepts a free-text concern at POST /predict.
2. The DistilBERT classifier predicts an emotion and confidence score.
3. Sentence-BERT encodes the query into a normalized vector.
4. FAISS searches the verse index for the nearest matches.
5. The API returns the predicted emotion and retrieved verse metadata.

**Implementation note:** The inspected source implements emotion classification and retrieval. It does not show an LLM-based response-generation stage or a frontend in this repository snapshot, so those capabilities are not claimed here.

## Repository layout

    ShlokaSense/
    ├── main.py
    ├── src/
    │   ├── __init__.py
    │   └── pipeline.py
    ├── clean_dataset.py
    ├── create_emotion_dataset.py
    ├── create_training_data.py
    ├── prepare_training_data.py
    ├── train_distilbert.py
    ├── evaluate_model.py
    ├── test_retrieval.py
    ├── models/
    │   ├── gita_faiss.index
    │   └── gita_metadata.pkl
    ├── requirements.txt
    └── .gitignore

The Python virtual environment, executable launchers, and compiled bytecode from the original repository are intentionally not copied.

## Setup

Use Python 3.10 or newer. Create and activate a virtual environment, then install dependencies:

    python -m venv .venv
    # Windows PowerShell:
    .venv\Scripts\Activate.ps1
    # macOS/Linux:
    source .venv/bin/activate
    pip install -r requirements.txt

The source code expects a trained emotion model at models/distilbert-emotion/. That trained model directory and the source CSV datasets were not present in the original repository tree inspected for this copy. They must be obtained from the project team or regenerated before running the complete training/inference workflow.

Expected dataset inputs for the data-preparation scripts are under data/, including data/bhagavad_gita.csv. These datasets are not included in this repository snapshot.

## Run the API

After placing the trained model in models/distilbert-emotion/:

    uvicorn main:app --reload

API endpoints:

| Method | Endpoint | Purpose |
|---|---|---|
| GET | / | Basic API status |
| GET | /health | Health status |
| POST | /predict | Emotion prediction and verse retrieval |

Example request:

    {
      "text": "I feel anxious about my exams and future."
    }

Interactive API documentation is available at http://127.0.0.1:8000/docs when the server is running.

## Training workflow

The included scripts represent this intended sequence:

1. clean_dataset.py — clean the Bhagavad Gita source CSV.
2. create_emotion_dataset.py — create initial emotion labels from keyword rules.
3. create_training_data.py — generate labelled example queries.
4. prepare_training_data.py — split examples into training, validation, and test sets.
5. train_distilbert.py — fine-tune DistilBERT.
6. evaluate_model.py — evaluate the trained classifier.
7. test_retrieval.py — test FAISS verse retrieval.

Review the generated labels and synthetic examples before treating evaluation results as representative of real-world user language.

## Technologies

Python · FastAPI · PyTorch · Hugging Face Transformers · Sentence-Transformers · FAISS · NumPy · Pandas · scikit-learn

## Attribution and project history

- Original group repository: https://github.com/ShreyVyas123/Shlokasense1
- This repository is a separately maintained copy for portfolio documentation and further development.
- Original contributors: please add all group members' GitHub usernames/names here after confirmation.
- Individual contributions: describe only the work personally completed by each contributor.

The original repository did not declare a license at the time it was inspected. Public visibility alone does not grant reuse rights; confirm permission and applicable ownership/licensing with the group before redistributing or reusing the code.

## Current limitations

- The trained DistilBERT checkpoint and input datasets are not included in the inspected source repository.
- Reproducible inference requires the trained checkpoint and matching model configuration/labels.
- The API and pipeline should be integration-tested after the missing assets and dependencies are supplied.
- No deployment configuration or frontend source was present in the inspected repository snapshot.
