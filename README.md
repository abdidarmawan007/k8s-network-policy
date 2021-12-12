# Kubernetes Network Policies
- GKE 1.21.x Dataplane v2

### Restricting ingress traffic to the redis-cart Pod only from the cartservice Pods.

```
kind: NetworkPolicy
apiVersion: networking.k8s.io/v1
metadata:
  namespace: production
  name: redis-cart-allow-from-cartservice
spec:
  policyTypes:
  - Ingress
  podSelector:
    matchLabels:
      app: redis-cart
  ingress:
  - from:
    - podSelector:
        matchLabels:
          app: cartservice
```
