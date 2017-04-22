---
title: Web Application Development
layout: default
---
theme: Next, 3

## The Cloud
### MSSE 2017

---

# Goals
- Brief overview of cloud providers, strategies, and technologies
- Overview of deployment choices and tools
- Attempt to get our Docker image up and running in Kubernetes

---

# Lots of different cloud providers
- AWS/Google/Rackspace/Heroku
- Each have their own way of managing systems - although there is general inter lap
- Each Cloud Service Provider has many of the same options:
   - CDN
   - Datastore
   - Object store
   - Load Balancers

---

# Common concepts of distributed computing
- Front door proxy - Could be ssh termination - proxy from public url to load balancers
  - Can serve as caching layer
- Load balancers - Distribute load to back ends

---

# Service discovery
- System that responds to new services, writes all available services in a service registry
- Other services can see what is available from the registry

---

# Virtual Machines
- Virtual machines managed by a cloud provider
- Each contains the entire OS and has it's own cpu, memory, and hard disk
- Good for things that require state - Database, Cache
- Have different performance specifications and price points

---

# Containers
- Similar to Docker containers (may be docker containers)
- Run in a "cluster" of VMs which provide a pool of CPU and Memory options
  - All details are abstracted away
- Popular choices Mesos / Kubernetes

---

# Tools for building/deploying to VMs
## Spinnaker

- Global continuous platform
- Cloud agnostic
- Layer on top of Cloud
  - Can see your applications add/remove
  - Specify scaling policy
  - Terminate machines, see IPs

---

# Tools for building/deploying to VMs
## Packer

- Can create images of AWS VMs with your applications on them
- "Bake Process" - Take your application and turn it into a VM image
- In Amazon these images are referred to as AMIs (Amazon Machine Images)

---

# Workflow for Spinnaker/Packer Deployment

- Commit feature
- Versioned artifacts pushed to some repository
- Jenkins has a packer job that builds a machine image and stores to a repository
- That image can be deployed via Spinnaker

---

# Kubernetes

- Google product
- Runs in Google's Cloud platform
- Can also run your own instance - Minikube
- Could also start up instance in AWS
- Works extremely well with docker

---

# Kubectl

- Main cli for interacting with Kubernetes
- Possible to switch contexts between data-centers
- Can see pods/services/ingress
- Makes it very simple to manage all of Kubernetes

---

# Kubernetes concepts

- Namespaces to organize clusters
- Clusters are a pool of VMs with a specific CPU and Memory footprint
- "Pods" are applications that run in the cluster

---

# Pods
- Application that runs in a namespace - defined by some image
- Generally stateless
- Can be N replicas or copies
- Each Pod has a definition that defines CPU and Memory allocation
- Pods have a health check - if it does not pass Kubernetes will destroy the pod and replace with another
- Access to pods defined by a Load Balancer or Service

---

# Running our application in Kubernetes
- Need a Google Cloud account (beware, this will eventually cost money)
- Install gcloud and kubectl

- Alternative (can do the same thing with Minikube for free)

---

# Tagging  

- Creates a tag with a format that google will allow:
     - $GOOGLE_REGION/$PROJECT_NAME/$IMAGE

``` bash
docker tag ubuntu us.gcr.io/crucial-cabinet-133423/ubuntu
```

---

# Push Image

- Push the Image to Google Cloud

``` bash

gcloud docker -- push us.gcr.io/crucial-cabinet-133423/ubuntu

```

---

# Create a cluster and run

- Create a cluster
- Tell Kubernetes to deploy to the cluster
- Expose the port via LoadBalancer

- If using Google Cloud

``` bash

gcloud container clusters create example-cluster

```
- If using Mikikube

``` bash

minikube start

```

---

# Running cont.

- Same with either approach

``` bash

kubectl run msse --image=us.gcr.io/crucial-cabinet-133423/msse:latest --port=8080
kubectl expose deployment msse --type="LoadBalancer"

kubectl get services

```

---

# Running in Kubernetes
- Three main components
  - Deployment
  - Service
  - Ingress
- Can then create a deployment from anywhere, so long as Kubernetes has the proper context

---

# Deployment
- Defines what to deploy
- Can define how many pods you needs
- Can specify rollback criteria

``` yaml

apiVersion: apps/v1beta1
kind: Deployment
metadata:
  name: nginx-deployment
spec:
  replicas: 3
  template:
    metadata:
      labels:
        app: nginx
    spec:
      containers:
      - name: nginx
        image: nginx:1.7.9
        ports:
        - containerPort: 80
```

---

# Service
- Allow pods to talk to each other
- Act as a load balancer and service discovery
- Interact directly with Ingress

``` yaml

kind: Service
apiVersion: v1
metadata:
  name: my-service
spec:
  selector:
    app: MyApp
  ports:
    - protocol: TCP
      port: 80
      targetPort: 9376

```

---

# Ingress
- Entry point from web to service
- Exposes your application to the outside world

``` yaml

apiVersion: extensions/v1beta1
kind: Ingress
metadata:
  name: test-ingress
spec:
  rules:
  - http:
      paths:
      - path: /testpath
        backend:
          serviceName: test
          servicePort: 80

```

---
