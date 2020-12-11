# Kubernetes cluster
A vagrant script for setting up a Kubernetes cluster using Kubeadm

## Pre-requisites

 * **[Vagrant 2.1.4+](https://www.vagrantup.com)**
 * **[Virtualbox 5.2.18+](https://www.virtualbox.org)**

## How to Run

Note: This is for play and absolutely not intended for production use. These scripts come as is and are not entitled to any support or warranties whatsoever. While they are safe to run, use at your own discretion. 

Clone this git repository using the git CLI or by [downloading the zip file](https://github.com/karan-kapoor90/kubernetes-cluster/archive/main.zip) on a machine where you'd like to run your VM's. 

```
git clone https://github.com/karan-kapoor90/kubernetes-cluster.git
cd kubernetes-cluster
```

Execute the following vagrant command to start a new Kubernetes cluster, this will start one master and two nodes:

```
vagrant up
```

You can also start invidual machines by vagrant up k8s-head, vagrant up k8s-node-1 and vagrant up k8s-node-2

If more than two nodes are required, you can edit the servers array in the Vagrantfile

```
servers = [
    {
        :name => "k8s-node-3",
        :type => "node",
        :box => "ubuntu/xenial64",
        :box_version => "20180831.0.0",
        :eth1 => "192.168.205.13",
        :mem => "2048",
        :cpu => "2"
    }
]
 ```

As you can see above, you can also configure IP address, memory and CPU in the servers array. 

## Connecting to the cluster

Get a list of the VM's that are provisioned by this vagrant file using 

```
vagrant status
```

Now choose a machine you want to ssh into. If you want to use the kubeclt cli and interact with the k8s cluster, ssh into the master using the command

```
vagrant ssh k8s-head
```

To get out of the ssh session and go back to your own machine, type

```
exit
```

### Helpful tips

Perform the following on the k8s-head (master node) vm

**kubectl helpers**

Add an alias for kubectl (allows you to use `k` instead of `kubectl` when working with kubernetes), and enable autocomplete
    
```bash
cat  >~/.bash_profile <<EOF
alias k=kubectl 
complete -F __start_kubectl k 
source <(kubectl completion bash) 
EOF
source ~/.bash_profile
```

**kubectx and kubens**

```bash
wget https://github.com/ahmetb/kubectx/releases/download/v0.9.1/kubectx
wget https://github.com/ahmetb/kubectx/releases/download/v0.9.1/kubens
chmod +x kubectx kubens
sudo mv kubectx /usr/local/bin/kubectx
sudo mv kubens /usr/local/bin/kubens
```

**Installing interactive mode for kubectx and kubens**

```bash
git clone --depth 1 https://github.com/junegunn/fzf.git ~/.fzf
~/.fzf/install
source ~/.bashrc
```

## Stop the clusters

To stop the running VM's, don't stop from the Virtualbox UI, instead use the following command after getting out of any of the kubernetes VM, from the host machine's terminal

```
vagrant halt
```

When you want to return, simple open a terminal where the Vagrantfile is located on your machine, and run `vagratn up`.

## Clean-up

Execute the following command to remove the virtual machines created for the Kubernetes cluster in the folder where the Vagrantfile is located.
```
vagrant destroy
```

You can destroy individual machines by vagrant destroy k8s-node-1 -f

## Licensing

Credit: Originally created and open sourced by https://github.com/ecomm-integration-ballerina/kubernetes-cluster/ 

[Apache License, Version 2.0](http://opensource.org/licenses/Apache-2.0).
