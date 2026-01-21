# Use the official MLFlow image from ghcr.io
FROM ghcr.io/mlflow/mlflow:v3.6.0

# Install pip to upgrade and manage Python packages
RUN pip install --upgrade pip

# Install specific packages required for this application
# 
# - cryptography: A package for cryptographic primitives.
# - boto3: The Amazon Web Services (AWS) SDK for Python.
# - psycopg2-binary: A PostgreSQL database adapter for Python.
RUN pip install cryptography==46.0.3 boto3==1.42.4 psycopg2-binary==2.9.11

# Expose the port 5000 to allow external access
EXPOSE 5000