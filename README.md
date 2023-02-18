# Flux Getting Started Guide

# 1 - Kubernetes

Get a Kubernetes Cluster. In this video, I use Docker for Windows.
If you are new to Kubernetes, checkout my videos [here](https://marceldempers.dev/videos/guides/kubernetes-getting-started)

# 2 - Flux CTL

I used Flux 1.18 which I got from the GitHub [Release Page](https://github.com/fluxcd/flux/releases/tag/1.18.0)
Rename it to `fluxctl.exe` & place it in a folder that is on your `$env:Path` environment variable.
Open a new terminal and try
```
fluxctl
```

# 4 - Installing Flux

Make sure you are pointing to the kubernetes cluster you want to use
```
kubectl config current-context
kubectl get nodes
```
```
kubectl create ns flux

$GHUSER = "divyam006"
fluxctl install `
--git-url=git@github.com:divyam006/flux `
--git-path=Yamls `
--git-branch=main `
--namespace=flux | kubectl apply -f -

kubectl -n flux rollout status deployment/flux

$env:FLUX_FORWARD_NAMESPACE = "flux"
fluxctl list-workloads
fluxctl identity

fluxctl sync

  annotations:
    fluxcd.io/tag.example-app: semver:~1.0
    fluxcd.io/automated: 'true'

fluxctl policy -w default:deployment/example-deploy --tag "example-app=1.0.*"
```




===================
Examples:
  # Check prerequisites
  flux check --pre

  # Install the latest version of Flux
  flux install

  # Create a source for a public Git repository
  flux create source git nginx-latest `
   --url=https://github.com/divyam006/flux `
   --branch=main `
   --interval=3m

  # List GitRepository sources and their status
  flux get sources git

  # Trigger a GitRepository source reconciliation
  flux reconcile source git flux-system

  # Export GitRepository sources in YAML format
  flux export source git --all > sources.yaml

  # Create a Kustomization for deploying a series of microservices
  flux create kustomization webapp-dev \
    --source=webapp-latest \
    --path="./deploy/webapp/" \
    --prune=true \
    --interval=5m \
    --health-check="Deployment/backend.webapp" \
    --health-check="Deployment/frontend.webapp" \
    --health-check-timeout=2m

  # Trigger a git sync of the Kustomization's source and apply changes
  flux reconcile kustomization webapp-dev --with-source

  # Suspend a Kustomization reconciliation
  flux suspend kustomization webapp-dev

  # Export Kustomizations in YAML format
  flux export kustomization --all > kustomizations.yaml

  # Resume a Kustomization reconciliation
  flux resume kustomization webapp-dev

  # Delete a Kustomization
  flux delete kustomization webapp-dev

  # Delete a GitRepository source
  flux delete source git webapp-latest

  # Uninstall Flux and delete CRDs
  flux uninstall


  --------------
  https://fluxcd.io/flux/installation/

kubectl delete --all daemonsets,replicasets,services,deployments,pods,rc,ingress -n argocd