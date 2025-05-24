# This section delivers important files and commands to manage resource (cpu/mem) consumption


### Resource quotas

Define hard limits within total resource usage in a namespace

```
apiVersion: v1
kind: ResourceQuota
metadata:
  name: dev-quota
  namespace: dev
spec:
  hard:
    requests.cpu: "4"
    requests.memory: 8Gi
    limits.cpu: "6"
    limits.memory: 12Gi
    pods: "20"
```



### Limit ranges

Define min and max values for for resources limits/requests for a pod/container within a namespace

```
apiVersion: v1
kind: LimitRange
metadata:
  name: dev-limitrange
  namespace: dev
spec:
  limits:
  - default:
      cpu: "500m"
      memory: "512Mi"
    defaultRequest:
      cpu: "200m"
      memory: "256Mi"
    type: Container/Pod

```

