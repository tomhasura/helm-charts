# DDN Engine Helm Chart

This chart deploys the DDN Engine service.

## Build Source and render manifests and apply
```bash
helm dep update
helm template .

# apply manifests (example)
helm template --set global.containerRegistry="my_docker_registry" --set global.namespace="my_namespace" --set image.repository="my_custom_image" --set image.tag="my_custom_image_tag" . | kubectl apply -f -
```

## Packaged Helm chart

You can pick the tarball for this Helm chart under https://storage.googleapis.com/hasura-ee-charts/ddn-engine-<helm-chart-version\>.tgz

## Prerequisites

1. Helm (preferably v3) installed – instructions are [here](https://helm.sh/docs/intro/install/).
2. Hasura helm repo configured.
  
```bash
helm repo add hasura https://hasura.github.io/helm-charts
helm repo update
```

> You can change the repo name `hasura` to another one if getting conflicts.

## Get Started

```bash
helm install [RELEASE_NAME] hasura/ddn-engine
```
See [configuration](#parameters) below.

See [helm install](https://helm.sh/docs/helm/helm_install/) for command documentation.

## Parameters 

| Name                                              | Description                                                                                                | Value                           |
| ------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- | ------------------------------- |
| `global.containerRegistry`                        | Global container image registry                                                                            | `""`                            |
| `global.namespace`                                | Namespace to deploy to                                                                                     | `"default"`                     |
| `global.serviceAccount.enabled`                   | Creates ServiceAccount resource which references hasura-image-pull Secret                                  | `false`                         |
| `global.dataPlane.deployImagePullSecret`          | Creates Secret of type kubernetes.io/dockerconfigjson with contents of secrets.imagePullSecret             | `false`                         |
| `image.repository`                                | Image repository containing custom created ddn-engine                                                      | `""`                            |
| `image.pullPolicy`                                | Image pull policy                                                                                          | `Always`                        |
| `image.tag`                                       | Image tag to use for custom created ddn-engine                                                             | `""`                            |
| `image.otelCollectorRepository`                   | OTEL collector image repository                                                                            | `otel/opentelemetry-collector`        |
| `image.otelCollectorTag`                          | OTEL collector image tag                                                                                   | `0.104.0`                              |
| `healthChecks.enabled`                            | Enable health check for ddn-engine container                                                               | `true`                         |
| `healthChecks.livenessProbePath`                  | Health check liveness Probe path ddn-engine container                                                      | `"/health"`                       |
| `healthChecks.readinessProbePath`                 | Health check readiness Probe path ddn-engine container                                                     | `"/health"`                       |
| `hpa.enabled`                                     | Enable HPA for mongo-connector.  Ensure metrics cluster is configured when enabling                        | `false`                       |
| `hpa.minReplicas`                                 | minReplicas setting for HPA                                                                                | `2`                       |
| `hpa.maxReplicas`                                 | maxReplicas setting for HPA                                                                                | `4`                       |
| `hpa.metrics.resource.name`                       | Resource name to autoscale on                                                                              | ``                       |
| `hpa.metrics.resource.target.averageUtilization`  | Utilization target on specific resource type                                                               | ``                       |
| `openDDPath`                                      | Path to `opendd.json`                                                                                      | `/md/open_dd.json`              |
| `authnConfigPath`                                 | Path to `auth_config.json`                                                                                 | `/md/auth_config.json`          |
| `metadataPath`                                    | Path to `metadata.json`                                                                                    | `/md/metadata.json`             |
| `enableCors`                                      | Enable CORS by sending appropriate headers                                                                 | `true`                          |
| `deployOtelCollector`                             | Deploy OTEL collector as sidecar to ddn-engine container                                                   | `true`                          |
| `otelEndpoint`                                    | OTEL endpoint under Hasura                                                                                 | `https://gateway.otlp.hasura.io:443`                         |
| `extraVolumes`                                    | Optionally specify extra list of additional volumes for the ddn-engine pod                                 | `[]`                               |
| `extraContainers`                                 | Optionally specify extra list of additional containers for the ddn-engine pod                              | `[]`                               |
| `securityContext`                                 | Define privilege and access control settings for a Pod or Container                                        | `{}`                               |
| `httpPort`                                        | Running port of ddn-engine                                                                                 | `3000`                          |
| `replicas`                                        | Replicas setting for pod                                                                                   | `1`                             |
| `resources`                                       | Resource requests and limits of ddn-engine container                                                       | `{}`                               |
| `env`                                             | Required environment variables for ddn-engine                                                              | `[]`                               |