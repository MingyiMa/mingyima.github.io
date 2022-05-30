Kubernetes
==========

**Reference**:
* https://www.youtube.com/watch?v=d6WC5n9G_sM


Terminology
-----------
* Kubernetes is container orchestration system
* Kubernetes takes care of the
    * Automatic deployment of the containerized application across different servers
    * Distribution of the load across multiple servers
    * Auto-scaling of the deployed applications
    * Monitoring and health check of the containers
    * Replacement of the failed containers
* POD is the smallest unit
* Kubernetes Cluster >> Nodes - Master Node, Worker Node >> Pods >> Containers
* Kubernetes services
    * kubelet: network communication between nodes
    * kube-proxy
    * Container Runtime - running actual container inside each node like Docker
* cluster management - kubectl, command line tool - HTTPS to API Server


Kubernetes Example
------------------------

kubectl creating nginx pod
::
    minikube status
    minikube start --driver=virtualbox
    minikube ip
    minikube ssh / ssh docker@192.168.59.100 -default docker/tcuser
    kubectl cluster-info
    kubectl get nodes
    kubectl get pods
    kubectl get namespace
    kubectl get pods --namespace=kube-system
    kubectl run nginx --image=nginx
    kubectl describe pod nginx
    
check nginx pod
::
    ssh docker@192.168.59.100 -default docker/tcuser
    docker exec -it 89a17cf7b3b3 sh
    hostname -i
    curl 172.17.0.3
    kubectl get pods -o wide
    kubectl delete pod nginx

create deployment, scale up/down
::
    kubectl create deployment nginx-deployment --image=nginx
    kubectl get deployments
    kubectl describe deployments nginx-deployment
    kubectl scale deployment nginx-deployment --replicas=5
    kubectl get pods -o wide
    kubectl scale deployment nginx-deployment --replicas=3

create services
cluster-ip is only available inside the cluster
cluster-ip >> deployment >> pods, curl would be answered by one of the pods
::
    kubectl expose deployment nginx-deployment --port=8080 --target-port=80
    kubectl get services
    kubectl get svc
    kubectl describe service nginx-deployment
    kubectl delete deployment nginx-deployment
    kubectl delete service nginx-deployment

create customer image and use it for deployment
same port mapping, no need target port then
NodePort service, connect through outside by using port random generated port
LoadBalancer service
::
    docker build . -t mingyima/k8s-web-hello
    docker login
    docker push mingyima/k8s-web-hello
    kubectl create deployment k8s-web-hello --image=mingyima/k8s-web-hello
    kubectl expose deployment k8s-web-hello --port=3000
    minikube ip
    ssh docker@192.168.59.100
    curl 10.109.161.205:3000; echo
    kubectl delete service k8s-web-hello
    kubectl expose deployment k8s-web-hello --type=NodePort --port=3000
    minikube service k8s-web-hello
    kubectl expose deployment k8s-web-hello --type=LoadBalancer --port=3000

RollingUpdate - image is updating meanwhile old image is still working
::
    kubectl describe deploy k8s-web-hello
    docker build . -t mingyima/k8s-web-hello:2.0.0
    docker push mingyima/k8s-web-hello:2.0.0
    kubectl set image deployment k8s-web-hello k8s-web-hello=mingyima/k8s-web-hello:2.0.0
    kubectl rollout status deploy k8s-web-hello
    minikube dashboard
    kubectl delete all --all

YAML deployment
::
    kubectl apply -f deployment.yaml
    kubectl apply -f service.yaml
    kubectl delete -f deployment.yaml -f service.yaml