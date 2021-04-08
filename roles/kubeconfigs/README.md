Kubeconfigs
=========

Role to distribute kubeconfig for initializing the cluster

Requirements
------------

- kubeadm and all cluster components installed

Description
--------------

The cluster.kubeconfig.j2 file will be placed into the $HOME/.kube directory on master node and used with the kubeadm init --config option to initialize the cluster.

- There is a file under this roles files directory for more complext creation of kubeconfigs. 

Role Variables
--------------

Dependencies
------------

A list of other roles hosted on Galaxy should go here, plus any details in regards to parameters that may need to be set for other roles, or variables that are used from other roles.

Example Playbook
----------------

Including an example of how to use your role (for instance, with variables passed in as parameters) is always nice for users too:

    - hosts: servers
      roles:
         - { role: username.rolename, x: 42 }

License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
