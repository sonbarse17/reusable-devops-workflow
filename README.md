# 🚀 Reusable DevOps Workflow (K8s & Python)

A high-performance, strictly-validated GitHub Actions workflow designed to build Python applications, package them into Docker images, and deploy them to Kubernetes clusters.

## 🌟 Features
*   **Branch-to-Env Mapping**: Enforces strict deployment rules:
    *   `release` branch ➔ **Production**
    *   `main` branch ➔ **UAT**
    *   `feature/*` branch ➔ **Development**
*   **Optimized Build Engine**: Uses latest stable actions with native caching for Python (`pip`) and Docker (`buildx`).
*   **Kubernetes Ready**: Supports both **Standard Manifests (kubectl)** and **Helm Charts**.
*   **Modular & Reusable**: Can be used by any repository in your organization.

## 📂 Project Structure
*   `.github/workflows/reusable-pipeline.yml`: The central engine containing all the logic.
*   `.github/workflows/main.yml`: An example caller file used for manual triggers in the UI.

## 🛠️ Setup Guide

### 1. GitHub Secrets
You must configure the following secrets in the repository where you trigger the workflow:

| Secret | Description |
| :--- | :--- |
| `KUBE_CONFIG_DATA` | Base64 encoded kubeconfig file (`cat ~/.kube/config \| base64`) |
| `REGISTRY_USERNAME` | Username for your Docker registry (e.g., DockerHub, GHCR) |
| `REGISTRY_PASSWORD` | Password or Personal Access Token for the registry |

### 2. GitHub Environments
Create three environments in **Settings > Environments**:
*   `dev`
*   `uat`
*   `prod` (Optional: Add "Required reviewers" here for extra safety)

## 📖 How to Reuse in Other Repositories
To use this workflow in a different repository, create a `.github/workflows/deploy.yml` file and use the following syntax:

```yaml
jobs:
  deploy:
    uses: <YOUR_USERNAME>/reusable-devops-workflow/.github/workflows/reusable-pipeline.yml@main
    with:
      environment: 'dev'
      image_name: 'my-app'
      registry: 'ghcr.io'
      k8s_cluster: 'my-cluster'
      k8s_namespace: 'dev-ns'
      k8s_server: 'https://k8s-api.example.com'
    secrets: inherit # This passes all secrets from the caller to the reusable workflow
```

## 🚀 Usage
1.  Navigate to the **Actions** tab in GitHub.
2.  Select **Main CI/CD Pipeline**.
3.  Click **Run workflow**.
4.  Choose your **Branch** and **Environment**.
5.  The pipeline will validate the combination and proceed with Build & Deploy.
