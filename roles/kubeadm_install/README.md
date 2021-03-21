Kubeadm Install
=========
Role to configure prerequisites for installing a Kubeadm cluster

- Remove existing repos and gpg keys
- Open firewalld ports
- Disable swap
- Load modules and edit sysctl
- Install containerd
- Install kubelet, kubeadm, and kubectl

Manual Commands to match this playbook
-------------
These assume you're running sudo.  

To ensure the gpg keys and repos are removed:
- rm -rf /etc/apt/sources.list.d/kubernetes.list
- rm -rf /usr/share/keyrings kubernetes-archive-keyring.gpg
- rm -rf /etc/apt/sources.list.d/docker.list
- rm -rf /usr/share/keyrings/docker-archive-keyring.gpg


To Open firewalld ports, restart, and enable firewalld: ( Do the --add-port= command for each port)
- firewall-cmd --permanent --add-port=6443/tcp
- systemctl restart firewalld
- systemctl enable firewalld

To disable swap:
- swapoff -a
- Edit /etc/fstab
  * Comment out the swap line

To check if br_netfilter and overlay modules are loaded and load them:
- lsmod | grep br_netfilter ( if nothing is output, its not loaded)
  * modprobe br_netfilter
- lsmod | grep overlay
  * modprobe overlay

Add modules to a modules-load.d config
- vi /etc/modules-load.d/k8s.conf
- Add the below to the file
  * overlay
  * br_netfilter
- hit ESC and type :wq to save and quit

Add sysctl configs to /etc/sysctl.d
- vi /etc/sysctl.d/k8s.conf
- Add the below lines to the file
  * net.bridge.bridge-nf-call-ip6tables = 1
  * net.bridge.bridge-nf-call-iptables = 1
  * net.ipv4.ip_forward                 = 1
- hit ESC and type :wq to save and quit

To apply the sysctl changes now type:
- sysctl --system

To install required packages to install containerd 
- apt-get install apt-transport-https ca-certificates curl gnupg lsb-release

Add docker official gpg key
- curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /usr/share/keyrings/docker-archive-keyring.gpg

Setup Stable docker repository
- echo \
  "deb [arch=arm64 signed-by=/usr/share/keyrings/docker-archive-keyring.gpg] https://download.docker.com/linux/ubuntu \
  $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list > /dev/null

Update repo lists
- apt-get update

Install containerd
- apt-get install containerd.io

Make /etc/containerd directory
- mkdir /etc/containerd

Set containerd config default
- containerd config default | sudo tee /etc/containerd/config.toml

Restart containerd
- systemctl restart containerd

Add lines to the end of /etc/containerd/config.toml
- vi /etc/containerd/config.toml
  * [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc]
  * [plugins."io.containerd.grpc.v1.cri".containerd.runtimes.runc.options]
  *  SystemdCgroup = true
- hit ESC and type :wq to save and quit

Restart containerd
- systemctl restart containerd

Download google cloud GPG key
- sudo curl -fsSLo /usr/share/keyrings/kubernetes-archive-keyring.gpg https://packages.cloud.google.com/apt/doc/apt-key.gpg

Setup kubernetes repository
- echo "deb [signed-by=/usr/share/keyrings/kubernetes-archive-keyring.gpg] https://apt.kubernetes.io/ kubernetes-xenial main" | sudo tee /etc/apt/sources.list.d/kubernetes.list

Update repo lists
- apt-get update

To Install kubeadm, kubectl, and kubelet
- apt-get install kubeadm kubectl kubelet

------------


Requirements
------------

- A Sudo user on your hosts you wish to apply this to
- An internet connection


License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
