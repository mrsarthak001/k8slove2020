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

#### Git clone the webapps from github and rename the folders
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

#### Create file for service named "q2srvsarthakjain.yaml"
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

#### Create service file named "q3svcsarthakjain.yaml"
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

## Task 4
#### create a replicasets
##### Create file named "q4rs.yaml"
```
apiVersion: apps/v1
kind: ReplicaSet
metadata:
  name: adhocrssarthakjain4
  labels:
    app: adhocrssarthakjain4
spec:
  replicas: 1
  selector:
    matchLabels:
      adhoc: sarthakjainq4 
  template:
    metadata:
      name: adhocpod4
      labels:
        adhoc: sarthyakjainq4
    spec:
      containers:
      - env:
        - name: x
          value: app2
        image: sarthakjain07/may2020q1:v1
        imagePullPolicy: Always
        name: adhhocpod4
        ports:
        - containerPort: 80
```
#### Create the replicaset from the file
```
kubectl create -f q4rs.yaml
```
#### Create file for service named "q4svcsarthakjain.yaml"
```
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    app: adhocrssarthakjain4
  name: q4svcsarthakjain
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    adhoc: sarthakjainq4
  type: LoadBalancer
```
#### Create the service
```
kubectl create -f q4svcsarthakjain.yaml
```
## Task 5
#### deployment create
##### Create file named "q5dep1.yaml"
```
apiVersion: apps/v1
kind: Deployment
metadata:
  name: adhhocdepsarthakjaini5
  labels:
    adhoc: sarthakjainq5
spec:
  replicas: 3
  selector:
    matchLabels:
      adhoc: sarthakjainq5
  template:
    metadata:
      labels:
        adhoc: sarthakjainq5
    spec:
       containers:
        - env:
          - name: x
            value: app2
          image: sarthakjain/may2020q1:v1
          imagePullPolicy: Always
          name: adhocpod2
          ports:
          - containerPort: 80
```
#### Create the replicaset from the file
```
kubectl create -f q5dep1.yaml
```
#### Create file for service named "q5svcsarthakjain.yaml"
```
apiVersion: v1
kind: Service
metadata:
  creationTimestamp: null
  labels:
    adhoc: q5svcsarthakjain
  name: q5svcsarthakjain
spec:
  ports:
  - port: 80
    protocol: TCP
    targetPort: 80
  selector:
    adhoc: sarthakjainq5
  type: LoadBalancer
```
#### Create the service
```
kubectl create -f q5svcsarthakjain.yaml
```

## Task 6
#### configure portainer in k8s
##### Create pod file named "portainer.yaml"
```
apiVersion: v1
kind: Pod
metadata:
  creationTimestamp: null
  labels:
    adhoc: sarthakjainq6
  name: adhocpod6
spec:
  nodeSelector:
    kubernetes.io/hostname: ip-172-31-41-74.ec2.internal
  containers:
  - image: portainer/portainer
    name: adhhocpod6
    ports:
    - containerPort: 9000
```
#### Create the pod
```
kubectl create -f portainer.yaml

````

#### Create service file named "q6svcsarthakjain.yaml"

```
apiVersion: v1
kind: Service
metadata:
  name: q6srvsarthakjain
spec:
  type: NodePort
  selector:
    adhoc: sarthakjainq6
  ports:
    - port: 80
      targetPort: 9000
```
#### Create the service
```
kubectl create -f q6svcsarthakjain.yaml
```
## Task 7
#### Run date command every 3 second and store output
##### Create pod file named "q7.yaml"
```
apiVersion: v1
kind: Pod
metadata:
  labels:
    adhoc: sarthakjainq7
  name: adhocpod7
spec:
  containers:
  - image: alpine
    name: adhhocpod7
    command: ["/bin/shsh","-c","while true; do date>>/mnt/date.txt; sleep 3; done"]
    resources: {}
  dnsPolicy: ClusterFirst
  restartPolicy: Never
status: {}
```
#### Create the pod
```
kubectl create -f q7.yaml

````
