# Ravelin Code Test For DevOps - Sanket Deshpande Working Steps 

### Setup Tools and environment 
1. Setup gcloud tool locally 
2. Successfully created VM with the correct ravelin image
3. Setup http and https ingress from ALL
4. VM instance external IP 35.237.228.128 and internal IP is 10.142.0.2
5. Last login: Thu Jun 14 15:26:40 2018 from 88.98.211.9

### Problem 1: 
1. Created self signed SSL cert and key
2. Added a new ravelinsite to nginx site-available and symlink to site-enabled. Added the correct site block i.e.

>  server {
    listen 443 ssl;
    server_name 10.142.0.2;
    ssl_certificate     /etc/ssl/certs/selfsigned.crt;
    ssl_certificate_key /etc/ssl/private/selfsigned.key;
}


3. Run nginx -t Test
4. Restarted nginx
5. Tested ssl with curl -k --ipv4 https://localhost:443
  
> <!DOCTYPE html>
> <html>
> <head>
> <title>Welcome to nginx!</title>
> <style>
>    body {
>        width: 35em;
>        margin: 0 auto;
>        font-family: Tahoma, Verdana, Arial, sans-serif;
>    }
> </style>
> </head>
> <body>
> <h1>Welcome to nginx!</h1>
> <p>If you see this page, the nginx web server is successfully installed and
> working. Further configuration is required.</p>
>
>
> <p>For online documentation and support please refer to
> <a href="http://nginx.org/">nginx.org</a>.<br/>
> Commercial support is available at
> <a href="http://nginx.com/">nginx.com</a>.</p>>
>
>
> <p><em>Thank you for using nginx.</em></p>
> </body>
> </html>



### PROBLEM 2:

1. First collect the server certificates by using the below commands. They yield different results.
> openssl s_client -showcerts -servername www.example.com -connect www.example.com:443 </dev/null
> openssl s_client -connect api.ravelin.com:443 -CApath /etc/ssl/certs -servername api.ravelin.com
> openssl s_client -CApath /etc/ssl/certs/ -connect ravelin.com:443

2.  Collected certificate for *.ravelin.com, Gandi Standard SSL CA 2, DigiCert High Assurance EV Root CA, USERTRUST RSA Cert Authority
3. Also collected cacert.pem from https://curl.haxx.se/docs/sslcerts.html 
4. Added various .crt files to /usr/share/ca-certificates/
5. Added various .crt files to /etc/ca-certificates.conf
6. Ran update-ca-certificates --fresh. The stdout shows 130 certs updated
7. Check .crt have been added to /etc/ssl/certs/ca-certificates.crt. They are successfully added. 


### PROBLEM 3:
1. Adduser sudo user no password. Ssh public key added to /home/ravelin/.ssh/authorized_keys
2. Sudo adduser ravelin
3. Sudo usermod -aG sudo ravelin
4. Sudo visudo - add line "ravelin ALL=(ALL) NOPASSWD:ALL"
5. As root user - "passwd -d ravelin"
6. As ravelin user - run cmd - ls /root/ not allowed, then try sudo ls /root/ allowed. which is what we want i.e. sudo without password.



### PROBLEM 4: High CPU
1. Identify process called stress-ng. Kill -9 PID
2. Pstree -s PID. shows stress-ng-cpu is started by systemd. But could not find the service config to prevent systemd from starting the service again

### PROBLEM 5:
-
### PROBLEM 6:
-
