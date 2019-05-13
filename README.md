

    We will use vagrant using Virtualbox as provider. Our nodes will run there.

    We will create 4 Kubernetes nodes for this deployment. 1 master and 3 nodes. 

   We will use Ubuntu 16.04 as OS in the nodes.

   Ansible is used to provision the machines

   And Kubeadm for Kubernetes configuration


   1-) We need to install Ansible. I have used a virtual environment for this setup : 
   virtualenv venv
   source venv/bin/activate
   pip install ansible

   2-) We ned to Configure Vagrant. We have a vagrant file for this. Please check Vagrantfile for this. Feel free to change the RAM parameter based on your local setup.

   3-) Start VMs : vagrant up

   4-) Kubernetes: We first have an inventory file containing information about our nodes. Check inventory file. We will use ansible for that part.

   We also have a vars.yml file, containing a few variables that will be used during the playbook execution.

   I configured 3 steps for kubernetes setup. 
   		Step1: Install Kubernetes and required package at everynode
   		Step2: Setup Master Node
   		Step3: Setup Workers
   	Please check playbook.yml file 

   	The roles used by Ansible are at roles directory....

   	Common role install common Kubernetes components : 

    docker.io
    kubelet
    kubeadm
    kubectl
    kubernetes-cni

    Master :
    etcd, 
    kube-apiserver, 
    kube-controller-manager, 
    kube-dns, 
    kube-proxy,

    Finally, the role installs the network solution that will be used by Kubernetes. In this case we chose Weave, because it is simple to setup and it runs as a CNI plug-in (kubeadm only supports CNI based networks).


    5-) We need to run ansible-playbook playbook.yml -i inventory -e @vars.yml


    6-) Check : 
    vagrant ssh kubernetes-master
    sudo kubectl get nodes

    You can even install a UI Dashboard for Kubernetes, by running the following on the master node :

    sudo kubectl create -f https://rawgit.com/kubernetes/dashboard/master/src/deploy/kubernetes-dashboard.yaml

