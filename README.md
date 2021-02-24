# Kubernetes-Practice

🎉🎉🎉  Kubernetes Study and Practice 🎉🎉🎉 

## Table of Contents

  - [Pod](#Pod)
  - [Service](#Service)
  - [Ingress](#Ingress)
  - ...
  
## Pod

## Service

## Ingress
![alt text](https://github.com/luisxiaomai/Images/blob/master/Kubernetes-Practice/ingress-flow.png)

#### Deploy Ingress Controller
```bash	
kubectl apply -f ingress/ingress-nginx-deploy.yaml
```

#### Deploy Tomcat Deployment and Service
```bash	
kubectl apply -f ingress/tomcat-deployment-service.yaml
```

#### Deploy Http Ingress
```bash	
kubectl apply -f ingress/tomcat-ingress.yaml
```
![alt text](https://github.com/luisxiaomai/Images/blob/master/Kubernetes-Practice/ingress-http.png)

#### Deploy Https Ingress

```bash
# create self certificate
openssl genrsa -out tls.key 2048
openssl req -new -x509 -key tls.key -out tls.crt -subj /C=CN/ST=GuangDong/L=GuangZhou/O=DevOps/CN=tomcat.linux.io -days 3650
# create k8s secret
kubectl create secret tls tomcat-ingress-secret --cert=tls.crt --key=tls.key -n testing
# create https Ingress
kubectl apply -f ingress/tomcat-ingress-tls.yaml
```     
![alt text](https://github.com/luisxiaomai/Images/blob/master/Kubernetes-Practice/ingress-https.png)

  
## ...
  
