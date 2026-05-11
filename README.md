# confluent-replicator-helm

Helm chart for deploying [Confluent Replicator](https://docs.confluent.io/platform/current/multi-dc-deployments/replicator/index.html) on Kubernetes for uni-directional Kafka cluster migration.

Used for one-shot uni-directional replication between two Confluent Cloud clusters during a migration. Archived here for reference; no longer in active use.

## What it deploys

- `Deployment` running the Replicator (Connect) worker
- `Service` exposing the Connect REST API (`:8083`)
- `ConfigMap` with the forward connector JSON
- `Job` that submits the connector to the Connect REST API
- `ServiceAccount` (IRSA-annotated) for pulling credentials
- `ExternalSecret` + `SecretStore` (External Secrets Operator) sourcing source/destination API key+secret from AWS SSM Parameter Store

## Layout

```
Chart.yaml
values.yaml
templates/
  configmap.yaml
  deployment.yaml
  service.yaml
  serviceaccount.yaml
  secret.yaml         # ExternalSecret pulling API keys from SSM
  secretstore.yaml    # SecretStore pointing at AWS SSM Parameter Store
development/values.yaml   # example overrides
```

## Usage

```bash
helm install confluent-replicator . -f development/values.yaml
```

Adjust source / destination `bootstrapServers`, `ssmPath`, `serviceAccount.roleArn`, and connector tuning in your own values file.

## License

MIT
