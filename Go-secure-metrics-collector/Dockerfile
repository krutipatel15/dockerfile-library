# syntax=docker/dockerfile:1.6

# 1. Build Stage

FROM golang:1.22.5-bookworm AS builder

WORKDIR /src

# Enable reproducible builds
ENV CGO_ENABLED=0 \
    GOOS=linux \
    GOARCH=amd64

# Copy mod files first for caching
COPY go.mod go.sum ./
RUN go mod download

# Copy source code
COPY . .

# Build statically linked binary
RUN go build -trimpath -ldflags="-s -w" -o /out/metrics-collector ./cmd/collector

# 2. Minimal Runtime

FROM scratch

# Copy CA certs for HTTPS
COPY --from=builder /etc/ssl/certs/ca-certificates.crt /etc/ssl/certs/

# Copy binary
COPY --from=builder /out/metrics-collector /metrics-collector

# Use numeric non-root user (no /etc/passwd needed)
USER 10001

# Expose metrics port
EXPOSE 9090

# Healthcheck using HTTP endpoint
HEALTHCHECK --interval=30s --timeout=2s --retries=3 \
  CMD ["/metrics-collector", "healthcheck"]

# Use read-only root filesystem (runtime flag recommended)

# docker run --read-only ...

ENTRYPOINT ["/metrics-collector"]