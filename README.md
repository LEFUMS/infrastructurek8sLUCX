# GitOps Repository for Azure Kubernetes Cluster

This repository uses FluxCD and Kustomize to manage deployments for the `servicetest` and `jotterwebsite` applications across Dev, Staging, and Prod environments.

## Structure

```
├── apps/
│   ├── base/              # Common configuration for apps
│   │   ├── servicetest/   # Nginx-based application
│   │   └── jotterwebsite/ # Podinfo-based application
│   └── overlays/          # Environment variations
│       ├── dev/           # Development environment
│       ├── staging/          # Test environment
│       └── prod/          # Production environment (High Availability)
├── clusters/
│   └── my-cluster/        # Flux configuration
└── README.md
```

## Applications

*   **servicetest**: A basic Nginx deployment.
*   **jotterwebsite**: A Podinfo application.

## Environments

*   **Dev**: Deploys to `dev` namespace.
*   **Staging**: Deploys to `staging` namespace.
*   **Prod**: Deploys to `prod` namespace with increased replica count (2 replicas).

## Usage

This repository is structured to be bootstrapped by Flux. The configuration in `clusters/my-cluster` sets up the synchronization.
