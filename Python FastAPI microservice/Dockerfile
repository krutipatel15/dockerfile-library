# syntax=docker/dockerfile:1.6

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


# Runtime Stage

FROM gcr.io/distroless/python3-debian12 AS runtime

# Set runtime environment variables
ENV PYTHONUNBUFFERED=1 \
    APP_ENV=production

WORKDIR /app

# Copy installed Python dependencies from builder
COPY --from=builder /install /usr/local

# Copy application source code
COPY app/ /app/app/

# Create and use non-root user (distroless already uses nonroot)
USER nonroot

# Expose application port
EXPOSE 8000

# Healthcheck for orchestration platforms
HEALTHCHECK --interval=30s --timeout=3s --start-period=10s --retries=3 \
  CMD ["python3", "-c", "import socket; s=socket.socket(); s.connect(('127.0.0.1',8000))"]

# Use exec form for proper signal handling
CMD ["python3", "-m", "app.main"]