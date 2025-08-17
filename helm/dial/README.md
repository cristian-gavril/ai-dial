# DIAL Platform Helm Chart

This Helm chart provides a one-click deployment of the entire DIAL (Deterministic Integrator of AI and LLMs) platform stack on a Kubernetes cluster.

## Introduction

DIAL is a modular, enterprise-grade ecosystem for building, managing, and deploying generative AI applications. This chart uses an "umbrella" or "parent" chart pattern, where each component of the DIAL platform (e.g., Core, Chat, Adapters) is a sub-chart that can be enabled or disabled.

## Prerequisites

*   Kubernetes 1.19+
*   Helm 3.2+
*   An Ingress controller (like NGINX Ingress Controller) installed in your cluster to expose the DIAL Chat UI.

## Installing the Chart

To install the chart with the release name `my-dial-release`:

```bash
helm install my-dial-release ./dial
```

This will deploy the DIAL platform with the default configuration specified in the `values.yaml` file.

## Upgrading the Chart

To upgrade an existing release:

```bash
helm upgrade my-dial-release ./dial
```

## Configuration

The main configuration is done in the `values.yaml` file. You can create a copy of this file, modify it, and then install the chart using your custom values:

```bash
helm install my-dial-release ./dial -f my-values.yaml
```

Alternatively, you can specify values on the command line using the `--set` flag.

### Enabling and Disabling Components

You can enable or disable each component of the DIAL stack by setting the `enabled` flag for that component to `true` or `false`.

Example `my-values.yaml`:

```yaml
# my-values.yaml

# Disable the OpenAI adapter and the built-in Redis
dial-adapter-openai:
  enabled: false
redis:
  enabled: false

# Configure the DIAL Chat Ingress
dial-chat:
  ingress:
    enabled: true
    hosts:
      - host: dial.mycompany.com
        paths:
          - path: /
            pathType: ImplementationSpecific
    tls:
      - hosts:
          - dial.mycompany.com
        secretName: my-tls-secret

# Configure DIAL Core to use an external Redis
dial-core:
  redis:
    address: "redis://my-external-redis.example.com:6379"
```

### Sub-chart Configuration

All values for a sub-chart are nested under a key with the sub-chart's name. For example, to change the number of replicas for the `dial-core` component, you would set:

```yaml
dial-core:
  replicaCount: 2
```

Please refer to the `values.yaml` file in this chart and in each sub-chart for a full list of configurable options.
