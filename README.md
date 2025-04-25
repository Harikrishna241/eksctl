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
