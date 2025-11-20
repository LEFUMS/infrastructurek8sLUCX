# GitOps Repository for Azure Kubernetes Clusters

This repository uses FluxCD and Kustomize to manage deployments for the `servicetest` and `jotterwebsite` applications across Dev, Staging, and Prod environments.

## Structure

```
├── apps/
│   ├── base/              # Common configuration for apps
│   │   ├── servicetest/   # Helm-based application (Chart in ACR)
│   │   └── jotterwebsite/ # Manifest-based application (Image in ACR)
│   └── overlays/          # Environment variations
│       ├── dev/           # Development environment
│       ├── staging/          # Test environment
│       └── prod/          # Production environment (High Availability)
├── clusters/
│   ├── testing/           # Configuration for the Testing Cluster
│   └── official/          # Configuration for the Official Cluster
└── README.md
```

## Applications

*   **servicetest**: Deployed via `HelmRelease`. The chart is stored in an Azure Container Registry (OCI).
*   **jotterwebsite**: Deployed via standard Kubernetes manifests. The image is stored in an Azure Container Registry.

## Clusters & Environments

*   **Dev**: Deploys to `dev` namespace.
*   **Staging**: Deploys to `staging` namespace.
*   **Prod**: Deploys to `prod` namespace with increased replica count (2 replicas).

## Azure Integration

*   **Container Registry**: Images and Helm charts are pulled from `myregistry.azurecr.io`.
*   **Key Vault**: Secrets (specifically ACR credentials) are synced from Azure Key Vault using the Secrets Store CSI Driver.
    *   `SecretProviderClass` resources are defined to fetch the `acr-secret`.
    *   Pods mount the secret via CSI driver to trigger the sync, and use `imagePullSecrets` to authenticate with the registry.

## Bootstrapping

Each cluster directory (`clusters/testing`, `clusters/official`) contains a `flux-system` folder with the necessary manifests to bootstrap Flux.
