# Kubernetes-Practice

🎉🎉🎉  Kubernetes Study and Practice 🎉🎉🎉 

## Table of Contents

  - [Pod](#Pod)
  - [Service](#Service)
  - [Ingress](#Ingress)
  - [Volume-PV,PVC](#volume-pvpvc)
  - [ConfigMap](#ConfigMap)
  - [Secret](#Secret)
  - ...
  
## Pod

## Service

## Ingress
![alt text](https://github.com/luisxiaomai/Images/blob/master/Kubernetes-Practice/ingress-flow.png)

### Deploy Ingress Controller
```bash	
kubectl create -f ingress/ingress-nginx-deploy.yaml
```

### Deploy Tomcat Deployment and Service
```bash	
kubectl create -f ingress/tomcat-deployment-service.yaml
```

### Deploy Http Ingress
```bash	
kubectl create -f ingress/tomcat-ingress.yaml
```
![alt text](https://github.com/luisxiaomai/Images/blob/master/Kubernetes-Practice/ingress-http.png)

### Deploy Https Ingress

```bash
# create self certificate
openssl genrsa -out tls.key 2048
openssl req -new -x509 -key tls.key -out tls.crt -subj /C=CN/ST=GuangDong/L=GuangZhou/O=DevOps/CN=tomcat.linux.io -days 3650
# create k8s secret
kubectl create secret tls tomcat-ingress-secret --cert=tls.crt --key=tls.key -n testing
# create https Ingress
kubectl create -f ingress/tomcat-ingress-tls.yaml
```     
![alt text](https://github.com/luisxiaomai/Images/blob/master/Kubernetes-Practice/ingress-https.png)

  
## Volume-PV,PVC
### Static
![alt text](https://github.com/luisxiaomai/Images/blob/master/Kubernetes-Practice/static-volume.png)

```bash
# create /mnt/data/index.hml in one node
echo 'Hello from Kubernetes storage' > /mnt/data/index.html
# create pv
kubectl create -f volume/pv.yaml
# create pvc
kubectl create -f volume/pvc.yaml
# create nginx pod for test
kubectl create -f volume/pvc-pod-test.yaml
# entry the nginx pod 
curl http://localhost/
# verify that nginx is serving the index.html file from the hostPath volume
Hello from Kubernetes storage
```
### Dynamic
![alt text](https://github.com/luisxiaomai/Images/blob/master/Kubernetes-Practice/dynamic-volume.png)

Suppose provisioner services existed and related client pod provisioner has been created in k8s cluster
```bash
# Create Storage Class.
kubectl create -f volume/portworx-sc.yaml
# create pvc
kubectl create -f volume/portworx-volume-pvcsc.yaml
# create Create Pod which uses Persistent Volume Claim with storage class.
kubectl create -f volume/portworx-volume-pvcscpod.yaml
```
  
## ConfigMap
### Env Variable
### Volume
