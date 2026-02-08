# House Price ML Serving

An end-to-end machine learning inference service built on the Kaggle **House Prices: Advanced Regression Techniques** dataset.

This project demonstrates a production-style ML workflow, including reproducible training pipelines, feature preprocessing, model versioning, and an online inference service implemented with FastAPI.

---

## Project Overview

This repository implements a full machine learning lifecycle for a tabular regression problem:

- Offline model training with consistent preprocessing using `scikit-learn` Pipelines
- Model artifact versioning with metadata for reproducibility
- An online inference service built with FastAPI
- Strong input validation via Pydantic schemas
- Dockerized deployment and automated testing

The focus of this project is **engineering-quality ML system design**, rather than notebook-based experimentation or leaderboard optimization.

---

## Architecture

```
Offline Training
┌────────────┐
│ Kaggle CSV │
└─────┬──────┘
      ↓
┌────────────────────────┐
│ Feature Engineering    │
│ + Preprocessing        │  (sklearn Pipeline)
└─────┬──────────────────┘
      ↓
┌────────────────────────┐
│ Trained Model Artifact │  model.joblib
└─────┬──────────────────┘
      ↓
┌────────────────────────┐
│ Metadata               │  metadata.json
│ (metrics, schema hash) │
└────────────────────────┘


Online Serving
┌────────────────────────┐
│ FastAPI Inference API  │
│  /predict              │
│  /health               │
│  /model-info           │
└────────────────────────┘
```

---

## Tech Stack

- **Language**: Python 3.11
- **Machine Learning**: scikit-learn (Pipeline, ColumnTransformer)
- **Models**: Regularized linear models / tree-based models (configurable)
- **API**: FastAPI, Uvicorn
- **Validation**: Pydantic
- **Testing**: Pytest
- **Packaging & Deployment**: Docker, Docker Compose
- **Code Quality**: Ruff, Black

---

## Repository Structure

```
house-price-ml-serving/
├── app/                 # Online inference service
├── ml/                  # Offline training & evaluation
├── artifacts/           # Model artifacts and metadata
├── data/
│   ├── raw/             # Kaggle CSV files (not committed)
│   └── sample_requests/ # Example API input payloads
├── tests/               # Unit and API tests
├── docker/              # Docker configuration
├── Makefile
├── pyproject.toml
└── README.md
```

---

## Getting Started

### 1. Environment Setup

```bash
python -m venv .venv
source .venv/bin/activate
```

Install dependencies:

```bash
pip install -e .[dev]
```

---

### 2. Train the Model

Place the Kaggle `train.csv` file under `data/raw/`, then run:

```bash
make train
```

This command will:

- Train the model using cross-validation
- Persist the full preprocessing + model pipeline
- Save model metadata for reproducibility

Artifacts are stored under the `artifacts/` directory.

---

### 3. Start the Inference Service

```bash
make serve
```

Once running, API documentation is available at:

```
http://localhost:8000/docs
```

---

### 4. Example Prediction Request

```bash
curl -X POST http://localhost:8000/predict \
  -H "Content-Type: application/json" \
  -d @data/sample_requests/single.json
```

Batch prediction is also supported.

---

## API Endpoints

| Method | Endpoint      | Description                                |
|------|---------------|--------------------------------------------|
| GET  | `/health`     | Service health check and model version     |
| GET  | `/model-info` | Model metadata and training metrics        |
| POST | `/predict`    | House price prediction (supports batching)|

---

## Testing

Run the full test suite:

```bash
make test
```

Tests cover:

- API health endpoint
- Input schema validation
- Prediction endpoint behavior

---

## Docker

Build and run the service using Docker:

```bash
docker compose up --build
```

The service will be exposed on port `8000`.

---

## Engineering Highlights

- **Train–Serve Consistency**  
  Preprocessing and model logic are bundled into a single sklearn Pipeline to avoid feature skew.

- **Model Versioning & Reproducibility**  
  Each trained model is saved together with metadata including cross-validation metrics, schema hash, and timestamp.

- **Production-Oriented API Design**  
  Strong input validation, clear separation of concerns, and structured logging.

- **Clean ML / Serving Boundary**  
  Offline training logic is fully decoupled from the online inference layer.

---

## Notes

- Raw Kaggle datasets are intentionally excluded from version control.
- This project prioritizes system design and engineering practices over competition performance.

---

## License

MIT
