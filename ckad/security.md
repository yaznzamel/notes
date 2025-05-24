# This file is for security related configs




### Run a pod with certain capabilities

```
# Create a pod with SYS TIME capabilities

apiVersion: v1
kind: Pod
metadata:
    app: webapp

spec:
    containers:
        - name: webapp
          image: webapp
          securityContext:
            capabilities:
                add: ["SYS_TIME"]
```


#### Create service accounts

```
# Get service accounts 
kubectl get serviceaccounts

# check the token inside the pod
cat /var/run/secrets/kubernetes.io/serviceaccount/token

# create a token for a service account k8.1.24
kubectl create token dashboard-sa

# create a temp token
    kubectl create token fapp-sa --duration=10m --namespace=default

# set a service account for a current deployment
kubectl set serviceaccount deployment/web-dashboard dashboard-sa
```