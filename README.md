# redis 
redis tutorial. 

## Redis installation

### Install Redis Server through Docker 

+ create local folder to allow redis config and data to map to local folder

  ~~~ shell
  mkdir ~/Documents/docker_volume/redis/conf/
  mkdir ~/Documents/docker_volume/redis/data/
  ~~~

  

+ Download redis config file from [redis website](https://raw.githubusercontent.com/redis/redis/6.0/redis.conf), and save to ~/Documents/docker_volume/redis/conf/redis.conf

+ Update redis.conf

  + **bind 127.0.0.1** #注释掉这部分，这是限制redis只能本地访问
  + **protected-mode no** #默认yes，开启保护模式，限制为本地访问
  + **daemonize no**#默认no，改为yes意为以守护进程方式启动，可后台运行，除非kill进程，**改为yes会使配置文件方式启动redis失败**
  + **databases 16** #数据库个数（可选），我修改了这个只是查看是否生效
  + **dir ./** #输入本地redis数据库存放文件夹（可选）
  + **appendonly yes** #redis持久化（可选）
  + **requirepass** # password, run in local docker so no need to set

+ run docker command

~~~ shel
docker run -p 6379:6379 --name myredis -v /Users/<user>/Documents/docker_volume/redis/conf/redis.conf:/etc/redis/redis.conf -v /Users/<user>/Documents/docker_volume/redis/data:/data -d redis redis-server /etc/redis/redis.conf --appendonly yes

# Command guideline
# -p 6379:6379 端口映射：前表示主机部分，：后表示容器部分。

# --name myredis  指定该容器名称，查看和进行操作都比较方便。

# -v 挂载目录，规则与端口映射相同。

# 为什么需要挂载目录：个人认为docker是个沙箱隔离级别的容器，这个是它的特点及安全机制，不能随便访问外部（主机）资源目录，所以需要这个挂载目录机制。

# -d redis 表示后台启动redis

# redis-server /etc/redis/redis.conf  以配置文件启动redis，加载容器内的conf文件，最终找到的是挂载的目录/usr/local/docker/redis.conf

# --appendonly yes  开启redis 持久化

######
# Optional - Export this docker container as new images
######

#commit all changes
docker commit <container id> redis:vsofo

#start as new container
docker run -itd -p 6379:6379 customize_redis bash

#Export to a file
docker save -o customize_redis.tar redis:vsofo
~~~

+ Check redis status

  ~~~ shel
  docker ps
  docker logs <redis container name>
  ~~~

  

### Install Redis Client

+ Install another redis desktop manager

~~~ shel
brew install --cask another-redis-desktop-manager
~~~

+ Must go Application -> right click -> open to start another redis desktop manager
+ New Connection, only need host: 127.0.0.1, Port: 6379



