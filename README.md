# Ethereum Node on Kubernetes

Migrating my Ethereum node stack from `docker-compose` to Kubernetes (K8s).

## Why Kubernetes over Docker Compose?
Kubernetes provides automated orchestration, self-healing capabilities, and scalability that `docker-compose` lacks. In a production environment, K8s automatically restarts crashed pods, ensuring high availability.

## Components
- **Geth Deployment**: Running Holesky testnet node.
- **Service**: ClusterIP for internal RPC communication.

## Answers to Mission Tasks
1. **In docker-compose, you use "services"; in K8s, you use "Deployments" and "Pods".**
2. **In docker-compose, port mapping uses "ports:"; in K8s, we use "Service" (ClusterIP/NodePort) to expose container ports.**
3. **If a Geth container crashes in docker-compose, it stays down. In K8s with replicas: 1, Kubernetes detects the failure and automatically recreates the Pod to restore the desired state (Self-healing).**

## Verified Setup
- Hardware: Lenovo V110 (AMD A9, 12GB RAM)
- OS: Arch Linux
- Environment: Minikube (Kubernetes v1.35.1)
