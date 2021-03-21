Certificates
=========
##.GITIGNORE WARNING

##THIS ROLE WILL PUT ALL THE KEYS AND CERTS CREATED ONTO THE CONTROL NODE IN THE ROLES /FILES DIRECTORY. PLEASE AT THAT DIRECTORY TO YOUR .GITIGNORE SO YOU DO NOT UPLOAD YOUR KEYS AND CERTS TO GITHUB
-------------
Role to create certificates:

- Create a CA
- Create keys, certiciate signing requests, and certificates
- Fetch files from the host you configured these on TO the Ansible control node
- Distribute certificates based on requirmentes

Manual Commands to match this playbook
-------------
These assume you're running sudo. 

Install openssl:
- apt-get install openssl

Create the CA private key
- openssl genrsa -out ca-key.pem 2048

Create CA csr
Creating openssl certs and CSR's requires configurations to be passed in for certain items like extensions. You can either create a .cfg file and pass it into the openssl command or specify the configuration as CONFIG= variable in the bash shell and then echo that variable. 
```
CONFIG="
distinguished_name = my_req_distinguished_name
req_extensions = my_extensions
prompt = no
[ my_req_distinguished_name ]
C = US
ST = State
L = City
O  = kubernetes
CN = kubernetes
[ my_extensions ]
basicConstraints=critical,CA:TRUE
keyUsage=critical, cRLSign, keyCertSign
"
```
- openssl req -config <(echo "$CONFIG") -new -key ca-key.pem -out ca.csr 





Requirements
------------

- A Sudo user on your hosts you wish to apply this to
- An internet connection or nginx and required dependencies


License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
