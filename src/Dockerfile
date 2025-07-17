# Use slim base Python image
FROM python:3.12-slim-bookworm

# Install build dependencies
RUN apt-get update && apt-get install -y \
    build-essential \
    libpq-dev \
    && rm -rf /var/lib/apt/lists/*

# ✅ Upgrade pip, setuptools, and wheel — needed for building packages from source
RUN pip install --upgrade pip setuptools wheel

# Copy requirements file and install dependencies
COPY requirements.txt .
RUN pip install --no-cache-dir --upgrade -r requirements.txt

# Copy project files into the image
COPY . /code

# Set working directory (recommended)
WORKDIR /code

# Set PYTHONPATH environment variable
ENV PYTHONPATH=/code

# Install project in editable mode
RUN pip install -e .

# Optional: run training pipeline during build (only if model training is light)
RUN python prediction_model/training_pipeline.py

# Expose API port
EXPOSE 8005

# Run your app
CMD ["python", "main.py"]
