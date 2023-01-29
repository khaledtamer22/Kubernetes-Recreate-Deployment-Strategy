# What is KUBERNETES ?

Kubernetes is an open-source container orchestration system for automating software deployment, scaling, and management. Originally designed by Google, the project is now maintained by the Cloud Native Computing Foundation. The name Kubernetes originates from Greek, meaning helmsman or pilot. Wikipedia

# What will we create ?
In this example we will create a NodeJs application and dockerize it with `Docker` then we will deploy it with `kubernetes` on our local machine

# Let's start :

# 1- Create a node js application

Open you cmd in the project path first, then start execting the commands
- Creat package.json file
`npm init -y`

- Intall express framework
`npm install express`

- Creat a file `index.mjs`

- we will write a code to show the pod that the application is running on in the `index.mjs` file

```
import express from 'express'
import os from 'os'

const app = express()
const PORT = 3000

app.get("/" , (req,res)=>{
    const message= `Hello world I am POD ${os.hostname}` 
res.send(message)
})

app.listen(PORT, ()=> {
    console.log('Web server is listening at port ${PORT}')
    console.log(os.hostname)
})
```

- Run the application

`node index.mjs`

# 2- Dockerize the application and make an image for it locally

- Create the docker file and name it Dockerfile

- Add your Dockerfile code
```
FROM node:alpine

WORKDIR /app

EXPOSE 3000

COPY package.json package-lock.json ./

RUN npm install

COPY . ./
```

- Build the image

`docker build . -t yourDockerHubAccountName/repositoryName`

`docker build . -t khaledeltaweel/nodejsapp`

# 3- Push the local image to your dockerhub repository

login to dokcer account

`docker login`

- push the image

`docker push yourDockerHubAccountName/repositoryName`

`docker push khaledeltaweel/nodejsapp`

# 4- Create a deployment from our online image and create a service

- create a deployment

`kubectl create deployment deploymentName --image=imageSoureceFromDockerHub`

`kubectl create deployment nodejsapp --image=khaledeltaweel/nodejsapp`

- Create a service

`kubectl expose deployment nodejsapp --type=NodePort --port=3000`

# 5- Connect to the application

- run the command to open browser and run the application

`minikube service appName`

`minikube service nodejsapp`

The web browser will open and will run the code in the index.mjs file and the pod name will show up

The application url will look like this `172.26.177.67:30374`