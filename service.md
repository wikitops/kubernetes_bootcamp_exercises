---
description: >-
  A Kubernetes service is an abstraction which defines a logical set of Pods and
  a policy by which to access them - sometimes called a micro-service.
---

# Service

## Create

What ? Why ? How ?

Explain port types :

| Type | Description |
| :--- | :--- |
| ClusterIP | Internal |
| NodePort | Host |
| LoadBalancer | External |
| Endpoint | Endpoint |

#### Exercise n°1

Expose the pod nginx in the namespace app-demo to be able to access it on HTTP in command line.

The pod has to be opened outside the cluster.

```text
kubectl create service --type=NodePort --port=80
```

#### Exercise n°2

Expose the pod nginx in the namespace app-demo to be able to access it on HTTP and HTTPS thanks to a YAML file.

```text
apiVersion: v1
kind: Service
metadata:
  name: nginx
spec:
  type: NodePort
  ports:
  - port: 80
    targetPort: 80
  - port: 443
    targetPort: 443
  selector:
    app: nginx
```

```text
kubectl apply -f nginx.yaml
```

## List

## Describe

## Delete



