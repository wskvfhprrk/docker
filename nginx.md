#### nginx配置

```
upstream third.upstream {
    server 192.168.166.181:11000 weight=1;
}
server {
    listen       80;
    server_name  localhost;

    location /AlipayAward/ {
        proxy_redirect off;
        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_pass   http://third.upstream;        
    }
    
    location /{
        root   /usr/share/nginx/html;
        index  index.html index.htm;
     }
 }
```
