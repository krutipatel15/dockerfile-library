# Secure Metrics Collector - Production-Grade Docker Image

This project demonstrates how to build a highly secure, minimal, and production-ready Docker image for a Go-based metrics collector application.  
The focus is on container security, resilience, and cloud-native best practices.

## Objectives

- Build a minimal and secure Docker image using scratch base
- Harden container runtime with non-root user and read-only filesystem
- Implement health checks for orchestration platforms
- Ensure reproducible and static binary builds
- Demonstrate advanced Docker security and resilience features

## Key Features

- Multi-stage build for clean runtime
- Scratch runtime image for minimal attack surface
- Statically compiled Go binary for fast startup and small image size
- Numeric non-root user execution
- Healthcheck endpoint for monitoring
- Supports read-only filesystem
- Capability drop and resource limits recommended at runtime
- Multi-arch friendly design

## Dockerfile Highlights

Practice                         Reason
Multi-stage build                Keeps runtime image clean and small
Scratch runtime                  Removes OS, shell, and package managers
Static Go binary                 Fast startup and minimal runtime dependencies
Non-root numeric user            Security hardening
Read-only filesystem             Prevents runtime modifications
Healthcheck                      Enables automatic failure detection
ENTRYPOINT                       Proper signal handling for graceful shutdown
Explicit environment variables   Reproducible and controlled builds

## Security Considerations

- Runs as non-root numeric user
- No OS, no shell, no package manager in runtime
- Read-only filesystem recommended
- Capability drop and resource limits advised for extra isolation
- Statically compiled binary ensures minimal attack surface

## Learning Focus

This project focuses on container security, minimalism, and resilience rather than application complexity, reflecting advanced DevOps practices.