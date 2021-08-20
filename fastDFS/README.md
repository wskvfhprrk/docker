### 官网docker
```
https://github.com/happyfish100/fastdfs/commit/35dcf5ca1b793b34d6610d0e7b7d941651f231fb#diff-f2bb5730c7a29fd85001850d2d11dc7f
```

### docker建立fastDFS单机版（docker-compose.yml）

```
version: '3'
services:
  tracker:
    container_name: tracker
    image: ygqygq2/fastdfs-nginx
    command: tracker
    network_mode: host
    volumes:   
      - /var/fdfs/tracker:/var/fdfs    
  storage0:
    container_name: storage0
    image: ygqygq2/fastdfs-nginx
    command: storage
    network_mode: host  
    environment:
      - TRACKER_SERVER=127.0.0.1:22122
    volumes: 
      - /var/fdfs/storage0:/var/fdfs
  # storage1:
  #   container_name: storage1
  #   image: ygqygq2/fastdfs-nginx
  #   command: storage
  #   network_mode: host  
  #   environment:
  #     - TRACKER_SERVER=127.0.0.1:22122
  #   volumes: 
  #     - /var/fdfs/storage1:/var/fdfs
  #  storage2:
  #   container_name: storage2
  #   image: ygqygq2/fastdfs-nginx
  #   command: storage
  #   network_mode: host  
  #   environment:
  #     - TRACKER_SERVER=10.1.5.85:22122
  #     - GROUP_NAME=group2
  #     - PORT=24000
  #   volumes: 
  #     - /var/fdfs/storage2:/var/fdfs         
```
`docker-compose up -d`启动容器后，只需要打开防火墙8080和22122端口即可，8080是图片访问端口，22122是后台图片上传端口
需要修改访问端口，修改容器tracker里nginx配置,`TRACKER_SERVER`应写本地ip，不要`localhost`
