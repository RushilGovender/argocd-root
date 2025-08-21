# Argo CD App-of-Apps (ApplicationSet + Helm) — Rancher Desktop/k3s

This repository bootstraps apps in Argo CD using **ApplicationSet** that generates one **Application** per Helm chart.
- **Repo URL:** https://github.com/RushilGovender/argocd-root.git
- **Branch:** main
- **Namespaces:** match app names (e.g., `nginx` -> namespace `nginx`)

## Structure
```
environments/
  dev/
    root-app.yaml        # ApplicationSet with a list of Helm charts
```

## Quick Start
1) Push this repo to your Git host at `https://github.com/RushilGovender/argocd-root.git` (branch `main`).  
2) Install Argo CD in your Rancher Desktop cluster (k3s) if not already:
```bash
kubectl create namespace argocd || true
kubectl apply -n argocd -f https://raw.githubusercontent.com/argoproj/argo-cd/stable/manifests/install.yaml
```
3) Apply the ApplicationSet root:
```bash
kubectl apply -f environments/dev/root-app.yaml
```
4) Open Argo CD UI and watch apps sync:
- `nginx`, `grafana`, `prometheus` (namespaces created automatically)

## Add more Helm apps
Edit `environments/dev/root-app.yaml` and append to the `elements:` list with the following fields:
- `name`: app/namespace name
- `repoURL`: Helm chart repository URL
- `chart`: chart name
- `version`: chart version (SemVer or exact)

> Values: Start simple. You can later add `helm: values` snippets per-app (inline) or migrate to a Kustomize overlay per app if you need complex values.
