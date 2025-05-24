# This section is meant to create a service and network management




```
Create a nodeport service

apiVersion: v1
kind: Service
metadata:
  labels:
    app: sampleService
  name: sampleservice
spec:
  ports:
  - port: 30009
    protocol: TCP
    targetPort: 8000
  selector:
    app: sampleService
  type: NodePort


```


