---
title: "Kubernetes Networking: Services, Ingress, and Network Policies"
categories:
  - DevOps
  - Kubernetes
tags:
  - kubernetes
  - networking
  - ingress
  - services
date: 2024-11-10
---

Understanding Kubernetes networking is key to running reliable workloads. This post covers the three core abstractions: Services, Ingress, and Network Policies.

## Services — Stable Endpoints for Pods

Pods are ephemeral — they come and go, and their IPs change. **Services** provide a stable IP and DNS name:

```yaml
apiVersion: v1
kind: Service
metadata:
  name: my-service
spec:
  selector:
    app: my-app
  ports:
    - port: 80
      targetPort: 8080
  type: ClusterIP
```

### Service Types

| Type | Use Case |
|------|----------|
| **ClusterIP** | Internal communication only |
| **NodePort** | Expose on each node's IP at a static port |
| **LoadBalancer** | Cloud load balancer integration |

## Ingress — HTTP Routing

Services handle L3/L4. For HTTP routing (host-based, path-based, TLS termination), use **Ingress**:

```yaml
apiVersion: networking.k8s.io/v1
kind: Ingress
metadata:
  name: my-ingress
spec:
  rules:
    - host: api.example.com
      http:
        paths:
          - path: /v1
            pathType: Prefix
            backend:
              service:
                name: api-v1
                port:
                  number: 80
```

Pair with an **Ingress Controller** (nginx-ingress, Traefik, Cilium) to actually process these rules.

## Network Policies — Firewall for Pods

By default, all pods can talk to each other. Network Policies restrict traffic:

```yaml
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: deny-from-other-namespaces
spec:
  podSelector:
    matchLabels:
      app: backend
  ingress:
    - from:
        - namespaceSelector:
            matchLabels:
              name: frontend
```

## Key Takeaways

1. **Services** give pods stable addresses — use ClusterIP internally, Ingress for external HTTP
2. **Ingress** handles L7 routing, TLS, and virtual hosting
3. **Network Policies** are your firewall — start with deny-all and whitelist what you need
4. Your CNI plugin (Calico, Cilium, Weave) must support NetworkPolicy enforcement
