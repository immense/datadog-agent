# Datadog OpenTelemetry Collector

Custom Datadog OpenTelemetry Collector with enhanced Kubernetes monitoring capabilities.

## Features

This collector extends the standard Datadog Agent with additional OpenTelemetry components:

### Kubernetes Receivers
- **k8sclusterreceiver**: Cluster-level metrics (nodes, pods, resources)
- **k8sobjectsreceiver**: Kubernetes object events and changes  
- **kubeletstatsreceiver**: Container and pod statistics from kubelet

### Routing Connector
- **routingconnector**: Advanced telemetry data routing based on configurable criteria

## Quick Start

```bash
docker build . -t agent-ddot --no-cache \
  --build-arg AGENT_REPO="datadog/agent" \
  --build-arg AGENT_VERSION="7.65.0-full" \
  --build-arg AGENT_BRANCH="7.65.x"

docker run -e DD_API_KEY=your_key -e DD_SITE=datadoghq.com agent-ddot
```

## Automated Builds

GitHub Actions automatically builds and publishes Docker images:
- **Triggers**: Push to main, tags, daily schedule
- **Registry**: GitHub Container Registry (`ghcr.io`)
- **Signing**: Images signed with cosign
- **Multi-platform**: Supports AMD64 and ARM64

## Documentation

- [Datadog OpenTelemetry Collector Custom Components](https://docs.datadoghq.com/opentelemetry/setup/ddot_collector/custom_components/)
- [GitHub Actions Docker Workflow](.github/workflows/docker-publish.yml)