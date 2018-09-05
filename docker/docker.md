### 

### docker版本

docker-ce 社区版

docker-ee  企业版

### 安装步骤





## 常用命令

```
// 查看镜像
docker images

// 编写dockerfile
FROM nginx
RUN echo '<h1>Hello,Docker!</h1>' > /usr/share/nginx/html/index.html

// 编译生成自定义镜像
docker build -t  image_name:image_version
例如： docker build -t nginx:v2 .

// 启动镜像
docker run 
例如： docker run --name web2 -d -p 8083:80 nginx:v2
// 更多的时候，需要让 Docker 在后台运行而不是直接把执行命令的结果输出在当前宿主机下。此时，可以通过添加 -d 参数来实现。

//  查看容器
docker ps
docker container ls

// 查看容器的输出信息
docker container logs [container ID or NAMES]

//  进入容器(其中，-t 选项让Docker分配一个伪终端（pseudo-tty）并绑定到容器的标准输入上， -i 则让容器的标准输入保持打开。)
docker exec -it web2 bash -- 通过容器名称
docker exec -it 96f975744f54 bash -- 通过容器id

// 停止容器
docker container stop [container ID or NAMES]

// 查看容器的状态
docker container ls -a

// 启动已经停止的容器
docker container start [container ID or NAMES]

// 重启容器
docker container restart [container ID or NAMES]

// 删除容器
docker container rm   [container ID or NAMES](已经停止的)
docker container rm -f [container ID or NAMES](正在运行的)

// 删除掉所以终止状态的容器
docker container prune
```



