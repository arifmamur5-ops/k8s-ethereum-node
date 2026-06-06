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

# Kubernetes Ethereum Node (Production-Grade)

This repository contains production-ready Kubernetes (K8s) manifests to deploy and manage a Go-Ethereum (Geth) Execution Client on the Holesky testnet. It follows industry best practices for stateful applications by decoupling configurations, securing secrets, and ensuring data persistence.

##Architecture Components

The deployment leverages three core pillars of Kubernetes resource management:
1. **`ConfigMap` (`geth-config.yaml`)** - Stores non-sensitive runtime configurations such as the `--holesky` flag, `syncmode: snap`, and required HTTP/Engine API modules.
2. **`Secret` (`jwt-secret`)** - Safely injects the Engine API authentication token (`jwt.hex`) into the pod environment using native K8s Secret objects instead of hardcoding credentials.
3. **`PersistentVolumeClaim` (`geth-pvc.yaml`)** - Provisions 10Gi of persistent storage to ensure the blockchain sync data remains intact even if the Geth pod crashes or restarts.

## Quick Start

```bash
# 1. Start your local cluster (Minikube)
minikube start --driver=docker

# 2. Create the K8s Secret from your local JWT file
kubectl create secret generic jwt-secret --from-literal=jwt=$(cat ~/projects/web3-node-journey/jwt.hex)

# 3. Apply the configuration and storage manifests
kubectl apply -f geth-config.yaml
kubectl apply -f geth-pvc.yaml

# 4. Deploy the production Geth node
kubectl apply -f geth-deployment-v2.yaml

## Verified Setup
- Hardware: Lenovo V110 (AMD A9, 12GB RAM)
- OS: Arch Linux
- Environment: Minikube (Kubernetes v1.35.1)
