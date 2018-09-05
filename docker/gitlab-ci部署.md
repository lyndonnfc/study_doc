## 安装runner

1，安装gitlab runner
1，curl -L https://packages.gitlab.com/install/repositories/runner/gitlab-ci-multi-runner/script.deb.sh -o /tmp/gl-runner.deb.sh

2，less /tmp/gl-runner.deb.sh


3，bash /tmp/gl-runner.deb.sh

4，sudo apt-get install gitlab-runner

## 注册runner
5，sudo gitlab-runner register
                                                
Please enter the gitlab-ci coordinator URL (e.g. https://gitlab.com/):
http://10.211.55.15/
Please enter the gitlab-ci token for this runner:
9q7YJeAYxt_ts1SkQayT
Please enter the gitlab-ci description for this runner:
[ubuntu]: lyndon
Please enter the gitlab-ci tags for this runner (comma separated):
lyndon_test
Whether to run untagged builds [true/false]:
[false]: false
Whether to lock Runner to current project [true/false]:
[false]: 
Registering runner... succeeded                     runner=9q7YJeAY
Please enter the executor: docker, docker-ssh, parallels, docker+machine, docker-ssh+machine, kubernetes, shell, ssh, virtualbox:
[alpine:latest]: docker
Please enter the default Docker image (e.g. ruby:2.1):
apline:latest    
Runner registered successfully. Feel free to start it, but if it's running already the config should be automatically reloaded! 

## 查看runner信息
6，sudo gitlab-runner list
Listing configured runners                          ConfigFile=/etc/gitlab-runner/config.toml
lyndon                                              Executor=docker Token=89a201628b9ea1664a38ca62a62554 URL=http://10.211.55.15/

## 查看runner状态
7，sudo gitlab-runner status
gitlab-runner: Service is running!

## 重启runner
8，sudo gitlab-runner restart

## 停止runner
9，sudo gitlab-runner stop

## 启动runner
10，sudo gitlab-runner start

查看runner列表
sudo gitlab-runner list
Listing configured runners                          ConfigFile=/etc/gitlab-runner/config.toml
lyndon                                              Executor=docker Token=89a201628b9ea1664a38ca62a62554 URL=http://10.211.55.15/
lyndon_test                                         Executor=docker Token=9f08729d8aa2de7c328a7b653bd098 URL=http://10.211.55.15/

删除指定的runner注册信息
gitlab-runner unregister --url http://10.211.55.15/ --token 89a201628b9ea1664a38ca62a62554



export JAVA_HOME=/usr/local/jdk/jdk1.8

export JRE_HOME=${JAVA_HOME}/jre

export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib

export PATH=.:${JAVA_HOME}/bin:$PATH


M2_HOME=/use/local/maven/apache-maven-3.5.3

CLASSPATH=$CLASSPATH:$M2_HOME/lib

PATH=$PATH:$M2_HOME/bin

export   PATH    CLASSPATH   M2_HOME
                              