# house-price-ml-serving
house-price-ml-serving/
├── app/
│   ├── main.py
│   ├── schemas.py
│   ├── model_loader.py
│   ├── predict.py
│   └── logging_conf.py
├── ml/
│   ├── train.py
│   ├── features.py
│   ├── evaluate.py
│   └── config.py
├── artifacts/
│   ├── model.joblib
│   └── metadata.json
├── data/
│   ├── raw/
│   └── sample_requests/
├── tests/
│   ├── test_health.py
│   └── test_predict.py
├── docker/
│   ├── Dockerfile
│   └── docker-compose.yml
├── Makefile
├── pyproject.toml
├── README.md
└── .gitignore
