# Local Kubernetes Cluster with Node.js Web Application

This repository demonstrates setting up a local Kubernetes cluster using Minikube and deploying a Node.js web application. The project includes a simple Express.js server serving a static website, containerized with Docker and orchestrated using Kubernetes.

<img width="1916" height="616" alt="image" src="https://github.com/user-attachments/assets/9bfc91a1-7157-45eb-afc0-12c21e7127ab" />


<img width="1090" height="435" alt="Screenshot 2025-08-11 232817" src="https://github.com/user-attachments/assets/a9e1d3fc-0598-44b2-b895-1b1b7e05b3d2" />

## Project Overview

- Simple Node.js/Express web application
- Docker containerization
- Kubernetes deployment with replicas
- NodePort service for external access
- Health check endpoints
- Live deployment status monitoring

## Prerequisites

- Docker Desktop (latest version)
- Minikube (v1.36.0 or later)
- kubectl (latest version)
- Node.js (v18 or later, for local development)

## Setup and Installation

### 1. Install Required Tools

```powershell
# Install Chocolatey (Windows Package Manager) if not already installed
Set-ExecutionPolicy Bypass -Scope Process -Force; [System.Net.ServicePointManager]::SecurityProtocol = [System.Net.ServicePointManager]::SecurityProtocol -bor 3072; iex ((New-Object System.Net.WebClient).DownloadString('https://chocolatey.org/install.ps1'))

# Install required tools using Chocolatey
choco install minikube kubernetes-cli docker-desktop -y
```

### 2. Start Docker Desktop

Ensure Docker Desktop is running and configure it properly:
- Enable Kubernetes integration (optional)
- Allocate sufficient resources (recommended: 2 CPU, 4GB RAM)
- Wait for Docker Desktop to be in "Running" state

### 3. Configure and Start Minikube

```powershell
# Start Minikube with Docker driver
minikube start --driver=docker

# Verify cluster status
kubectl cluster-info

# Configure shell to use Minikube's Docker daemon
& minikube -p minikube docker-env --shell powershell | Invoke-Expression
```

### 4. Build and Deploy the Application

```powershell
# Build the Docker image
docker build -t web-app .

# Deploy to Kubernetes
kubectl apply -f k8s/

# Verify deployment
kubectl get deployments
kubectl get pods
kubectl get services

# Wait for pods to be ready
kubectl wait --for=condition=ready pod -l app=web
```

### 5. Access the Application

```powershell
# Get the service URL
minikube service web-service --url

# The application will be available at the provided URL
# Note: Keep the terminal open when using Docker driver on Windows
```

### 4. Access the Application

```powershell
# Get the URL to access the service
minikube service web-service --url
```

## Development Commands

### Local Development
```powershell
# Install dependencies
cd src
npm install

# Start local development server
npm start
```

### Docker Commands
```powershell
# Build image
docker build -t web-app .

# Run container locally
docker run -p 3000:3000 web-app

# List containers
docker ps
```

## Kubernetes Commands Reference

### Deployment Management
```powershell
# List all resources
kubectl get all

# Watch pod status
kubectl get pods -w

# Scale deployment
kubectl scale deployment/web-app --replicas=3

# Rollout restart
kubectl rollout restart deployment/web-app
```

### Debugging
```powershell
# Get pod logs
kubectl logs <pod-name>

# Describe resources
kubectl describe pod <pod-name>
kubectl describe service web-service

# Port forward to pod
kubectl port-forward <pod-name> 3000:3000
```

### Service Management
```powershell
# List services
kubectl get services

# Get service details
kubectl describe service web-service

# Get service URL
minikube service web-service --url
```

## Project Structure

```
.
├── src/                  # Source code directory
│   ├── public/           # Static files directory
│   │   └── index.html    # Main webpage
│   ├── package.json      # Node.js dependencies and scripts
│   └── server.js         # Express server implementation
├── k8s/                  # Kubernetes configuration directory
│   ├── deployment.yaml   # Deployment configuration
│   └── service.yaml      # Service configuration
├── Dockerfile            # Docker container configuration
└── README.md            # Project documentation
```

## Application Components

### Web Application
- Express.js server
- Static file serving
- Health check endpoint (/health)
- Responsive web interface

### Docker Container
- Base: Node.js 18 Alpine
- Optimized for small size and security
- Exposed port: 3000

### Kubernetes Configuration
- Deployment with 2 replicas
- NodePort service type
- Health and readiness probes
- Container port: 3000, Service port: 80

## Cleanup

To clean up and delete all resources:

```powershell
# Delete all Kubernetes resources
kubectl delete -f k8s/

# Stop Minikube
minikube stop

# Delete Minikube cluster (optional)
minikube delete

# Reset Docker context (if needed)
docker context use default
```

## Troubleshooting

### Common Issues

1. **Image Pull Error**
   ```powershell
   # Ensure you're using Minikube's Docker daemon
   & minikube -p minikube docker-env --shell powershell | Invoke-Expression
   # Rebuild the image
   docker build -t web-app .
   ```

2. **Service Not Accessible**
   - Keep terminal open when using Docker driver on Windows
   - Use `minikube service web-service --url` to get the correct URL
   - Check service status with `kubectl describe service web-service`

3. **Pod Startup Issues**
   ```powershell
   # Check pod status
   kubectl describe pod <pod-name>
   # Check logs
   kubectl logs <pod-name>
   ```

## Additional Resources

- [Minikube Documentation](https://minikube.sigs.k8s.io/docs/)
- [Kubernetes Documentation](https://kubernetes.io/docs/home/)
- [Express.js Documentation](https://expressjs.com/)
- [Docker Documentation](https://docs.docker.com/)
