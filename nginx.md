# docker下使用nginx前后端分离

## docker安装nginx

```ssh
 docker run --name nginx9000 -p 9000:80  -v /opt/nginx/html:/usr/share/nginx/html:ro -v /opt/nginx/conf.d:/etc/nginx/conf.d -d nginx
```

**重要说明**：

1、-p端口映射，可以映射为任何端口，但后面必须是80端口

2、-v中容器路径可以随意更改，但后面路径不可更改，html中放置的静态页面，conf.d中存在nginx的配置文件conf

3、--name 要建名称+端口号进行多个nginx区分

```ssh
##为服务器ip和端口
upstream third.upstream {
    server 192.168.166.180:8080 weight=1;
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
    	## root的路径是docker nginx默认静态路径，与docker映射的宿主机静态页面的路径一致
        root   /usr/share/nginx/html;
        index  index.html index.htm;
     }
 }
```

