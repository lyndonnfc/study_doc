## gitlab搭建

1.安装依赖包，运行命令

sudo apt-get install curl openssh-server ca-certificates postfix

执行完成后，出现邮件配置，选择Internet那一项（不带Smarthost的）


2.安装gitlab-ce软件包

在https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/ubuntu/pool/xenial/main/g/gitlab-ce/链接中下载最新版gitlab-ce（这里下载gitlab-ce_9.5.5-ce.0_amd64.deb）


3.执行命令：

apt-get update

dpkg –i gitlab-ce_9.5.5-ce.0_amd64.deb


4.修改gitlab的配置

gedit /etc/gitlab/gitlab.rb

修改external_url为

external_url 'http://10.211.55.15'

该ip地址为ubuntu的ip地址（具体采用ifconfig查看）

注意：gitlab的ip必须跟ubuntu的ip相同，这样局域网中其他计算机才能访问到gitlab

 

5.gitlab配置重新生成

gitlab-ctl reconfigure


6.检查GitLab是否安装好并且已经正确运行，输入下面的命令

 sudo gitlab-ctl status

 

7.如果得到类似下面的结果，则说明GitLab运行正常

run: gitlab-workhorse: (pid 1148) 884s; run: log: (pid 1132) 884s  

run: logrotate: (pid 1150) 884s; run: log: (pid 1131) 884s 

run: nginx: (pid 1144) 884s; run: log: (pid 1129) 884s 

run: postgresql: (pid 1147) 884s; run: log: (pid 1130) 884s

run: redis: (pid 1146) 884s; run: log: (pid 1133) 884s 

run: sidekiq: (pid 1145) 884s; run: log: (pid 1128) 884s   

run: unicorn: (pid 1149) 885s; run: log: (pid 1134) 885s


8.在浏览器地址栏中输入： http://10.211.55.15，即可访问GitLab的Web页面

9.初始化页面，设置新的登录密码，默认用户名为root。设置完成之后，就可以登录了。

10.gitlab常用命令：
 --- 如果需要停止gitlab服务：sudo gitlab-ctl stop
 --- 如果需要开启gitlab服务：sudo gitlab-ctl start



