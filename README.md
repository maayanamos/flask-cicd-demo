# Project Overview & Usage Notes
This project demonstrates a full CI/CD pipeline for a Flask application using Docker, Kubernetes (Minikube), and GitHub Actions.

- **CI (Continuous Integration):**
Automated build of a Docker image on every push, which is then pushed to Docker Hub. This is handled entirely by GitHub Actions workflows.

- **CD (Continuous Deployment):**
Automated deployment of the Docker image to a Kubernetes cluster. The deployment uses Kubernetes manifests (k8s/deployment.yaml) which include health checks (liveness and readiness probes) for better reliability.

The CD pipeline expects a Kubernetes cluster to deploy to.
In this project, the deployment targets a *local Kubernetes cluster* using Minikube.

Since GitHub Actions runners do not have access to your local Minikube cluster, the kubectl apply command in the workflow will fail if run in GitHub Actions.
This is because the Kubernetes API server is not accessible remotely.

To see the deployment in action, you must run `kubectl apply -f k8s/deployment.yaml` locally on your machine where Minikube is running. This will apply the manifests and deploy the app.

The Docker image build and push will work seamlessly in GitHub Actions, so the CI part is fully automated and can be verified via the GitHub Actions logs and [Docker Hub repository](https://hub.docker.com/r/maayanamos/flask-cicd-demo).

To deploy and test the Flask app on your local machine:

1. Start Minikube (if not running): `minikube start`
2. Build the Docker image locally (optional if you want to test a local build): `docker build -t maayanamos/flask-cicd-demo:latest`
3. Load the Docker image into Minikube: `minikube image load maayanamos/flask-cicd-demo:latest`
4. Apply the Kubernetes manifests to deploy the app: `kubectl apply -f k8s/deployment.yaml`
5. Check the status of the deployment and pods:
`kubectl get deployments`
`kubectl get pods`
6. Access the application: `minikube service flask-cicd-service --url`
Then open the displayed URL in your browser

