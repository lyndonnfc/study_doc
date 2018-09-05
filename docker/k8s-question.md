### 重新启动机器，docker服务不能正常启动

```
# 查看错误信息
journalctl -xe -u docker --no-page
```

```
# 查看可以安装的docker版本
apt-cache policy docker-ce
```

```
# docker版本和k8s版本对应问题
```

```
# 查看k8s-master节点的token命令
kubeadm token generate
```

```
# ubuntu18.04系统，找不到docker17.03的源，可以更换对应的版本号
sudo add-apt-repository \
   "deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   $(lsb_release -cs) \
   stable"
   
   修改为：

add-apt-repository \
   		"deb [arch=amd64] https://download.docker.com/linux/ubuntu \
   		xenial \
   		stable"
   		
   		
   		sudo add-apt-repository \
     "deb [arch=amd64] https://mirrors.aliyun.com/docker-ce/linux/ubuntu \
     xenial \
     stable"
```
