Certificates
=========
#.GITIGNORE WARNING

##THIS ROLE WILL PUT ALL THE KEYS AND CERTS CREATED ONTO THE CONTROL NODE IN THE ROLES /FILES DIRECTORY. PLEASE AT THAT DIRECTORY TO YOUR .GITIGNORE SO YOU DO NOT UPLOAD YOUR KEYS AND CERTS TO GITHUB
-------------
Useful links:

- [Adding your own CA trusted to firefox](https://javorszky.co.uk/2019/11/06/get-firefox-to-trust-your-self-signed-certificates/)
- [Adding your own CA to Debian host](https://unix.stackexchange.com/questions/90450/adding-a-self-signed-certificate-to-the-trusted-list)
-------------
Documentation

How to apply OpenSSL extensions:
https://www.openssl.org/docs/man1.0.2/man5/x509v3_config.html

Ansible modules:
- https://docs.ansible.com/ansible/2.7/modules/openssl_certificate_module.html
- https://docs.ansible.com/ansible/2.4/openssl_csr_module.html
- https://docs.ansible.com/ansible/2.5/modules/openssl_privatekey_module.html

.
-------------
Errors I Encountered

When generating some files I was getting:
- "error:2406F079:random number generator:RAND_load_file:Cannot open file:../crypto/rand/randfile.c:88:Filename=/home/user/.rnd"
The fix was to comment out "RANDFILE = $ENV::HOME/.rnd" in /etc/ssl/openssl.cnf

I Also got this:
- "error:0D07A097:asn1 encoding routines:ASN1_mbstring_ncopy:string too long:../crypto/asn1/a_mbstr.c:107:maxsize=2"
If you see "maxsize=#" in the error it means you had more characters than allowed in a field. My case was I had more than 2 characters in the Country field. 
--------------
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

To View the CSR so you can verify it has all the right options you want:
- openssl req -text -noout -verify -in ca.csr

Create the CA cert
- openssl req -new -key ca-key.pem -in ca.csr -x509 -days 1000 -out ca.pem

You will repeat these steps; creating a key, csr, and cert over and over. HOWEVER the options in the $CONFIG variable will change depending on what the cert is for. CA:TRUE will only be applied for the CA. Everything else will get CA:FALSE. Pay attentions to key_usages and extended key_usages. 

Documentation for openssl extensions can be found:
https://www.openssl.org/docs/man1.0.2/man5/x509v3_config.html




Requirements
------------

- A Sudo user on your hosts you wish to apply this to
- An internet connection or openssl and required dependencies


License
-------

BSD

Author Information
------------------

An optional section for the role authors to include contact information, or a website (HTML is not allowed).
