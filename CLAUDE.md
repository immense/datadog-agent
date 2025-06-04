# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Project Overview

This is a Datadog OpenTelemetry Collector project that builds a custom OTel agent with Datadog integration. The project extends the Datadog Agent with OpenTelemetry capabilities using a custom manifest and configuration.

## Architecture

- **Dockerfile**: Multi-stage build that clones the Datadog Agent source, builds a custom OTel collector using the manifest.yaml, and creates a final image with the built binary
- **manifest.yaml**: Defines the OpenTelemetry Collector components (receivers, processors, exporters, connectors) and their versions to include in the build
- **config.yaml**: OpenTelemetry Collector configuration with OTLP receivers, Datadog exporters, and a routing connector for traces/metrics

## Custom Components

This collector includes several specialized components for Kubernetes and Datadog integration:

### Kubernetes Receivers
- **k8sclusterreceiver**: Collects cluster-level metrics like node status, pod counts, and resource utilization
- **k8sobjectsreceiver**: Monitors Kubernetes object changes and events for logs and metrics
- **kubeletstatsreceiver**: Gathers detailed container and pod statistics from the kubelet API

### Connectors
- **routingconnector**: Routes telemetry data based on configurable criteria, enabling complex data flow patterns
- **datadog/connector**: Custom Datadog connector for generating metrics from trace data

For detailed information about customizing components, see the [Datadog OpenTelemetry Collector documentation](https://docs.datadoghq.com/opentelemetry/setup/ddot_collector/custom_components/).

## Build Process

The Docker build:
1. Uses the Datadog Agent source code from GitHub
2. Installs Go and Python dependencies 
3. Uses the `dda` tool to generate collector code based on manifest.yaml
4. Builds the OTel agent binary
5. Copies the binary into the final Datadog Agent image

## Development Commands

**Build the Docker image:**
```bash
docker build -t agent-ddot .
```

**Build with custom agent version:**
```bash
docker build --build-arg AGENT_VERSION=7.65.0-full --build-arg AGENT_BRANCH=7.65.x -t agent-ddot .
```

**Run the container:**
```bash
docker run -e DD_API_KEY=your_key -e DD_SITE=datadoghq.com agent-ddot
```

## Key Files

- `manifest.yaml` - Controls which OTel components are included in the build
- `config.yaml` - Runtime configuration for the OTel collector
- `Dockerfile` - Build instructions and multi-stage setup
- `.github/workflows/docker-publish.yml` - CI/CD pipeline for automated builds