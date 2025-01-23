# ArgoCD HELM ApplicationSets Repository

This repository serves as the central location for managing ArgoCD ApplicationSets for various tools deployed with Helm. Each ApplicationSet defines configurations for deploying and managing applications across multiple clusters and environments (e.g., production, staging, and development).

## Repository Structure

The repository contains YAML files defining ApplicationSets. These files are used by ArgoCD to automate the deployment of Helm charts to specified clusters and namespaces. Below is a breakdown of the files and their purpose:

### Files

- **`redis-appset.yaml`**: Defines an ArgoCD ApplicationSet for deploying the Redis Helm chart across production, staging, and development environments.
- **`rabbitmq-appset.yaml`**: Defines an ArgoCD ApplicationSet for deploying the RabbitMQ Helm chart with metrics enabled and a Prometheus service monitor configuration.
- **`grafana-loki-appset.yaml`**: Defines an ArgoCD ApplicationSet for deploying Grafana Loki to manage logs across environments, with configurations for S3 bucket storage.

## ApplicationSet Details

### `redis-appset.yaml`

This ApplicationSet deploys Redis using the [Bitnami Redis Helm chart](https://artifacthub.io/packages/helm/bitnami/redis) to three environments:

- **Production**: 
  - Namespace: `redis-sentinel`
  - Cluster: `https://cluster-production:1234`
- **Staging**: 
  - Namespace: `redis-sentinel`
  - Cluster: `https://cluster-staging:1234`
- **Development**: 
  - Namespace: `redis-sentinel`
  - Cluster: `https://cluster-development:1234`

#### Key Configuration

- **Chart Source**: [Bitnami Redis](https://charts.bitnami.com/bitnami)
  - Chart Version: `20.2.1`
- **Authentication**: Disabled (`auth.enabled: false`)
- **Sync Policy**: Automated with pruning and self-healing enabled
- **Release Name**: `redis`

### `rabbitmq-appset.yaml`

This ApplicationSet deploys RabbitMQ using the [Bitnami RabbitMQ Helm chart](https://artifacthub.io/packages/helm/bitnami/rabbitmq) to three environments:

- **Production**: 
  - Namespace: `rabbitmq`
  - Cluster: `https://cluster-production:1234`
- **Staging**: 
  - Namespace: `rabbitmq`
  - Cluster: `https://cluster-staging:1234`
- **Development**: 
  - Namespace: `rabbitmq`
  - Cluster: `https://cluster-development:1234`

#### Key Configuration

- **Chart Source**: [Bitnami RabbitMQ](https://charts.bitnami.com/bitnami)
  - Chart Version: `14.7.0`
- **Authentication**: 
  - Username: `user`
  - Password: `password`
- **Metrics**: Enabled with Prometheus service monitor and additional configuration
- **Sync Policy**: Automated with pruning and self-healing enabled

### `grafana-loki-appset.yaml`

This ApplicationSet deploys Grafana Loki using the [Grafana Loki Helm chart](https://grafana.github.io/helm-charts) to manage logs across three environments:

- **Production**: 
  - Namespace: `observability`
  - Cluster: `https://cluster-production:1234`
  - Bucket: `grafana-loki-production`
- **Staging**: 
  - Namespace: `observability`
  - Cluster: `https://cluster-staging:1234`
  - Bucket: `grafana-loki-staging`
- **Development**: 
  - Namespace: `observability`
  - Cluster: `https://cluster-development:1234`
  - Bucket: `grafana-loki-development`

#### Key Configuration

- **Chart Source**: [Grafana Loki](https://grafana.github.io/helm-charts)
  - Chart Version: `6.12.0`
- **Storage**: S3 buckets configured for chunks, ruler, and admin storage
  - Access Key: `{{s3AccessKeyId}}`
  - Secret Key: `{{s3SecretAccessKey}}`
  - S3 Endpoint: Configured for Oracle Cloud Object Storage
- **Sync Policy**: Automated with pruning and self-healing enabled
- **Deployment Mode**: Single Binary

## Prerequisites

To use the files in this repository, ensure:

1. **ArgoCD** is installed and configured in your cluster.
2. Appropriate permissions are in place to manage resources in the target namespaces and clusters.
3. The target clusters are registered with ArgoCD.
