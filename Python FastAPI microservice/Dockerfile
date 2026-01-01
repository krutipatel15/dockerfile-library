# Builder Stage


FROM python:3.11.9-slim-bookworm AS builder

# Prevent Python from writing .pyc files and enable unbuffered logs
ENV PYTHONDONTWRITEBYTECODE=1 \
    PYTHONUNBUFFERED=1

WORKDIR /build

# Install build dependencies only
RUN apt-get update && apt-get install -y --no-install-recommends \
    build-essential gcc curl \
    && rm -rf /var/lib/apt/lists/*

# Copy dependency files separately for layer caching
COPY requirements.txt .

# Install dependencies into a virtual environment-like folder
RUN pip install --upgrade pip \
    && pip install --no-cache-dir --prefix=/install -r requirements.txt
