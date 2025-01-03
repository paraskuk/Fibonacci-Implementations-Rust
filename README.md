[![Rust](https://github.com/paraskuk/Fibonacci-Rust-Kubernetes-Deployment-Command-Line-POC-App/actions/workflows/rust.yml/badge.svg)](https://github.com/paraskuk/Fibonacci-Rust-Kubernetes-Deployment-Command-Line-POC-App/actions/workflows/rust.yml)
![Rust_programming_language_black_logo svg](https://github.com/user-attachments/assets/ab07a1c3-fdf7-4a45-be0e-977184cb8914)

# Rust App that calculates the Fibonacci sequence and deploys it to Kubernetes


This project provides various implementations of the Fibonacci sequence in Rust, including recursive, memoized, iterative, and dynamic programming approaches. 
It also includes a Kubernetes deployment using Helm charts using minikube.

> [!TIP] 
> The Fibonacci sequence is a series of numbers in which each number is the sum of the two preceding ones, usually starting with 0 and 1.

> [!TIP]
> The sequence starts: 0, 1, 1, 2, 3, 5, 8, 13, 21, 34, ...

> [!TIP]
> The Fibonacci sequence is defined by the recurrence relation: F(n) = F(n-1) + F(n-2) with base cases F(0) = 0 and F(1) = 1.

> [!TIP]
> The Fibonacci sequence grows exponentially, so the recursive implementation is not efficient for large values of n.

> [!TIP]
> The iterative and dynamic programming implementations are more efficient for large values of n.

> [!TIP]
> The memoized recursive implementation is more efficient than the simple recursive implementation because it avoids redundant calculations.

> [!TIP]
> The pattern matching implementation is similar to the simple recursive implementation but uses pattern matching instead of if-else statements.

> [!TIP]
> The dynamic programming implementation uses an array to store the Fibonacci numbers and avoids redundant calculations.

> [!TIP]
> The following implementations are provided:

- `fibonacci`: A simple recursive implementation.
- `fibonacci_match`: A recursive implementation using pattern matching.
- `fibonacci_dp`: An implementation using dynamic programming.
- `fibonacci_memo`: A memoized recursive implementation.
- `fibonacci_iterative`: An iterative implementation.


## Prerequisites
> [!IMPORTANT]
> Before you begin, ensure you have the following tools installed:
> 1. **Rust & Cargo** (for local builds/verification, optional if you only build in Docker).
> 2. **Docker** (to build and push container images).
> 3. **Helm** (to manage Kubernetes deployments).
> 4. **Minikube or a Kubernetes Cluster** (for testing).
> 5. **Docker Hub Account** (to push container images).

---

## 1. Building the Rust Application Locally (Optional)

You can verify that the application compiles and runs locally:
> [!TIP]
> You can build the Rust application using Cargo:
```sh
cargo run --package fibonacci --bin fibonacci -- 10

# Output:
Time taken by fibonacci_match: ...
Time taken by fibonacci_memo: ...
Time taken by fibonacci_iterative: ...
```

## 2. Building the Docker Image
> [!TIP]
> You can build the Docker image using the provided Dockerfile
> Tag the image after its building
> Using your username login to Docker Hub and push the image to Docker Hub.
```sh
docker build -t fibonacci_rust:latest .
docker tag fibonacci_rust:latest ${DOCKER_USERNAME}/fibonacci_rust:latest
docker login --username ${DOCKER_USERNAME}
docker push ${DOCKER_USERNAME}/fibonacci_rust:latest
docker tag fibonacci_rust:latest ${DOCKER_USERNAME}/fibonacci_rust:v2
docker push ${DOCKER_USERNAME}/fibonacci_rust:v2
```

## 3. Deploying the Application to Kubernetes
### 3.0 Deploy using Manifests
> [!TIP]
> You can deploy the application to Kubernetes using the provided deployment.yaml file:
```sh
kubectl apply -f deployment.yaml
```

### 3.1 Or Deploy via Helm Charts - Helm Chart Overview
> [!TIP]
> This repository includes a simple Helm chart (in the fibonacci/ directory, for example) that deploys the fibonacci container into Kubernetes. The key files are:

* Chart.yaml – Chart metadata.
* values.yaml – Default values (e.g., image, replicas, etc.).
* templates/ – Contains Kubernetes manifests (Deployment, Service, etc.).

### 3.2 Creating Secret
> [!TIP]
> Create a secret to pull the image from Docker Hub:
```sh
kubectl create secret docker-registry regcred \
--docker-server=docker.io \
--docker-username=myusername \
--docker-password=MY_PERSONAL_ACCESS_TOKEN \
--docker-email=myemail@example.com
```

Then reference it in your values.yaml or deployment.yaml:
> [!TIP]
> Add the following to your deployment.yaml file:
```yaml
imagePullSecrets:
- name: regcred
```

### 3.3 Install Helm Chart
> [!TIP]
> Navigate to the fibonacci/ directory and install the Helm chart for the first time:
```sh
helm install fibonacci .
```
> [!TIP]
> If you already have the chart installed and want to upgrade it:

```sh
helm upgrade --install fibonacci .
```
> [!TIP]
> Check the deployment status:

```sh
helm status fibonacci
kubectl get pods
```
### 3.4 Accessing the Application
This program just prints to the STDOUT
> [!TIP]
> You can access the logs of the fibonacci-deployment pod to see the output.

```sh
kubectl logs deployment/fibonacci-deployment
```

### 4 .Cleaning Up
> [!WARNING]
> This will uninstall helm release.

```sh
helm uninstall fibonacci
```

> [!CAUTION]
> This will delete the deployment and service and the minikube cluster.

```sh
kubectl delete secret regcred
minkube delete
```
