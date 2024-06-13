  # Config & Mechanism of Nginx

1. Update of apt repository (Debian - Unbuntu)
```bat
sudo apt dpate
sudo apt upgrade
```

 &nbsp; 


2. Install of Nginx
```bat
sudo apt install nginx
```

If you install using apt repository, Nginx will be managed by paraent process whici is called systemctl. and it will be registered in .service file.

 &nbsp;

3. Start of Nginx (systemctl)
 ```bat
 sudo systemctl start nginx
 ```

&nbsp;

4. If you want to SSL protocol (https) you need to buy a domain (such as xxxxx.com) and configure like this
```bat
server{
        listen 9001 ssl;

        server_name {your domain};

        ssl_certificate /etc/letsencrypt/live/{your_domain}/fullchain.pem;
        ssl_certificate_key /etc/letsencrypt/live/{your_domain}/privkey.pem;

        location /{
                #redis
                proxy_pass http://172.30.1.71:10000;
        }

}
```

&nbsp;

4. You can use Nginx as LoadBalancer as below: (round-robin)
```bat

upstream backend{

        server 172.30.1.41:9005;
        server 172.30.1.26:9005;


}

server {
        listen 9002 ssl;

        server_name www.taylee.link;

        ssl_certificate *****/fullchain.pem;
        ssl_certificate_key ****/privkey.pem;

        location / {

                proxy_pass http://backend;

        }

}


```

