Configure Hosts
=========
Role to configure day one bootstrapping of hosts including:

- hostnames
- /etc/hosts file
- Add an administator user with sudo abilities
- Change the root password
- Distribute ssh key to hosts
- Change the login banner
- Lock the ubuntu account

Manual Commands to match this playbook
-------------
These assume you'r running sudo. The hostname, hosts file, and user will all need to be done on each machine you want them on. 

To set a hostname: 
- hostnamectl set-hostname
To edit /etc/hosts: 
- vi /etc/hosts
  * Use "i" to enter insert mode and use the arrow keys to move around
  * Hit "Esc" to exit insert mode and type ":wq" to write and quit the file

To change the root password: 
- passswd root

To add a user: 
- useradd k8sadmin -c "kubernetes admin" -s /bin/bash

To add a user to the sudo group:
- usermod -aG sudo k8sadmin

To change the password for the user:
- passwd k8sadmin

To make users home directory:
- mkdir /home/k8sadmin && chown k8sadmin:k8sadmin /home/k8sadmin

To lock the ubuntu account:
- usermod -L ubuntu

To create ssh keys for the user:
- ssh-keygen (follow the prompts or hint "Enter" 3 times)

This one only needs to be done from the machine you will manage all of the others from

To copy your ssh keys to the other hosts:
- ssh-copy-id k8sadmin@k8sworker01 (do this for each host)

Encrypting passwords
------------

* Create vault.pass in the playbook directory with a password that will be used to encrypt and decrypt with ansible vault
* Create a .gitignore file and place the name of the vault.pass file in it
* Run "ansible-vault encrypt_string --vault-password-file pass.vault 'password_to_encrypt' --name 'root_password'"

Requirements
------------

- A Sudo user on your hosts you wish to apply this to


License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
