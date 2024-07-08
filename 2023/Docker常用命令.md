##### #启动docker服务

```bash
systemctl start docker
```

##### #停止docker服务

~~~bash
systemctl stop docker
~~~

##### #重启docker服务

```bash
systemctl restart docker
```

##### #查看docker服务状态

```bash
systemctl status docker
```

##### #设置开机启动docker服务

```bash
systemctl enable docker
```

##### #查看镜像

```bash
docker images
docker images -q # 查看所有镜像的id
```

##### #搜索镜像

```bash
docker search 镜像名称
```

##### #拉取镜像

```bash
docker pull 镜像名称/镜像名称:版本号
```

##### #删除镜像

```bash
docker rmi 镜像id/名称 # 删除指定本地镜像
docker rmi `docker images -q` # 删除所有本地镜像
```

##### #查看容器

```bash
docker ps # 查看正在运行的容器
docker ps -a # 查看所有容器
docker ps -aq # 查看所有容器id
```

##### #创建并启动容器

```bash
docker run 参数
docker run -it --name=x1 镜像 /bin/bash # 例
docker run -id --name=x2 镜像
# -i:保持容器运行，通常与-t同时使用，加it两个参数后，容器创建后自动进入容器，退出后容# 器自动关闭
# -t:为容器重新分配一个伪输入终端，通常与-i同时使用
# -it创建的容器一般称为交互式容器，-id创建容器一般称为守护式容器
# exec在后台运行时进入容器
docker exec -it c1 /bin/bash # 例 退出后容器不会关闭
# --name:为创建的容器命名
# -d 后台运行
#-i 参数表示以交互模式（Interactive Mode）运行容器允许你与容器的主进程进行交互
```

##### #停止容器

```bash
docker stop 容器名称
```

##### #启动容器

```bash
docker start 容器名称
```

##### #删除容器

```bash
docker rm 容器名称
docker rm `docker ps -aq` # 删除所有容器
```

##### #查看容器信息

```bash
docker inspect 容器名称
```

##### #配置数据卷

```bash
docker run -it --name=x2 -v 宿主机目录(文件):容器内目录(文件) 镜像
```

##### #配置数据卷容器

```bash
# 创建c3数据卷容器
docker run -it --name=c3 -v /volume 镜像 /bin/bash
# 创建c1、c2容器，使用--volumes-from设置数据卷
docker run -it --name=c1 --volumes-from c3 镜像 /bin/bash
docker run -it --name=c2 --volumes-from c3 镜像 /bin/bash
```

##### #docker应用部署

```bash
# 搜索镜像
docker search 应用镜像
# 拉取镜像
docker pull 应用镜像
# 创建容器，设置端口映射和数据卷映射,第一个80是宿主机端口号
docker run -id --name=xxx -p 80:80 -v 宿主机目录:容器内目录    应用镜像 
```

##### #镜像制作

```bash
# 将容器制作成镜像
docker commit 容器id/镜像名称:版本号
# 将镜像压缩
docker save -o 压缩文件名称 镜像名称:版本号
# 将压缩解为镜像
docker load -i 压缩文件名称
```

##### #dockerfile

| 关键字      | 作用                     | 备注                                                         |
| ----------- | ------------------------ | ------------------------------------------------------------ |
| FROM        | 指定父镜像               | 指定dockerfile基于那个image构建                              |
| MAINTAINER  | 作者信息                 | 用来标明这个dockerfile谁写的                                 |
| LABEL       | 标签                     | 用来标明dockerfile的标签 可以使用Label代替Maintainer 最终都是在docker image基本信息中可以查看 |
| RUN         | 执行命令                 | 执行一段命令 默认是/bin/sh 格式: RUN command 或者 RUN ["command" , "param1","param2"] |
| CMD         | 容器启动命令             | 提供启动容器时候的默认命令 和ENTRYPOINT配合使用.格式 CMD command param1 param2 或者 CMD ["command" , "param1","param2"] |
| ENTRYPOINT  | 入口                     | 一般在制作一些执行就关闭的容器中会使用                       |
| COPY        | 复制文件                 | build的时候复制文件到image中                                 |
| ADD         | 添加文件                 | build的时候添加文件到image中 不仅仅局限于当前build上下文 可以来源于远程服务 |
| ENV         | 环境变量                 | 指定build时候的环境变量 可以在启动的容器的时候 通过-e覆盖 格式ENV name=value |
| ARG         | 构建参数                 | 构建参数 只在构建的时候使用的参数 如果有ENV 那么ENV的相同名字的值始终覆盖arg的参数 |
| VOLUME      | 定义外部可以挂载的数据卷 | 指定build的image那些目录可以启动的时候挂载到文件系统中 启动容器的时候使用 -v 绑定 格式 VOLUME ["目录"] |
| EXPOSE      | 暴露端口                 | 定义容器运行的时候监听的端口 启动容器的使用-p来绑定暴露端口 格式: EXPOSE 8080 或者 EXPOSE 8080/udp |
| WORKDIR     | 工作目录                 | 指定容器内部的工作目录 如果没有创建则自动创建 如果指定/ 使用的是绝对地址 如果不是/开头那么是在上一条workdir的路径的相对路径 |
| USER        | 指定执行用户             | 指定build或者启动的时候 用户 在RUN CMD ENTRYPONT执行的时候的用户 |
| HEALTHCHECK | 健康检查                 | 指定监测当前容器的健康监测的命令 基本上没用 因为很多时候 应用本身有健康监测机制 |
| ONBUILD     | 触发器                   | 当存在ONBUILD关键字的镜像作为基础镜像的时候 当执行FROM完成之后 会执行 ONBUILD的命令 但是不影响当前镜像 用处也不怎么大 |
| STOPSIGNAL  | 发送信号量到宿主机       | 该STOPSIGNAL指令设置将发送到容器的系统调用信号以退出。       |
| SHELL       | 指定执行脚本的shell      | 指定RUN CMD ENTRYPOINT 执行命令的时候 使用的shell            |

##### #基于dockerfile构建镜像

```bash
docker build -f dockerfile -t centos:7 . 
-f ：指定dockerfile 文件（默认不写的话指的是当前目录）
-t ：镜像名和标签
```















