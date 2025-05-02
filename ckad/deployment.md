# This file is for deployment related files and commands





### Create a simple deployment
```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: test
  name: test
spec:
  replicas: 1
  selector:
    matchLabels:
      app: test
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: test
    spec:
      containers:
      - image: nginx
        name: nginx
        resources: {}
status: {}

```


### Create deployment with secret

```
apiVersion: apps/v1
kind: Deployment
metadata:
  creationTimestamp: null
  labels:
    app: test
  name: test
spec:
  replicas: 1
  selector:
    matchLabels:
      app: test
  strategy: {}
  template:
    metadata:
      creationTimestamp: null
      labels:
        app: test
    spec:
      containers:
      - image: nginx
        name: nginx
        env:
         - name: SECRET_IN_CONTAINER
           valueFrom:
            secretKeyRef:
                name: NAME_OF_K8_SECRET
                key: NAME_OF_KEY_K8_SECRET
        resources: {}
status: {}

```



### Create deployment with security context

```

```