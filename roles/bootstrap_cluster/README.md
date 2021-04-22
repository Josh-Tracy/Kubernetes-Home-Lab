Bootstrap Cluster
=========

This role does the following:

- Cleans up old kubeadm configs and reset cluster on all nodes
- Creates etcd for use with multiple master/controller nodes
- Initializes the cluster on the master node
- Distributes the $HOME/.kube/config to all nodes
- Creates and parses out token and hash variables for dynamic kubeadm join commands on cluster nodes
- Joins other nodes to the cluster
- Installs flannel CNI


Requirements
------------
- Variables: Edit the variables for the etcd template in the vars directory 
- The ability to connect to the internet or a flannel.yaml file availble on an air gapped network
- An account with sudoer privileges

Known issues
--------------
- Flannel pods stuck in an Error and/or Crash state: This was due to the api-server not reachable from flannel pods. Kube-proxy was not creating iptables rules. The only way to get around this was to disable firewalld, add multiple ALLOW policies in the kubeadm_install role, and remove all cluster configurations. 


License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
