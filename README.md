

  

# k8s-installation
![enter image description here](https://lh3.googleusercontent.com/u-UVCZJHQiRnsgebzsHX6igGyEBSrwCCqwM9wSgchqnygIIJRtwkYFMaVmWVJdQ4kHKDWMLnQ0QkiQ)

## About...

*This repository is used to create ***Kubernetes Cluster*** using **8** simple steps on ***bare VM's**** 


## Table of Contents

* [What are the pre-requisites ?](#pre-requisites)
* [What are the VM's provisioned ?](#configuration)
* [How to deploy kubernetes cluster ?](#deploy)
* [How to access Kubernetes Dashboard ?](#dashboard)
* [How to install NFS Server ?](#addons)
* [What are the addons provided ?](#addons)


<a id="pre-requisites"></a>
## What are the pre-requisites ?
* [Git](https://git-scm.com/downloads "Git")


<a id="configuration"></a>
## What are the VM's provisioned ?

***Note: We are not going to create any VM's during this process. User is expected to have VM's before proceeding with this repository***

*Below is the ***example configuration*** that we are going to refer ***through out the repository***.*

*Name*|*IP*|*OS*|*RAM*|*CPU*|
|----|----|----|----|----|
*k8s-master*   |*100.10.10.100*|*CentOS7*|*4GB* |*4*|
*k8s-worker-1* |*100.10.10.101*|*CentOS7*|*16GB*|*4*|
*k8s-worker-2* |*100.10.10.102*|*CentOS7*|*16GB*|*4*|
*k8s-worker-3* |*100.10.10.103*|*CentOS7*|*16GB*|*4*|


<a id="deploy"></a>
## How to deploy kubernetes cluster ?

*Checkout the code  (git clone https://github.com/SubhakarKotta/k8s-vagrant.git)* 

## ***Step 1***

***Update host names for all nodes***

* `100.10.10.100 (k8s-master)`
* `100.10.10.101 (k8s-worker-1)`
* `100.10.10.102 (k8s-worker-2)`
* `100.10.10.103 (k8s-worker-3)`

***Unix Command!!!***

`$ nano /etc/hostname` 


## ***Step 2***

***Copy k8s-installation folder to all master/worker nodes***

* `100.10.10.100 (k8s-master)`
* `100.10.10.101 (k8s-worker-1)`
* `100.10.10.102 (k8s-worker-2)`
* `100.10.10.103 (k8s-worker-3)`

*Example copy to root folder and execution permissions can be applied by executing the below command.*

***Unix Command!!!***

`$ chmod +x -R k8s-installation` 


## ***Step 3***

***Execute the below script on all master/worker nodes***

* `100.10.10.100 (k8s-master)`
* `100.10.10.101 (k8s-worker-1)`
* `100.10.10.102 (k8s-worker-2)`
* `100.10.10.103 (k8s-worker-3)`

![enter image description here](https://lh3.googleusercontent.com/ilOz9uQHxUPmMM1JKlg3uBHZoBFWsFkHdUu2gsxwJe679fwDgPQHdZ-vhHiNbrMJaPAJCxva8LYGqg)

*Before executing the below script please don't forget to change the IP's and Host Names as per your requirements in the below script*


  ***Unix Command!!!***
  
 `$ k8s-installation/provisioning/prerequisites/install.sh`


## ***Step 4***

***Reboot all master/worker nodes***

* `100.10.10.100 (k8s-master)`
* `100.10.10.101 (k8s-worker-1)`
* `100.10.10.102 (k8s-worker-2)`
* `100.10.10.103 (k8s-worker-3)`

***Unix Command!!!***

 `$ reboot`

## ***Step 5***

***Execute the below script only on master nodes***

* `100.10.10.100 (k8s-master)`

***Unix Command!!!***

`$ cd k8s-installation/provisioning/vm-master`

`$ ./install.sh`


## ***Step 6***

***Execute the below script on all worker nodes***

* `100.10.10.101 (k8s-worker-1)`
* `100.10.10.102 (k8s-worker-2)`
* `100.10.10.103 (k8s-worker-3)`

![enter image description here](https://lh3.googleusercontent.com/uz3dGNIXtUP9sFZNrDE3EOLbRjh7j96hIa1_g_Uf7bu23DEvn-phgyaP3QVzWGbI0EtlvWW9IS6nNQ)

  ***Unix Command!!!***
  
`$ kubeadm token create --print-join-command`

*Before executing the ***below*** script please ***don't forget*** to change the ***token*** in the script. We can get the ***token*** by running the ***above*** command on* ***master node IP***  

  ***Unix Command!!!***
  
`$ cd k8s-installation/provisioning/vm-worker`

`$ ./install.sh`



## ***Step 7***

***Execute the below script only on master node IP to install HELM***

* `100.10.10.100 (k8s-master)`

***Unix Command!!!***

`$ cd k8s-installation/provisioning/helm`

`$ ./install.sh`

## ***Step 8***
***Verify k8s-installation is success by executing below to commands to see all the nodes and pods.***

***Unix Command!!!***

`$ kubectl get nodes`

`$ kubectl get pods -o wide --all-namespaces`


<a id="dashboard"></a>
## How to access Kubernetes Dashboard ?

*The ***Kubernetes Dashboard*** can be accessed via the below URL with you ***master node IP*** with the same port ***30070****
[http://100.10.10.100:30070/#!/overview?namespace=_all](http://100.10.10.100:30070/#!/overview?namespace=_all)

<a id="nfs-configuration"></a>
## How to install NFS Server ?
***Here we are going use *master VM* as *NFS Server* instead of configuring separate VM***
*Execute the below command on only ***master*** node to install ***NFS-Client****
***Note:** Please don't forget to change **nfs.server** with your **master node IP** configured.*

`$ helm install stable/nfs-client-provisioner --name nfs-client-provisioner --set nfs.server=100.10.10.100 --set nfs.path=/mnt/storage --set storageClass.defaultClass=true`

<a id="addons"></a>
## What are the addons provided ?
* `kubernetes dashboard`
* `helm`