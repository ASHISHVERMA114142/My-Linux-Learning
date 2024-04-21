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
