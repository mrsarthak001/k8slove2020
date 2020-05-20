# Docker and K8S Exam

## Task 1
#### create Dockerfile for httpd named "adhochttpd.dockerfile"
```
FROM centos
MAINTAINER mrsarthak001@gmail.com
RUN yum install -y httpd
ENV x=app
EXPOSE 80
RUN mkdir /myapps
RUN mkdir /scripts
COPY beginner-html-site-styled /myapps/app1
COPY project-html-website /myapps/app2
COPY start.sh /scripts/start.sh
RUN chmod +x /scripts/start.sh
ENTRYPOINT ["/bin/bash","/scripts/start.sh"]
```

#### Git clone the webapps grom github and rename the folders
```
git clone https://github.com/mdn/beginner-html-site-styled
git clone https://github.com/microsoft/project-html-website
```

#### Make the script "start.sh"
```
#!/bin/bash

if [ "$x" == "app1" ]
then
        cp -rf /myapps/app1/* /var/www/html/
        httpd -DFOREGROUND

elif [ "$x" == "app2" ]
then
        cp -rf /myapps/app1/* /var/www/html/
        httpd -DFOREGROUND
else
        echo "You have not selected correct env for app1 or app2" > /var/www/html/index.html
        httpd -DFOREGROUND
fi
```
#### Docker build command
```
docker build -f adhochttpd.dockerfile -t sarthakjain07/may2020q1:v1 .
```

#### Push to docker hub
```
docker login
docker push sarthakjain07/may2020q1:v1
docker logout
```


## Task 2
#### write a pod file and host it k8s
##### Create pod file named "q2.yaml"
```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    adhoc: sarthakjainq2
  name: adhhocpod1
spec:
  containers:
  - image: nginx
    name: adhhocpod1
    ports:
    - containerPort: 80
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```
#### Create the pod
```
kubectl create -f q2.yaml
```

#### Create file for service named "q2srvrhythmbhiwani.yaml"
```
apiVersion: v1
kind: Service
metadata:
  name: q2srvsarthakjain
spec:
  type: NodePort
  selector:
    adhoc: sarthakjainq2
  ports:
    - port: 80
      targetPort: 80
```
#### Create the service
```
kubectl create -f q2svcsarthakjain.yaml
```


## Task 3
#### write podfile with custom scheduling
#### Create a pod file named "q3.yaml"
```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    adhoc: sarthakjainq3
  name: adhhocpod2
spec:
  nodeSelector:
    kubernetes.io/hostname: ip-172-31-36-77.ec2.internal
  containers:
  - env:
    - name: x
      value: app2
    image: sarthakjain07/may2020q1:v1
    name: adhhocpod2
    ports:
    - containerPort: 80
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```
#### Create the pod
```
kubectl create -f q3.yaml
```

#### Create service file named "q3svcrhythmbhiwani.yaml"
```
apiVersion: v1
kind: Service
metadata:
  name: q3svcsarthakjain
spec:
  type: NodePort
  selector:
    adhoc: sarthakjainq3
  ports:
    - port: 80
      targetPort: 80
      nodePort: 32123
```
#### Create the service
```
kubectl create -f q3svcsarthakjain.yaml
```
