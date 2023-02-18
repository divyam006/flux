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
--git-user=${GHUSER} `
--git-email=${GHUSER}@users.noreply.github.com `
--git-url=git@github.com:${GHUSER}/flux `
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