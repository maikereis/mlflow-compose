# MLOps Platform

[![Docker](https://img.shields.io/badge/Docker-Compose-2496ED?logo=docker&logoColor=white)](https://www.docker.com/)
[![MLflow](https://img.shields.io/badge/MLflow-3.6.0-0194E2?logo=mlflow&logoColor=white)](https://mlflow.org/)
[![MinIO](https://img.shields.io/badge/MinIO-S3_Compatible-C72E49?logo=minio&logoColor=white)](https://min.io/)
[![PostgreSQL](https://img.shields.io/badge/PostgreSQL-15-336791?logo=postgresql&logoColor=white)](https://www.postgresql.org/)

A containerized MLOps platform featuring MLflow for experiment tracking and model registry, backed by PostgreSQL for metadata storage and MinIO for artifact storage.

## Architecture

The platform consists of four main services:

- **MLflow Server**: Experiment tracking, model registry, and artifact serving
- **PostgreSQL**: Backend store for MLflow metadata and metrics
- **MinIO**: S3-compatible object storage for model artifacts
- **MinIO Configuration**: Automated bucket creation and setup

## Prerequisites

- Docker Engine 29.1.3, build f52814d
- Docker Compose v5.0.0

## Quick Start

1. Copy the environment template and configure your credentials:

```bash
cp .env.template .env
```

2. Edit `.env` and set your AWS credentials:

```env
AWS_ACCESS_KEY_ID=your_access_key
AWS_SECRET_ACCESS_KEY=your_secret_key
AWS_REGION=us-east-1
```

3. Start the platform:

```bash
# Start all services
docker compose --profile full up -d

# Or start only ML services (excludes optional services)
docker compose --profile ml up -d
```

4. Access the services:

- MLflow UI: http://localhost:5000
- MinIO Console: http://localhost:9001 (credentials from `.env`)
- MinIO API: http://localhost:9000

## Service Details

### MLflow Server

- Tracks experiments, parameters, metrics, and artifacts
- Serves models through a REST API
- Backend: PostgreSQL database
- Artifact storage: MinIO S3-compatible storage

### PostgreSQL

- Stores MLflow experiment metadata
- Database: `mlops`
- User: `mlops`
- Port: 5432

### MinIO

- S3-compatible object storage
- Bucket: `mlflow-artifacts`
- Console port: 9001
- API port: 9000

## Usage

### Connecting to MLflow

Configure your MLflow client:

```python
import mlflow

mlflow.set_tracking_uri("http://localhost:5000")
```

### Environment Variables

The platform uses AWS S3 configuration for MinIO compatibility:

```python
import os

os.environ["MLFLOW_S3_ENDPOINT_URL"] = "http://localhost:9000"
os.environ["AWS_ACCESS_KEY_ID"] = "your_access_key"
os.environ["AWS_SECRET_ACCESS_KEY"] = "your_secret_key"
```

## Docker Compose Profiles

- `full`: Starts all services
- `ml`: Starts core ML services only

## Health Checks

All services include health checks:

- MinIO: 10s interval, 5 retries
- PostgreSQL: 10s interval, 5 retries
- MLflow: 30s interval, 5 retries, 60s start period

## Volumes

Persistent data is stored in named volumes:

- `minio-mlflow`: MinIO object storage data
- `postgres-mlflow`: PostgreSQL database data

## Stopping the Platform

```bash
# Stop all services
docker compose --profile full down

# Stop and remove volumes (WARNING: deletes all data)
docker compose --profile full down -v
```

## Troubleshooting

### Services won't start

Check service health:

```bash
docker compose --profile full ps
docker compose logs <service-name>
```

### Connection issues

Ensure all services are healthy before connecting:

```bash
docker compose --profile full ps
```

### Reset the platform

Remove all containers and volumes:

```bash
docker compose --profile full down -v
```

## License

This project configuration is provided as-is for MLOps platform setup.