# ArgoCD Repository for Kubernetes Cluster

This repository contains the GitOps configurations for deploying ArgoCD itself as well as Cilium CNI to a Talos-based Kubernetes cluster. The configurations are compatible with the setup created by the `terraform-proxmox-talos-k8s` project.

## Structure

```
argocd_repo/
├── argocd/                      # ArgoCD deployment resources
│   ├── namespace.yaml           # ArgoCD namespace definition
│   ├── helm-repository.yaml     # Helm repository for ArgoCD charts
│   ├── helm-release.yaml        # ArgoCD Helm release definition
│   ├── argocd-values.yaml       # Custom values for ArgoCD
│   └── kustomization.yaml       # Kustomization for ArgoCD resources
├── apps/                        # Applications managed by ArgoCD
│   └── infrastructure/          # Infrastructure components
│       └── cilium/              # Cilium CNI configuration
│           ├── application.yaml # Cilium Application definition
│           └── kustomization.yaml
└── kustomization.yaml           # Root kustomization file
```

## Components

### ArgoCD

The ArgoCD configuration includes:

- ArgoCD version: v2.9.3
- Helm chart version: 5.51.4
- High availability setup with Redis HA
- Resource limits and requests configured
- Enhanced security settings

### Cilium

The Cilium CNI configuration includes:

- Cilium version: 1.15.6 (matching the `terraform-proxmox-talos-k8s` project)
- Configured for Talos Linux with kube-proxy replacement enabled
- Hubble enabled for network visibility
- Prometheus metrics enabled

## Usage

This repository is designed to be used with Flux CD to deploy ArgoCD to your Kubernetes cluster. Once ArgoCD is deployed, you can use it as your primary GitOps tool for managing applications on your cluster.

### Accessing ArgoCD UI

After deployment, you can access the ArgoCD UI by port-forwarding the ArgoCD server service:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:80
```

Then access the UI at http://localhost:8080.

### Initial Admin Password

The initial admin password is auto-generated and stored in a Kubernetes secret. Retrieve it with:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

It's recommended to change this password after the first login.
