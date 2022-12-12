##############################################################################################
Single Container

apiVersion: v1
kind: Pod
metadata:
  name: first-pod
spec:
  containers:
  - name: my-first-container
    image: nginx
    
    
##############################################################################################

Multicontainer


apiVersion: v1
kind: Pod
metadata:
  name: multi-container-pod
spec:
  containers:
    - name: first-container
      image: nginx
    - name: second-container
      image: ubuntu
      command:
        - /bin/bash
        - -ec
        - while :; do echo '.'; sleep 5; done
        
##############################################################################################
namespace

apiVersion: v1
kind: Namespace
metadata:
  name: dev
##############################################################################################
Pod with namespace

apiVersion: v1
kind: Pod
metadata:
  name: myapp-pod
  namespace: dev
  labels:
     app: myapp
     type: front-end
spec:
  containers:
  - name: nginx-container
    image: nginx


##############################################################################################

Deplloyment


apiVersion: apps/v1
kind: Deployment
metadata:
  name: kubeserve 
  labels:
    app: kubeserve 
spec:
  replicas : 3
  selector:
    matchLabels:
      app: kubeserve 
  template:
    metadata:
      labels:
        app: kubeserve 
    spec:
      containers:
      - name: nginx
        image: nginx
        ports:
        - containerPort: 80
        
##############################################################################################

Replicaset

    apiVersion: apps/v1
    kind: ReplicaSet
    metadata:
      name: myapp-replicaset
      labels:
        app: myapp
        type: front-end
    spec:
     template:
        metadata:
          name: myapp-pod
          labels:
            app: myapp
            type: front-end
        spec:
         containers:
         - name: nginx-container
           image: nginx
     replicas: 3
     selector:
       matchLabels:
        type: front-end

##############################################################################################

Replication Controller

    apiVersion: v1
    kind: ReplicationController
    metadata:
      name: myapp-rc
      labels:
        app: myapp
        type: front-end
    spec:
     template:
        metadata:
          name: myapp-pod
          labels:
            app: myapp
            type: front-end
        spec:
         containers:
         - name: nginx-container
           image: nginx
     replicas: 3
     
 ##############################################################################################
 
 ClusterIP
 
 apiVersion: v1
kind: Service
metadata:
 name: back-end
spec:
 types: ClusterIP
 ports:
 - targetPort: 80
   port: 80
 selector:
   app: myapp
   type: back-end
   
##############################################################################################   

NodePort


apiVersion: v1
kind: Service
metadata:
 name: myapp-service
spec:
 types: NodePort
 ports:
 - targetPort: 80
   port: 80
   nodePort: 30008
   selector:
     matchLabels:
      type: front-end
 ##############################################################################################
