# nginx-pod.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod
  labels:
    app: nginx
    tier: dev
spec:
  containers:
  - name: nginx-container
    image: nginx


# Create and display PODs
kubectl create -f nginx-pod.yaml
kubectl get pod
kubectl get pod -o wide
kubectl get pod nginx-pod -o yaml
kubectl describe pod nginx-pod


ConfigMaps
A ConfigMap is an API object used to store non-confidential data in key-value pairs. Pods can consume ConfigMaps as environment variables,
command-line arguments, or as configuration files in a volume.

#nginx-pod-configmap-vol.yaml
apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod-configmap-vol
spec:
  containers:
  - name: nginx-container
    image: nginx
    volumeMounts:
    - name: test-vol
      mountPath: "/etc/non-sensitive-data"
      readOnly: true
  volumes:
    - name: test-vol
      configMap:
        name: nginx-configmap-vol
        items:
        - key: file-1.txt
          path: file-a.txt
        - key: file-2.txt
          path: file-b.txt
          
# Create
kubectl create -f nginx-pod-configmap-vol.yaml

# Display
kubectl get po
kubectl get configmaps
kubectl describe pod nginx-pod-configmap-vol


Secrets
A Secret is an object that contains a small amount of sensitive data such as a password, a token, or a key. 
Such information might otherwise be put in a Pod specification or in a container image. 
Using a Secret means that you don't need to include confidential data in your application code.


apiVersion: v1
kind: Pod
metadata:
  name: nginx-pod-secret-vol
spec:
  containers:
  - name: nginx-container
    image: nginx
    volumeMounts:
    - name: test-vol
      mountPath: "/etc/confidential"
      readOnly: true
  volumes:
  - name: test-vol
    secret:
      secretName: nginx-secret-vol
      
# Create
kubectl create -f nginx-pod-secret-vol.yaml

# Display
kubectl get po
kubectl get secrets
kubectl describe pod nginx-pod-secret-vol
