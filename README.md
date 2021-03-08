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
kubectl create -f Ingress/ingress-nginx-deploy.yaml
```

### Deploy Tomcat Deployment and Service
```bash	
kubectl create -f Ingress/tomcat-deployment-service.yaml
```

### Deploy Http Ingress
```bash	
kubectl create -f Ingress/tomcat-ingress.yaml
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
kubectl create -f Ingress/tomcat-ingress-tls.yaml
```     
![alt text](https://github.com/luisxiaomai/Images/blob/master/Kubernetes-Practice/ingress-https.png)

  
## Volume-PV,PVC
### Static Provisioning
![alt text](https://github.com/luisxiaomai/Images/blob/master/Kubernetes-Practice/static-volume.png)

```bash
# create /mnt/data/index.hml in one node
echo 'Hello from Kubernetes storage' > /mnt/data/index.html
# create pv
kubectl create -f Volume/pv.yaml
# create pvc
kubectl create -f Volume/pvc.yaml
# create nginx pod for test
kubectl create -f Volume/pvc-pod-test.yaml
# entry the nginx pod 
curl http://localhost/
# verify that nginx is serving the index.html file from the hostPath volume
Hello from Kubernetes storage
```
### Dynamic Provisioning
![alt text](https://github.com/luisxiaomai/Images/blob/master/Kubernetes-Practice/dynamic-volume.png)

*Suppose provisioner services existed and related client pod provisioner has been created in k8s cluster*
```bash
# create Storage Class.
kubectl create -f Volume/portworx-sc.yaml
# create pvc
kubectl create -f Volume/portworx-volume-pvcsc.yaml
# create Create Pod which uses Persistent Volume Claim with storage class.
kubectl create -f Volume/portworx-volume-pvcscpod.yaml
```
  
## ConfigMap
![alt text](https://github.com/luisxiaomai/Images/blob/master/Kubernetes-Practice/configmap.png)

### Environment Variables
```bash
# create ConfigMap
kubectl create -f ConfigMap/configmap-env.yaml
# create test pod
kubectl create -f ConfigMap/configmap-env-pod.yaml
# execute check env variables command in pod contianer
kubectl exec -it configmap-env bash -- env | grep "log_level\|SPECIAL_HOW_KEY"
# verify that env variables from configmap injected in pod container
log_level=INFO
SPECIAL_HOW_KEY=very
```

### Configuration Files in a Volume
```bash
# create ConfigMap
kubectl create -f ConfigMap/configmap-volume.yaml
# create test pod
kubectl create -f ConfigMap/configmap-volume-pod.yaml
# execute check config files command in pod contianer
kubectl exec -it configmap-volume bash -- ls /etc/config
# verify configs files which defined in configmap keys exist in /etc/config directory
special.how  special.when
```

## Secret
![alt text](https://github.com/luisxiaomai/Images/blob/master/Kubernetes-Practice/configmap-secret.png)

Secret is similar to config maps, secrets can be mounted into a pod as a volume to expose needed information or can be injected as environment variables.
For example, to store two strings in a Secret using the data field, convert the strings to base64 as follows
```bash
# convert the "admin/123" to base64 as follows 
> echo -n 'admin' | base64
YWRtaW4=
> echo -n '123' | base64
MTIz
# create secret
> kubectl create -f Secret/secret-env.yaml
# create test pod
> kubectl create -f Secret/secret-env-pod.yaml
# execute and check secrets existed in env variables of test pod container
> kubectl exec -it configmap-volume bash -- env
  password=123
  username=admin
```
