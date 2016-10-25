# 以在一个虚拟机里面跑一个Java web docker服务

1. 生成关于java运行环境的一个虚拟镜像 image
2. docker run -it -v 本地目录:虚拟机目录 -p 本地端口:虚拟机端口 docker镜像【docker容器】
3. 通过端口号和相关bash操作，进行容器交互


> 以如下的bash为例
> [注:以下docker java image的bash命令来自爱屋吉屋FE恩鹏 https://github.com/mtyang]

```
#!/bin/bash
#
# 加载本地镜像
# docker load -i java.tar
#
#  其中，相关参数包括：

#   -i：表示以“交互模式”运行容器
#   -t：表示容器启动后会进入其命令行
#   -v：表示需要将本地哪个目录挂载到容器中，格式：-v <宿主机目录>:<容器目录>
# 启动容器
# docker run -it -v /Users/huangxiaogang/iwjw/iwjw-BE/iwjw-h5:/opt/root -p 8080:8080 enzo/java
#
# 退出容器
# exit
#
# 查看已启动的容器ID
# docker ps -a
#
# 启动容器
# docker start 容器ID
#
# 进入已经启动的容器
# docker exec -it 容器ID bash
#
# 手动打包其他环境
# dev test beta
# mvn clean install -U -Pdev -DskipTests

# 变量
#
# war包地址
WAR_URL="/opt/root/target/*.war"

# tomcat 地址
TOM_URL="/usr/share/tomcat7"

# 项目启动地址
TOM_ROOT="${TOM_URL}/webapps"

# 文件监听间隔，单位秒
WT=5

# 拷贝 vm
WC_VM="src/main/webapp/WEB-INF/tpl /usr/share/tomcat7/webapps/ROOT/WEB-INF/"

# 拷贝class
WC_JAVA="target/classes /usr/share/tomcat7/webapps/ROOT/WEB-INF/"

# 通用方法
#

# 使用新包
function newwar(){

    # 删除旧包
    rm -rf ${TOM_ROOT}/*

    # 移动war包
    mv ${WAR_URL} ${TOM_ROOT}/ROOT.war
}

# 重启tomcat
function restart(){
    # 关闭已启动程序
    killall -9 java
    # 启动服务
    ${TOM_URL}/bin/startup.sh
    # 输入启动日志
    tail -f ${TOM_URL}/logs/catalina.out
}

# 指令处理
while getopts ":yprcwlh" optname
do
    case "$optname" in
    "y")
        echo "更新jar包"

        mvn clean install -U -Plocal -DskipTests
        newwar
        restart
        ;;
    "p")
        echo "重新打包"

        mvn clean package -Plocal -DskipTests

        newwar
        restart
        ;;
    "r")
        echo "重启tomcat"

        restart
        ;;
    "c")
        echo "重新编译并重启服务"

        mvn clean compile -Plocal -DskipTests
        cp -R ${WC_JAVA}
        restart
        ;;
    "w")
        echo "开始监听vm文件"

        # 监听 VM
        watch -n ${WT} cp -R ${WC_VM}
        ;;
    "l")
        echo "日志"

        # 监听 VM
        tail -f ${TOM_URL}/logs/catalina.out
        ;;
    "h")

        echo " -y 更新maven包-编译-打包-发布-启动一条龙服务"
        echo " -p 编译打包发布启动一条龙服务"
        echo " -r 重启tomcat"
        echo " -c 重新java文件并部署重启服务"
        echo " -w 监听vm文件,默认5S同步一次"
        echo " -l 查看日志"
        echo " -h 帮助"
        ;;
    esac
done
```

