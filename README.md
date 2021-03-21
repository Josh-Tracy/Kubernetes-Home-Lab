# Kubernetes-Home-Lab

A repository for the Ansible playbooks used in my Youtube video series. In this series I go over configuring my Raspberry Pi 4 cluster as a Kubernetes cluster using Kubeadm. I also automate the process as much as I can using these Ansible playbooks.

Each playbook role comes with a README file outling the manual commands as well if Ansible is not somethign the user wishes to use for this proces . 

Link to the youtube series:
- Part 1: https://www.youtube.com/watch?v=s017P0ns-YY&t=439s
- Part 2: https://www.youtube.com/watch?v=sDSuAPoM5iQ&t=505s
- Part 3: https://www.youtube.com/watch?v=BvMEXcJe-bs
- Part 3a: 
- Part 4: https://www.youtube.com/watch?v=4W8cwgPJKrw&t=222s
- Part 5:

## Architecure and Versions

- Ubuntu 20.04.2 LTS (GNU/Linux 5.4.0-1029-raspi aarch64)
- Kubeadm, kubectl, kubelet v1.20.4
- containerd .io v1.4.4
- Flannel ( k8s networking solution)
- Openssl v1.1.1f

## Networking
-Cluster CIDR: IPs for pods - 10.100.0.0/16
-Service Cluster CIDR: IPs for services - 10.32.0.0/24
-Worker Nodes Pod CIDR: Sepcific CIDR within the Cluster CIDR for one worker node. Multiple worker nodes pod CIDRs cannot overlap ( controlled by Flannel )