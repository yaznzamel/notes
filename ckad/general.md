# This file contains general most common commands 






```
# Create port forwarding with a pod
kubectl port-forward pod/my-app-pod 8080:5000

# exec into a container ssh
kubectl exec -i -t my-pod --container main-app -- /bin/bash

# get the logs of a container inside the pod
kubectl logs POD_NAME-c CONT_NAME

```