# MLOps Continuous Delivery Demo

Flask inference API with CI/CD pipeline using GitHub Actions and GHCR.

## Endpoints

- `GET /` — service info
- `GET /health` — health check, returns model version
- `POST /predict` — send `{"value": 5}`, get back `{"input": 5, "prediction": 10}`

## Running locally

```
python -m venv .venv
.venv\Scripts\activate
pip install -r requirements.txt
python app.py
```

## Running with Docker

```
docker build -t mlops-cd-demo:local .
docker run --rm -p 5000:5000 mlops-cd-demo:local
```

## Tests

```
pytest tests/ -v
```

## Verified Working

Tested locally on Windows with Docker Desktop. Pulled both versions from GHCR:

- `ghcr.io/salarshoaib/mlops-cd-demo:1.0.0` — returns `model_version: 1.0`
- `ghcr.io/salarshoaib/mlops-cd-demo:1.1.0` — returns `model_version: 1.1`

Rollback works: swap `:1.1.0` for `:1.0.0` and the older model version is restored instantly.

## CD Pipeline

Tag a version to trigger the pipeline:

```
git tag v1.0.0
git push origin v1.0.0
```

Pipeline: test → build → push to GHCR → deploy staging → approve → deploy production
