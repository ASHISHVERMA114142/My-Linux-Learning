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
