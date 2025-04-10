# ArgoCD Repository Installation Guide

This guide provides instructions for deploying ArgoCD to your Talos-based Kubernetes cluster and using it to deploy Cilium CNI.

## Prerequisites

- A working Talos-based Kubernetes cluster (such as one created by the terraform-proxmox-talos-k8s project)
- `kubectl` installed and configured to access your cluster

## Installing ArgoCD

1. Clone this repository:

```bash
git clone https://github.com/your-username/argocd_repo.git
cd argocd_repo
```

2. Apply the ArgoCD installation manifests:

```bash
kubectl apply -k argocd/
```

This will create the ArgoCD namespace and deploy ArgoCD using the Helm chart configuration.

3. After ArgoCD is deployed, apply the application configurations:

```bash
kubectl apply -k apps/
```

This will configure ArgoCD to deploy Cilium as the CNI for your Talos cluster.

## Verifying the Installation

Check the ArgoCD installation status:

```bash
kubectl get pods -n argocd
```

## Accessing the ArgoCD UI

After deployment, you can access the ArgoCD UI by port-forwarding:

```bash
kubectl port-forward svc/argocd-server -n argocd 8080:80
```

Then access the UI at http://localhost:8080

## Initial Admin Password

Retrieve the initial admin password:

```bash
kubectl -n argocd get secret argocd-initial-admin-secret -o jsonpath="{.data.password}" | base64 -d
```

## Troubleshooting

If you encounter issues with the ArgoCD deployment, check the FluxCD logs:

```bash
flux logs -f
```

And the ArgoCD logs:

```bash
kubectl logs -n argocd -l app.kubernetes.io/name=argocd-server
```
