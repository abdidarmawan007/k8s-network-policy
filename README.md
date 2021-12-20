# GKE Dataplane v2 network policies 
- GKE 1.21.x Dataplane v2

#### Restricting ingress traffic to the redis-cart only from the cartservice deployment with selector cartservice.
- No need Calico/Weave Net plugin

```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
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

#### More complex networkpolicy, only allow traffic egress port 443 and 80 
![alt text](https://i.imgur.com/ndzyOtx.png)
```
apiVersion: networking.k8s.io/v1
kind: NetworkPolicy
metadata:
  name: production-network-policy
  namespace: production
spec:
  podSelector: {}
  egress:
    - to:
        - namespaceSelector: {}
          podSelector:
            matchLabels:
              k8s-app: kube-dns
      ports:
        - port: 53
          protocol: UDP
    - to:
        - namespaceSelector: {}
    - to:
        - podSelector: {}
    - to:
        - ipBlock:
            cidr: 0.0.0.0/0
      ports:
        - port: 443
        - port: 80
```

#### Allow traffic egress with FQDN domain (Cillium)
![alt text](https://i.imgur.com/IjYZvuR.png)

[cillium editor](https://editor.cilium.io)
