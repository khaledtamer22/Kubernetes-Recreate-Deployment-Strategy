# What's the recreate deployment strategy in kubernetes?

A recreate deployment strategy is an all-or-nothing process that lets you update an application immediately, with some downtime. 

With this strategy, existing pods from the deployment are terminated and replaced with a new version. This means the application experiences downtime from the time the old version goes down until the new pods start successfully and start serving user requests. 

# In this example we will implement the recreate deployment on a node js application

# Let's start

# 1- Create `deployment.yml` file and make sure to define your `strategy type` to be `Recreate` 

```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: recreate-deployment
  labels:
    app: helloserver
spec:
  replicas: 8
  selector:
    matchLabels:
      app: helloserver
  strategy:
    type: Recreate
  template:
    metadata:
      labels:
        app: helloserver
    spec:
      containers:
      - name: helloserver
        image: khaledeltaweel/k8s-web-server
        # image: khaledeltaweel/k8s-web-server:1.0
        ports:
        - containerPort: 3000
```

# 2- Create  `service.yml` file

```
apiVersion: v1
kind: Service
metadata:
  name: recreate-service
spec:
  selector:
    app: helloserver
  ports:
    - port: 80
      targetPort: 3000
  type: NodePort
```

# Now we have created a normal application and you can run the application using the command

`minikube service recreate-service`

The web browser will open and the application  will be running on ip looks like this ip `172.26.177.67:30374`

# 3- Start the recreate deployment strategy

We used the image `khaledeltaweel/k8s-web-server` in the `deployment.yml` file, so we will change the image version to be `khaledeltaweel/k8s-web-server:1.0` and then run the deployment again using the command

`kubectl apply -f deployment.yml`

# What will happen when we run this above command?

- All pods will be terminated together
- All new pods will be recreated
- All pods are running

# you can see the pods status after running the deployment again using the command `kubectl get po`

![a65f3d26-9b28-42fc-a23f-28b82fd5723f](https://user-images.githubusercontent.com/35044692/215352562-e9a9d2c3-c0b4-4c95-a537-21a68a450197.jpg)
