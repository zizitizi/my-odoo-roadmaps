

# after installing ssl website editor not working

Just add this line on your nginx config file, on server block. It hangs because a request is send over http instead of https.

add_header 'Content-Security-Policy' 'upgrade-insecure-requests'; 
I tried with Odoo 16 CE, on ubuntu 22.04, and it works fine


https://www.odoo.com/forum/help-1/odoo-16-website-editor-i-cannot-edit-on-my-website-215417



# nginx block that i used

        server {
            server_name pardar.ir www.pardar.ir;
        
        location / {
                proxy_http_version 1.1;
                proxy_set_header Forwarded-by reverse_proxy_server;
                proxy_set_header X-Real-IP $remote_addr;
                proxy_set_header Host $host;
                proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
                proxy_pass http://127.0.0.1:8060;
                proxy_read_timeout 3m;
                proxy_connect_timeout 3m;
        
                 # Add the Content-Security-Policy header here
                add_header Content-Security-Policy 'upgrade-insecure-requests';
            }
        
        location /nginx_status {
               stub_status;
            }
        
        location = /nginx_health {
               return 200 "healthy\n";
               add_header content-type text/plain;
            }
        
        real_ip_header X-Forwarded-For;
        
        real_ip_recursive on;
        
        listen [::]:443 ssl ipv6only=on; # managed by Certbot
            listen 443 ssl; # managed by Certbot
            ssl_certificate /etc/letsencrypt/live/pardar.ir/fullchain.pem; # managed by Certbot
            ssl_certificate_key /etc/letsencrypt/live/pardar.ir/privkey.pem; # managed by Certbot
            include /etc/letsencrypt/options-ssl-nginx.conf; # managed by Certbot
            ssl_dhparam /etc/letsencrypt/ssl-dhparams.pem; # managed by Certbot
        
        }
        server {
            if (host = pardar.ir) {         return 301 https://host$request_uri;
            } # managed by Certbot
        
        listen 80;
            listen [::]:80;
            server_name pardar.ir www.pardar.ir;
            return 404; # managed by Certbot
        
        }
        





















