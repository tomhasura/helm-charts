# DDN Engine Helm Chart

This chart deploys the DDN NDC Postgres connector.

## Build Source, render manifests and apply
```bash
helm dep update
helm template .

# apply manifests (example)
helm template --set global.containerRegistry="my_docker_registry" --set global.namespace="my_namespace" --set image.repository="my_custom_image" --set image.tag="my_custom_image_tag" --set dbUri="db_connection_string" --set serviceTokenSecret="token" . | kubectl apply -f -
```

## Packaged Helm chart

You can pick the tarball for this Helm chart under https://storage.googleapis.com/hasura-ee-charts/ddn-ndc-postgres-<helm-chart-version\>.tgz

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
helm install [RELEASE_NAME] hasura/ddn-ndc-postgres
```
See [configuration](#parameters) below.

See [helm install](https://helm.sh/docs/helm/helm_install/) for command documentation.

## Parameters 

| Name                                              | Description                                                                                                | Value                           |
| ------------------------------------------------- | ---------------------------------------------------------------------------------------------------------- | ------------------------------- |
| `global.containerRegistry`                        | Global container image registry                                                                            | `""`                            |
| `global.namespace`                                | Namespace to deploy to                                                                                     | `"default"`                     |
| `labels.app`                                      | Common label for ndc postgres connector                                                                    | `"ndc-postgres"`                |
| `image.repository`                                | Image repository containing custom created ndc postgres                                                    | `""`                            |
| `image.tag`                                       | Image tag to use for custom created ndc postgres                                                           | `""`                            |
| `image.pullPolicy`                                | Image pull policy                                                                                          | `Always`                        |
| `image.otelCollectorRepository`                   | OTEL collector image repository                                                                            | `otel/opentelemetry-collector`        |
| `image.otelCollectorTag`                          | OTEL collector image tag                                                                                   | `0.104.0`                              |
| `replicas`                                        | Replicas setting for pod                                                                                   | `1`                             |
| `wsInactiveExpiryMins`                            | To be documented                                                                                           | `1`                             |
| `securityContext`                                 | Define privilege and access control settings for a Pod or Container                                        | `{}`                               |
| `healthChecks.enabled`                            | Enable health check for ndc postgres container                                                             | `false`                         |
| `healthChecks.livenessProbePath`                  | Health check liveness Probe path ndc postgres container                                                    | `"/healthz"`                       |
| `healthChecks.readinessProbePath`                 | Health check readiness Probe path ndc postgres container                                                   | `"/healthz"`                       |
| `hpa.enabled`                                     | Enable HPA for ndc postgres.  Ensure metrics cluster is configured when enabling                           | `false`                       |
| `hpa.minReplicas`                                 | minReplicas setting for HPA                                                                                | `2`                       |
| `hpa.maxReplicas`                                 | maxReplicas setting for HPA                                                                                | `4`                       |
| `hpa.metrics.resource.name`                       | Resource name to autoscale on                                                                              | ``                       |
| `hpa.metrics.resource.target.averageUtilization`  | Utilization target on specific resource type                                                               | ``                       |
| `deployOtelCollector`                             | Deploy OTEL collector as sidecar to ndc postgres container                                                 | `true`                          |
| `otelEndpoint`                                    | OTEL endpoint under Hasura                                                                                 | `https://gateway.otlp.hasura.io:443`                         |
| `configDirectory`                                 | Config directory for connector                                                                             | `"/etc/connector"`                               |
| `dbUri`                                           | Database connection string (Required to run ndc postgres)                                                  | `""`                               |
| `serviceTokenSecret`                              | Hasura Service Token Secret (Required to run ndc postgres)                                                 | `""`                               |
| `extraVolumes`                                    | Optionally specify extra list of additional volumes for the ndc postgres pod                               | `[]`                               |
| `extraContainers`                                 | Optionally specify extra list of additional containers for the ndc postgres pod                            | `[]`                               |                               |
| `resources`                                       | Resource requests and limits of ndc postgres container                                                     | `{}`                               |
| `env`                                             | Required environment variables for ndc postgres                                                            | `[]`                               |