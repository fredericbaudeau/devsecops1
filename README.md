
# DevSecOps - Kubernetes DevOps & Security

### Fork and Clone this Repo

### Clone to Desktop and VM

### NodeJS Microservice - Docker Image -

`docker run -p 5000:5000 hrefnhaila/node-service:v1`

`curl localhost:5000/plusone/99`
 
### NodeJS Microservice - Kubernetes Deployment -
`kubectl create deploy node-app --image hrefnhaila/node-service:v1`

`kubectl expose deploy node-app --name node-service --port 5000 --type ClusterIP`

`curl node-service-ip:5000/plusone/99`

//###\\ Test frédéric //###\\
