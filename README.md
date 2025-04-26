Upgrading the EKSCTL with managed manully not with AWS :
--------------------------------------------------------
1) We can do one minor release upgrade .Then only easily we can triage the issue if any thing goes wrong .
2) Better to take the backups if any thing 
3) 


Because the kube-apiserver static pod is running at all times (even if you have drained the node),
 when you perform a kubeadm upgrade which includes an etcd upgrade, in-flight requests to the server will stall 
 while the new etcd static pod is restarting. 
 As a workaround, it is possible to actively stop the kube-apiserver process a few seconds before starting the kubeadm upgrade apply command. 
 This permits to complete in-flight requests and close existing connections, and minimizes the consequence of the etcd downtime.
 This can be done as follows on control plane nodes:
 
 step1: Switching to another Kubernetes package repository : This must do for every minor release 
 ------
			a) sudo vi nano /etc/apt/sources.list.d/kubernetes.list
			b) You will see below line :
				deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /
				then update as 
				deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /
				and save
				
 
 First 
 Stoping the api-servver
 ------------------------
killall -s SIGTERM kube-apiserver # trigger a graceful kube-apiserver shutdown
sleep 20 # wait a little bit to permit completing in-flight requests
kubeadm upgrade ... # execute a kubeadm upgrade command


First upate kubeadm :
---------------------
sudo apt update
sudo apt-cache madison kubeadm with this you will get the available version of kubeadm (Pick the latest or suitable one with kublet) and replace in the below command 

cmds:
-------
sudo apt-mark unhold kubeadm && \
sudo apt-get update && sudo apt-get install -y kubeadm='1.30.x-*' && \
sudo apt-mark hold kubeadm  

Drain the node
-------------------
kubectl drain <node-to-drain> --ignore-daemonsets

Upgrade kubelet and kubectl
---------------------------
sudo apt-mark unhold kubelet kubectl && \
sudo apt-get update && sudo apt-get install -y kubelet='1.32.x-*' kubectl='1.32.x-*' && \
sudo apt-mark hold kubelet kubectl


sudo systemctl daemon-reload
sudo systemctl restart kubelet

Uncordon the node :
-----------------------
kubectl uncordon <node-to-uncordon>



 step1: Switching to another Kubernetes package repository : This must do for every minor release 
 ------
			a) sudo vi nano /etc/apt/sources.list.d/kubernetes.list
			b) You will see below line :
				deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /
				then update as 
				deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /
				and save
Upgrading Linux worker nodes:
------------------------
First ssh to that particular noe then 

sudo apt-mark unhold kubeadm && \
sudo apt-get update && sudo apt-get install -y kubeadm='1.32.x-*' && \
sudo apt-mark hold kubeadm

sudo kubeadm upgrade node

Drain the node
---------------
kubectl drain <node-to-drain> --ignore-daemonsets

upgrade the kubelet and kubectl 
-------------------------------
sudo apt-mark unhold kubelet kubectl && \
sudo apt-get update && sudo apt-get install -y kubelet='1.32.x-*' kubectl='1.32.x-*' && \
sudo apt-mark hold kubelet kubectl

sudo systemctl daemon-reload
sudo systemctl restart kubelet

kubectl uncordon <node-to-uncordon>


1) We can do one minor release upgrade .Then only easily we can triage the issue if any thing goes wrong .
2) Better to take the backups if any thing 
3) 


Because the kube-apiserver static pod is running at all times (even if you have drained the node),
 when you perform a kubeadm upgrade which includes an etcd upgrade, in-flight requests to the server will stall 
 while the new etcd static pod is restarting. 
 As a workaround, it is possible to actively stop the kube-apiserver process a few seconds before starting the kubeadm upgrade apply command. 
 This permits to complete in-flight requests and close existing connections, and minimizes the consequence of the etcd downtime.
 This can be done as follows on control plane nodes:
 
 step1: Switching to another Kubernetes package repository : This must do for every minor release 
 ------
			a) sudo vi nano /etc/apt/sources.list.d/kubernetes.list
			b) You will see below line :
				deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /
				then update as 
				deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /
				and save
				
 
 First 
 Stoping the api-servver
 ------------------------
killall -s SIGTERM kube-apiserver # trigger a graceful kube-apiserver shutdown
sleep 20 # wait a little bit to permit completing in-flight requests
kubeadm upgrade ... # execute a kubeadm upgrade command


First upate kubeadm :
---------------------
sudo apt update
sudo apt-cache madison kubeadm with this you will get the available version of kubeadm (Pick the latest or suitable one with kublet) and replace in the below command 

cmds:
-------
sudo apt-mark unhold kubeadm && \
sudo apt-get update && sudo apt-get install -y kubeadm='1.30.x-*' && \
sudo apt-mark hold kubeadm  

Drain the node
-------------------
kubectl drain <node-to-drain> --ignore-daemonsets

Upgrade kubelet and kubectl
---------------------------
sudo apt-mark unhold kubelet kubectl && \
sudo apt-get update && sudo apt-get install -y kubelet='1.32.x-*' kubectl='1.32.x-*' && \
sudo apt-mark hold kubelet kubectl


sudo systemctl daemon-reload
sudo systemctl restart kubelet

Uncordon the node :
-----------------------
kubectl uncordon <node-to-uncordon>



 step1: Switching to another Kubernetes package repository : This must do for every minor release 
 ------
			a) sudo vi nano /etc/apt/sources.list.d/kubernetes.list
			b) You will see below line :
				deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.29/deb/ /
				then update as 
				deb [signed-by=/etc/apt/keyrings/kubernetes-apt-keyring.gpg] https://pkgs.k8s.io/core:/stable:/v1.30/deb/ /
				and save
Upgrading Linux worker nodes:
------------------------
First ssh to that particular noe then 

sudo apt-mark unhold kubeadm && \
sudo apt-get update && sudo apt-get install -y kubeadm='1.32.x-*' && \
sudo apt-mark hold kubeadm

sudo kubeadm upgrade node

Drain the node
---------------
kubectl drain <node-to-drain> --ignore-daemonsets

upgrade the kubelet and kubectl 
-------------------------------
sudo apt-mark unhold kubelet kubectl && \
sudo apt-get update && sudo apt-get install -y kubelet='1.32.x-*' kubectl='1.32.x-*' && \
sudo apt-mark hold kubelet kubectl

sudo systemctl daemon-reload
sudo systemctl restart kubelet

kubectl uncordon <node-to-uncordon>



Prerequisite :
---------------
1) It is a good practice to cordon the node ( it is not necessary to cordon ,but better to uncordon) 
2) Throughly go throught release notes document 
3) Check what and all the api depricated 
4) Downgrae can't happen .If anything goes wrong ,we need provision with new set up 
5) So , better we can test it in lower env first ,everything fine then only update the production env 
6) while upgrading the version, control plane and worker node should be on same version 
7) 


Creating the EKS cluster: 
-------------------------
step1: creation of control plane:
-----------------------------------
eksctl create cluster --name my-cluster --region region-code --version 1.32 --vpc-private-subnets subnet-ExampleID1,subnet-ExampleID2 --without-nodegroup

Installing osci plugins to communicate the pods with other resource 

Creating Node groups :
------------------------
eksctl create nodegroup \
  --cluster my-cluster \
  --region region-code \
  --name my-mng \
  --node-ami-family ami-family \
  --node-type m5.large \
  --nodes 3 \
  --nodes-min 2 \
  --nodes-max 4 \
  --ssh-access \
  --ssh-public-key my-key
  
  
  updating the config file
  -------------------------------
  aws eks update-kubeconfig --region region-code --name my-cluster
  
  deleting the cluster 
  --------------------
  eksctl delete cluster --name my-cluster --region region-code
  
  
  Starting with EKS cluster upgrade : 
  ------------------------------------
  step-1:  eksctl upgrade cluster --name <cluster-name> --version <version-number> --approve
  
  step-2:With the above command it will upgrade the control pane in single az ; 
  if we want to update it with multiple az we can prepare a config file and update it using the config file 
  
  --- 
apiVersion: eksctl.io/v1alpha5
kind: ClusterConfig

metadata:
  name: cluster-4
  region: eu-north-1

vpc:
  id: "vpc-0dd338ecf29863c55"  # (optional, must match VPC ID used for each subnet below)
  cidr: "192.168.0.0/16"       # (optional, must match CIDR used by the given VPC)
  subnets:
    # must provide 'private' and/or 'public' subnets by availability zone as shown
    private:
      eu-north-1a:
        id: "subnet-0b2512f8c6ae9bf30"
        cidr: "192.168.128.0/19" # (optional, must match CIDR used by the given subnet)

      eu-north-1b:
        id: "subnet-08cb9a2ed60394ce3"
        cidr: "192.168.64.0/19"  # (optional, must match CIDR used by the given subnet)

      eu-north-1c:
        id: "subnet-00f71956cdec8f1dc"
        cidr: "192.168.0.0/19"   # (optional, must match CIDR used by the given subnet)
		

with the above configuration file , cmd is 
eksctl upgrade -f cluster.yml --profile=<profile name> --approve
  
  
step3:
now check the health of the pods 
kubectl get pods -n kube-system 

step4 : 
update workerng and prometheusng version from AWS EKS 

sstep 5: 
Now upgrade the addons
	upgrade add-on with override if any aad-on fails with error mesage"configurationconflict"
		a)select add-on
		b)select edit 
		c) select version you want to upgrade 
		d)select optional configuration settings 
		e) select overridde 
		f)save 
setp 6:
Check if AWS_VPC_K8s_CNI_CUSTOM_NETWORK_CFG IS SET TO TRUE AT AWS-NODE DS .IF FALSE MAKE IT TO TRUE
	kubectl get daemonset aws-node -n kube-system -o yaml 
	kubectl edit daemonset aws-node -n kube-system 
step 7 :
kubectl get nodes -o wide 

step 8: 
kubectl get pods -A 

step9 
disable featuresGates.driftEnabled flag in karpenter config and restart karpenter 
kubectl edit cm karpenter-global-settings -n karpenter 
kubectl rollout restart deployment apps/karpenter -n karpenter -n karpenter


Steps to upgrade EKS cluster from console : 
------------------------------------------
first step for 
---------------
disable featuresGates.driftEnabled flag in karpenter config and restart karpenter 
kubectl edit cm karpenter-global-settings -n karpenter 
kubectl rollout restart deployment apps/karpenter -n karpenter -n karpenter

then follow these steps:
------------------------
1) goto aws console 
2) goto eks cluster and select upgrade 
3) select version to upgrade and confirm (Make sure to upgrade to minor version which is one version upgrade at a time) 
4) wait for cluster status to be update with ne version 
5) upgrade nodegroups by selecting update next to nodegroup 
6) disable featureGates.driftEnabled flag in karpenter config and restart karpenter 

https://github.com/eksctl-io/eksctl/blob/main/examples/04-existing-vpc.yaml
