# Kubernetes Manifests

Production-grade Kubernetes manifests demonstrating real-world deployment patterns for a containerized Nginx application.

## Architecture

```
Ingress (TLS) → Service (ClusterIP) → Deployment (nginx:1.25.3)
                                             ↑
                            ConfigMap (env vars) + Secret (credentials)
                                             ↑
                            HPA (CPU 70% / Memory 80%) → min:2, max:5
                            PDB (minAvailable: 1)
                            NetworkPolicy (restricted ingress/egress)
```

## Files

| File | Purpose |
|------|---------|
| `namespace.yaml` | Isolated devops namespace |
| `deployment.yaml` | Deployment with probes, rolling update, resource limits |
| `service.yaml` | ClusterIP service |
| `configmap.yaml` | Non-sensitive environment config |
| `secret.yaml` | Base64-encoded credentials |
| `ingress.yaml` | NGINX Ingress with TLS and annotations |
| `hpa.yaml` | Autoscaling on CPU + Memory metrics |
| `pdb.yaml` | Ensures minimum pod availability during disruptions |
| `networkpolicy.yaml` | Restricts pod-level traffic |

## Key Concepts Demonstrated

- Rolling update strategy with zero downtime (`maxUnavailable: 0`)
- Liveness and Readiness probes for self-healing
- Resource requests and limits for QoS
- ConfigMap and Secret injection via environment variables
- HPA with CPU + Memory metrics and scale-down stabilization
- PodDisruptionBudget for safe cluster maintenance
- NetworkPolicy for least-privilege pod communication
- TLS termination at Ingress layer

## Deploy

```bash
kubectl apply -f namespace.yaml
kubectl apply -f configmap.yaml
kubectl apply -f secret.yaml
kubectl apply -f deployment.yaml
kubectl apply -f service.yaml
kubectl apply -f ingress.yaml
kubectl apply -f hpa.yaml
kubectl apply -f pdb.yaml
kubectl apply -f networkpolicy.yaml
```

## Verify

```bash
# Check all resources
kubectl get all -n devops

# Check HPA status
kubectl get hpa -n devops

# Check pod health
kubectl describe pod -n devops

# Check ingress
kubectl get ingress -n devops
```
