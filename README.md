# GKE Dataplane v2 network policies 
- GKE 1.21.x Dataplane v2

#### Restricting ingress traffic to the redis-cart only from the cartservice deployment with selector cartservice.
- Really helps prevent unnecessary services connecting to redis server and reduces tcp open connections on redis server

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
