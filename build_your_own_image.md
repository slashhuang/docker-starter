## write a dockerFile【docker配置文件,以官网为例】

```bash
    FROM docker/whalesay:latest
```
> The FROM keyword tells Docker which image your image is based on

```bash
    RUN apt-get -y update && apt-get install -y fortunes
```
> add the fortunes program to the image.


```bash
    CMD /usr/games/fortune -a | cowsay
```
> instruct the software to run when the image is loaded.

## 从dockerFile配置镜像
> 在当前目录下，运行
```bash
    docker build -t docker-whale .
```