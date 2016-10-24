## 综述
> Docker Engine provides the core Docker technology that enables images and containers
> docker引擎提供了启动images【镜像】和containers【容器】的核心逻辑

## 什么是image
> An image is a filesystem and parameters to use at runtime
> 镜像是指文件系统和运行时的命令参数。
> 镜像不具备任何状态和改变
### 更多
> 镜像可以在此基础上承载更多功能，最典型的就是启动数据库，并等待数据的增删改查操作

## 什么是container
>  A container is a running instance of an image
> 容器就是运行镜像的实例

## 举例
```bash
    docker run hello-world
    # docker : 告诉操作系统，采用docker来处理这一切
    # run : docker的子命令，用来运行一个docker容器
    # hello-world: 告诉docker，采用哪个image，来加载进docker容器
```

### 小结
> 通过docker image，开发者可以共享软件、开发等，所有这一切都可以交给docker image(docker container)来完成这一切