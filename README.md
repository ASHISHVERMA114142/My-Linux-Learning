# How to add alias 
1. vi .bashrc
2. set your aliase ( ex -> alias<your_alias_name > ="<command_or_name> )
3. save .bashrc file .


## How to setup your own microk8s cluster inside your local machine
-- This commands will install mircrok8s with kubectl and it will connect both.

1. sudo snap install microk8s --classic
2. microk8s status --wait-ready
3. sudo usermod -a -G microk8s dewashish
4. newgrp microk8s
5. instll kubectl ( curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl" )
6. curl -LO "https://dl.k8s.io/release/$(curl -L -s https://dl.k8s.io/release/stable.txt)/bin/linux/amd64/kubectl.sha256"
7. echo "$(cat kubectl.sha256)  kubectl" | sha256sum --check
8. sudo install -o root -g root -m 0755 kubectl /usr/local/bin/kubectl
9. kubectl version --client
10. cd #HOME
11. mkdir .kube
12. cd .kube
13. micork8s config > config
14. kubectl version
15. kubectl get all --all-namespaces
    ( Now you are good to go with microk8s with kubectl ) 

# commands for helm 
1. helm create helloworld ( this will create helloworld dir with all required yamls files )
2. helm install myhellowordl --debug --dry-run helloworld

# Interface vs Runnable Interface 
Interface -> An Interface is a java Reference Type similar to class that contains followings .
1. constants
2. method signature
3. default methods
4. static methods
5. nested type
Runnable -> Runnable is a functional interface in java that represents a command that can be executed . It is used for creating a new Thread . It has a single method "run()" which is meant to contain the code executed in the thread .

Purpose: An interface is used to achieve abstraction and multiple inheritance in Java. Runnable, being an interface itself, is used specifically for implementing multithreading.  
Method: An interface can have any number of methods, but Runnable interface has only one method, run().  
Usage: Any interface can be implemented by any class to use the methods declared in the interface. Runnable is specifically implemented by classes that are intended to be executed by a thread.  
Functional Interface: Runnable is a functional interface, meaning it has exactly one abstract method. This makes it a good fit for use with lambda expressions in Java 8 and beyond. A general interface is not necessarily a functional interface.

# Three types of interface 
1. Consumer interface take only one argument and returns nothing ... Consumer<T> and has only one method accept( Object)
2. Function interface take two argument first one is input type and second one is the output type .... Function <T,F> and has only one method accept ...
3. Supplier interface take only one argument and has only one method "get()" .. Supplier<T> ...

# Kind cluster ( locally run k8s cluster ) 
1. How to install kind on your local machine
   Linux ->
    For AMD64 / x86_64
    [ $(uname -m) = x86_64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.22.0/kind-linux-amd64
    For ARM64
    [ $(uname -m) = aarch64 ] && curl -Lo ./kind https://kind.sigs.k8s.io/dl/v0.22.0/kind-linux-arm64
   chmod +x ./kind
   sudo mv ./kind /usr/local/bin/kind
Windows ->
  curl.exe -Lo kind-windows-amd64.exe https://kind.sigs.k8s.io/dl/v0.22.0/kind-windows-amd64
  Move-Item .\kind-windows-amd64.exe c:\some-dir-in-your-PATH\kind.exe

Commands :
k=>kubectl 
persistance -> https://www.netapp.com/devops-solutions/what-is-kubernetes-persistent-volumes/#:~:text=A%20persistent%20volume%20is%20a%20volume%20plug-in%20that%20has,-provider-specific%20storage%20system.
youtube video -> https://www.youtube.com/watch?v=kkW7LNCsK74
For more details commands cheat sheet -> https://www.hackingnote.com/en/cheatsheets/kind/
1. kind create cluster 
2. king get clusters 
3. docker ps ( to see the processes ) 
4. grep server ~/.kube/config
5. k cluster-info 
6. kind create cluster --name <Name your cluster> 
7. grep server ~/.kube/config -> this will show us two cluster configuration for both the kind and < name of your cluster > at one config file .
8. Now how to interact with different clusters .
   i) k get nodes --context kind-kind
   ii) k get nodes --context <Name your Clusters >-kind
       ex-> kind get node --context kind-mycluster
9. k config get-contexts 
10. k config use-context kind-kind
11. kubecte get nodes -o wide
# why we load docker image to our cluster ??

Loading a Docker image into your Kubernetes cluster is necessary because Kubernetes uses the images to create containers, which are the fundamental units of deployment in a Kubernetes environment. By loading the image into the cluster, you ensure that the image is available to Kubernetes when creating pods, services, or other Kubernetes resources.

1. Load the Docker image into your Kubernetes 
cluster: kind load docker-image sleepy:latest
2. Verify the image is available in the cluster: kubectl get pods --all-namespaces -o jsonpath='{range .items[*]}{.spec.containers[*].image}{"\n"}{end}'

# what happen if we dont load docker image to our cluster??

If you don't load Docker images to your cluster, Kubernetes will not be able to create containers using those images. When you create a pod, Kubernetes will try to pull the specified image from a container registry, and if it fails to find the image, it will not be able to create the container.

Here's an example of a YAML manifest for a pod that uses a Docker image:

apiVersion: v1
kind: Pod
metadata:
  name: sleepy
spec:
  containers:
  - name: sleepy
    image: sleepy:latest
    imagePullPolicy: Never

In this example, the image field specifies the Docker image sleepy:latest. If you haven't loaded this image to your cluster, Kubernetes will not be able to create the container.

To avoid this issue, you can load the Docker image to your cluster using the kind load command, as I explained in my previous response. Alternatively, you can set the imagePullPolicy to Never or IfNotPresent, and Kubernetes will use the local image if it exists.

# docker exec -it my-node-name crictl images
Replace my-node-name with the name of the Docker container for the cluster node. This command will display a list of all the Docker images present on the cluster node.

#docker short tutorial 
https://www.hostinger.in/tutorials/docker-cheat-sheet?utm_campaign=Generic-Tutorials-DSA|NT:Se|LO:IN-t2&utm_medium=ppc&gad_source=1&gclid=EAIaIQobChMIkNf7oPSrhgMV1mwPAh2d6QA7EAAYASAAEgIJnfD_BwE

#DevTools settings 
spring.devtools.restart.poll-interval=2s
spring.devtools.restart.quiet-period=1s     

#spingboot application with mongodb container 
https://github.com/The-Tech-Tutor/mongo
https://www.linkedin.com/pulse/get-started-spring-boot-mongodb-docker-compose-saeid-farahi


# how to install bats 
https://bats-core.readthedocs.io/en/stable/installation.html
