# kubero-helm

A Helm chart to deploy a [Kubero](https://github.com/kubero-dev/kubero-operator) instance (Operator and UI).

## Overview

This chart deploys the Kubero Operator into your Kubernetes cluster. The operator manages the full Kubero ecosystem including application workloads, CI/CD pipelines, builds, and a wide range of database and service addons. It installs the operator controller-manager, all required CRDs, RBAC resources, and a metrics service.

Templates are auto-generated from the [upstream operator manifest](https://raw.githubusercontent.com/kubero-dev/kubero-operator/main/deploy/operator.yaml) using a bundled Ruby script.

## Prerequisites

- Kubernetes 1.19+
- Helm 3.x
- Cluster-admin access (the chart installs CRDs and ClusterRoles)

## Installation

Add the Helm repository and install the chart:

```bash
helm repo add kubero https://general-intelligence-systems.github.io/kubero-helm
helm repo update
helm install kubero kubero/kubero
```

To install into a specific namespace:

```bash
helm install kubero kubero/kubero --namespace kubero-operator-system --create-namespace
```

## Uninstallation

```bash
helm uninstall kubero
```

> **Note:** Helm does not remove CRDs on uninstall. To fully clean up, delete them manually:
>
> ```bash
> kubectl get crds -o name | grep kubero.dev | xargs kubectl delete
> ```

## Configuration

This chart does not expose configurable values yet. The `values.yaml` file is currently empty. Parameterization (e.g. image overrides, resource limits, replica counts) will be added in future versions.

## Development

### How templates are generated

The `bin/manifest-to-templates` script (Ruby) downloads the monolithic upstream operator manifest, splits it into individual YAML files, and writes them to `charts/kubero/templates/` (with CRDs going into `charts/kubero/templates/crds/`).

To regenerate templates locally:

```bash
# Requires Ruby 3.3+
bin/manifest-to-templates
```

This can also be triggered via the **Update Operator Templates** GitHub Actions workflow (`workflow_dispatch`), which runs the script and auto-commits the result.

### Releasing

Pushes to `main` automatically package and publish the chart using [chart-releaser-action](https://github.com/helm/chart-releaser-action).

## License

This project is licensed under the Apache License 2.0. See [LICENSE](LICENSE) for details.
