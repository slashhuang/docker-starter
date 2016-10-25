## step1

> get docker download-link

> url : https://download.docker.com/mac/stable/Docker.dmg

## step2

> get docker installed

> click the dmg install package and follow steps

## step3

> verify installation from 'hello-word'

```bash
    docker run hello-world
```

### docker的流程说明:

1. Docker Engine CLI client(客户端)  

>  ===>   the Docker Engine daemon(后台程序)

2. Docker Engine daemon  

>  ===pulled==> the "hello-world" image from the Docker Hub

3. Docker Engine daemon(后台程序） 

> ==created==> a new container from that image(可执行镜像)

4. Docker Engine daemon(后台程序)  

> ==流式输出==>streamed that output to the Docker Engine CLI client(客户端)


###  相关命令(show all containers on the system)

```bash
    docker ps -a
```