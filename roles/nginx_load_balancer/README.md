NGINX Load Balancer
=========
Documentation: https://docs.nginx.com/nginx/admin-guide/load-balancer/tcp-udp-load-balancer/
-------------
Role to configure nginx load balancer:

- Install, start, and enable nginx
- Create config.d directory
- Add extra config directory to main config
- Create K8s master/control hosts load balancer config
- reload nginx

Manual Commands to match this playbook
-------------
These assume you're running sudo. 

Install nginx:
- apt-get install nginx

Start and enable nginx:
- systemctl start nginx
- systemctl enable nginx

Create /etc/nginx/tcpconf.d directory:
- mkdir /etc/nginx/tcpconf.d

Add include statement to /etc/nginx/nginx.conf:
- echo "include /etc/nginx/tcpconf.d/*" >> /etc/nginx/nginx.conf

Create /etc/nginx/tcpconf.d/kubernetes.conf:
- vi /etc/nginx/tcpconf.d/kubernetes.conf
- Take the file from the "templates" file in this role directory named "kubernetes_conf.j2" and paste it into /etc/nginx/tcpconf.d/kubernetes.conf
- Hit ESC and type :wq to write and quit the file

Reload nginx:
- nginx -s reload

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
