# Kubernetes-Home-Lab

A repository for the Ansible playbooks used in my Youtube video series. In this series I go over configuring my Raspberry Pi 4 cluster as a Kubernetes cluster using Kubeadm. I also automate the process as much as I can using these Ansible playbooks.

Each playbook role comes with a README file outling the manual commands as well if Ansible is not somethign the user wishes to use for this proces . 

Link to the youtube series:
- Part 1: [Design, Hardware, and Installing the OS](https://www.youtube.com/watch?v=s017P0ns-YY&t=439s)
- Part 2: [Configuring Bare Metal Hosts](https://www.youtube.com/watch?v=sDSuAPoM5iQ&t=505s)
- Part 3: [Configuring Pre-requisites to run Kubeadm](https://www.youtube.com/watch?v=BvMEXcJe-bs)
- Part 3a: [My Thought Process When Building Palybooks](https://www.youtube.com/watch?v=gO8OMoW5VLo&t=2027s)
- Part 4: [Deploying an NGINX Load Balancer with Ansible](https://www.youtube.com/watch?v=4W8cwgPJKrw&t=222s)
- Part 5: [Creating Encryption Secrets for Kubernetes Cluster with Ansible](https://www.youtube.com/watch?v=DkkJviaWklY&t=162s)
- Part 6: [Creating a CA and certificates for Kubernetes Cluster with Ansible](https://www.youtube.com/watch?v=l-gOIylwsWo)
- Part 7: [Deploying Kubeadm and Joining Nodes](https://www.youtube.com/watch?v=qjJOABXe1JA&t=2s)
- Part 8: [Creating an NFS server for persistent Storage](https://www.youtube.com/watch?v=L97Z5In3KXQ)


## Architecure and Versions

- Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-1029-raspi aarch64)
- Kubeadm, kubectl, kubelet v1.21.0
- containerd .io v1.4.4
- Flannel ( k8s networking solution)
- Openssl v1.1.1f

## Networking
-Cluster CIDR: IPs for pods - 10.240.0.0/16
-Worker Nodes Pod CIDR: Sepcific CIDR within the Cluster CIDR for one worker node. Multiple worker nodes pod CIDRs cannot overlap ( controlled by Flannel )

## Known Issues

###### Issue:
- Kubernetes-dashboard pods stuck in creating
###### Error:
- Warning  FailedCreatePodSandBox  13s (x6 over 79s)  kubelet            (combined from similar events): Failed to create pod sandbox: rpc error: code = Unknown desc = failed to setup network for sandbox "1cce51808f870550bc01b3a8899e1fa13cad6372fa465a8e4c459a18ec8c51ea": failed to set bridge addr: "cni0" already has an IP address different from 10.240.1.1/24
###### Fix:
- This happens when you destroy flannel and deploy it again sometimes. Go to the nodes the kubernetes-dashboard hosts are hosted on and run `sudo ip link set cni0 down` and then `sudo brctl delbr cni0` and wait a few minutes. 

###### Issue:
- Flannel containers CrashLoppBackoff
###### Error:
- Error registering network: failed to configure interface flannel.1: failed to ensure address of interface flannel.1: link has incompatible addresses. Remove additional addresses and try again.
###### Fix:
- This happens when you destroy flannel and deploy it again sometimes. On the nodes in question run  `sudo ip link delete flannel.1` and then wait a few minutes for flannel pods to restart. 


###### Issue:
- kubectl not working with error:
###### Error:
- The connection to the server localhost:8080 was refused - did you specify the right host or port?
###### Fix:
- This means the admin.conf for your cluster doesnt exist in that users $HOME/.kube directory. If Kubeadm did NOT finish the init this wont be available yet. If it did finish the init, simply place it in the directory `cp /etc/kubernetes/admin.conf $HOME/.kube/`

###### Issue:
- kubelet cannot connect to API server address:6443
###### Error:
- This shows up under `journalctl -u kubelet` or `systemctl status kubelet` as connection refused to the kube-api-server address on port 6443.
###### Fix:
- This one can happen for multiple reasons, but the best way to get the bottom of it is to first verify the kube-api container is healthy `crictl ps -a` will show you the running containers on the mastern node. If the kube-api one is in a state other than "running" you can do a `crictl logs < kube-api container ID >` to get more info about why the container is not creating. It may be due to a bad option or syntax passed into the config used in `kubeadm init --config`. 
- It may also be due to firewall issues. Check to make sure port 6443 is accesible. 

###### Issue:
- Nodes not ready when using `kubectl get nodes`
###### Error:
- Container runtime network not ready" networkReady="NetworkReady=false reason:NetworkPluginNotReady message:Network plugin returns error: cni plugin not initialized
###### Fix:
- Did you install networking such as Netweave, flannel, calico, etc? If not this is needed.
- Did you delete /opt/cni/ and now its empty? You should see a "bin" directory. If you did delete it, then reinstall `kubernetes-cni`.
- Try and restart containerd as well then check for the cni0 interface.

## Troubleshooting
###### Get logs from a container using crictl
- `crictl ps -a` to list running containers and their ID
- `crictl logs < container ID >` to view logs for the container
- `kubectl get nodes` will list the nodes and their READY or NOT READY status in the cluster
- `kubectl describe node < node >` will show more information about the node and possible reveal any issues
- `kubectl get pods -n kube-system -o wide` A widely used command to display all pods in the kube-system namespace and their status

