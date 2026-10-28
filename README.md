# Jenkins + Kubernetes Connection Demo

A small static website served by Nginx. The Jenkins pipeline verifies access to Docker Desktop Kubernetes, builds a local image, deploys it, and checks rollout status.

## Files
- `index.html` — website content
- `Dockerfile` — packages the website into Nginx
- `deployment.yaml` — Kubernetes Deployment
- `service.yaml` — NodePort Service on port 30081
- `Jenkinsfile` — Windows Jenkins pipeline

## Requirements
- Docker Desktop with Kubernetes enabled and running
- Jenkins running on Windows
- Docker CLI at `C:\Users\HP\AppData\Local\Programs\DockerDesktop\resources\bin\docker.exe`
- kubectl at `C:\Users\HP\AppData\Local\Programs\DockerDesktop\resources\bin\kubectl.exe`
- Kubeconfig at `C:\Users\HP\.kube\config`

Update paths in `Jenkinsfile` if your installation differs.

## Upload to GitHub
Create a new empty GitHub repository and upload the files in this folder to its root. Do not upload the ZIP as the only file.

## Configure Jenkins
Create a Pipeline job using **Pipeline script from SCM**:
- SCM: Git
- Repository URL: your new repository URL
- Branch: `*/main`
- Script Path: `Jenkinsfile`

Click **Build Now** and inspect Console Output.

## Open the website
After successful deployment, visit `http://localhost:30081`.

If it does not load, check `kubectl get pods`, `kubectl get services`, and `kubectl logs deployment/simple-web-app`.

## Note
This pipeline builds the image locally and does not push it to Docker Hub. It assumes Docker Desktop Kubernetes can access the locally built image.
