//
This file provides step-by-step instructions for setting up and running a sample Kubernetes-based application using Minikube, Docker, and kubectl. It walks you through:

Pre-requisites – Ensuring the necessary tools are installed.
Installation – Installing Docker, kubectl, and Minikube on Linux and Windows.
Starting Minikube – Verifying its status.
Deploying an Application – Creating an Nginx deployment and checking its status.
Managing Deployments – Editing the deployment, rolling back changes, and debugging issues.
Self-Healing Feature – Demonstrating how Kubernetes automatically recreates deleted pods.
Port Forwarding – Exposing and accessing the application on localhost.
Cleanup – Deleting resources and stopping Minikube.
//

# Running Sample App

## Pre-requisites

Ensure you have a stable internet connection and Admin/Root Access:

- **Linux Users:** Open Terminal.
- **Windows Users:** Open Command Prompt as Administrator.
- **Ensure Docker is installed and running before proceeding.**

## Installation and Setup

### Step 1: Install Docker

#### Linux (Ubuntu):

```bash
sudo apt update
sudo apt install docker.io
sudo service docker start
docker run hello-world
```

**Expected Output:** "Hello from Docker!"

#### Windows:

1. Download and install Docker Desktop from Docker's official site.
2. Open Docker Desktop → Settings → Enable Kubernetes → Apply & Restart.
3. Verify Docker installation:

```bash
docker run hello-world
```

### Step 2: Install kubectl

#### Linux (Ubuntu):

```bash
sudo apt-get install -y apt-transport-https ca-certificates curl
sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://pkgs.k8s.io/core:/stable:/v1.29/deb/Release.key
echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /" | sudo tee /etc/apt/sources.list.d/kubernetes.list
sudo apt-get update
sudo apt-get install -y kubectl
```

#### Windows:

```bash
winget install kubernetes.kubectl
```

Verify installation:

```bash
kubectl version --client
```

### Step 3: Install Minikube

#### Linux (Ubuntu):

```bash
curl -LO https://storage.googleapis.com/minikube/releases/latest/minikube-linux-amd64
sudo install minikube-linux-amd64 /usr/local/bin/minikube
```

#### Windows:

```bash
winget install minikube
```

### Step 4: Start Minikube

```bash
minikube start
```

Verify Minikube is running:

```bash
minikube status
```

## Deploying an Application

### Step 1: Check Cluster Status

```bash
kubectl get nodes
kubectl get pods
kubectl get services
```

### Step 2: Create a Deployment

```bash
kubectl create deployment <your_name> --image=nginx
```

Verify deployment:

```bash
kubectl get deployment
kubectl get pods
kubectl get events
kubectl config view
kubectl logs <pod-name>
```

### Step 3: Edit Deployment

```bash
kubectl edit deployment <your_name>
```

Change the image to:

```yaml
image: nginx:1.16
```

Save and close the file. Verify the changes:

```bash
kubectl get deployment
```

### Step 4: Rollback Deployment

```bash
kubectl rollout undo deployment <your_name>
kubectl get deployment
```

## Debugging Pods

### Step 1: View Logs

```bash
kubectl logs <pod-name>
```

### Step 2: Describe Pod

```bash
kubectl describe pod <pod-name>
```

### Step 3: Delete Deployment

```bash
kubectl delete deployment <your_name>
```

## Self-Healing Feature

1. Create a deployment again if deleted:

```bash
kubectl create deployment <your_name> --image=nginx
```

2. Get the pod name and delete it:

```bash
kubectl get pods
kubectl delete pod <pod-name>
```

3. A new pod should be created automatically:

```bash
kubectl get pods
```

## Port Forwarding

1. Expose deployment as a service:

```bash
kubectl expose deployment <your_name> --type=ClusterIP --name=nginx-service-<your_name> --port=8080 --target-port=80
```

2. Forward port 8080:

```bash
kubectl port-forward service/nginx-service-<your_name> 8080:8080
```

3. Open a browser and visit:

```
http://localhost:8080
```

You should see the Nginx welcome page.

## Cleanup

1. Delete deployment and service:

```bash
kubectl delete deployment <your_name>
kubectl delete service nginx-service-<your_name>
```

2. Stop Minikube:

```bash
minikube stop
```

The application should now be successfully set up and tested!

