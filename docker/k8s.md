### 更新apt-get的源

```
# curl -s https://packages.cloud.google.com/apt/doc/apt-key.gpg | apt-key add
OK

# echo "deb http://apt.kubernetes.io/ kubernetes-xenial main" > /etc/apt/sources.list.d/kubernetes.list

# apt-get update
```



### 安装docker

```
# apt-get install  docker-engine

# docker version
```



### 安装kubernetes基础组件

```
// 查看当前可以安装的版本
# apt-cache madison kubeadm  
# apt-cache madison kubelet 
# apt-cache madison kubectl 
# apt-cache madison kubernetes-cni

// 安装
apt-get install kubeadm=1.11.0-00
apt-get install kubelet=1.11.0-00
apt-get install kubectl=1.11.0-00
apt-get install kubernetes-cni=0.6.0-00
```

### 复制虚拟机集群环境

```
## 自己玩的话是可以的
```



### 安装kubernetes Master 节点

```
# swapoff -a // 关闭swapoff
# kubeadm init --pod-network-cidr=192.168.0.0/16 --apiserver-advertise-address=10.211.55.24 --kubernetes-version=v1.11.0
```

```
I0813 12:03:23.969457   29086 feature_gate.go:230] feature gates: &{map[]}
[init] using Kubernetes version: v1.11.0
[preflight] running pre-flight checks
I0820 23:03:37.733752    5254 kernel_validator.go:81] Validating kernel version
I0820 23:03:37.733894    5254 kernel_validator.go:96] Validating kernel config
[preflight/images] Pulling images required for setting up a Kubernetes cluster
[preflight/images] This might take a minute or two, depending on the speed of your internet connection
[preflight/images] You can also perform this action in beforehand using 'kubeadm config images pull'
[kubelet] Writing kubelet environment file with flags to file "/var/lib/kubelet/kubeadm-flags.env"
[kubelet] Writing kubelet configuration to file "/var/lib/kubelet/config.yaml"
[preflight] Activating the kubelet service
[certificates] Generated ca certificate and key.
[certificates] Generated apiserver certificate and key.
[certificates] apiserver serving cert is signed for DNS names [ubuntu kubernetes kubernetes.default kubernetes.default.svc kubernetes.default.svc.cluster.local] and IPs [10.96.0.1 10.211.55.24]
[certificates] Generated apiserver-kubelet-client certificate and key.
[certificates] Generated sa key and public key.
[certificates] Generated front-proxy-ca certificate and key.
[certificates] Generated front-proxy-client certificate and key.
[certificates] Generated etcd/ca certificate and key.
[certificates] Generated etcd/server certificate and key.
[certificates] etcd/server serving cert is signed for DNS names [ubuntu localhost] and IPs [127.0.0.1 ::1]
[certificates] Generated etcd/peer certificate and key.
[certificates] etcd/peer serving cert is signed for DNS names [ubuntu localhost] and IPs [10.211.55.24 127.0.0.1 ::1]
[certificates] Generated etcd/healthcheck-client certificate and key.
[certificates] Generated apiserver-etcd-client certificate and key.
[certificates] valid certificates and keys now exist in "/etc/kubernetes/pki"
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/admin.conf"
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/kubelet.conf"
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/controller-manager.conf"
[kubeconfig] Wrote KubeConfig file to disk: "/etc/kubernetes/scheduler.conf"
[controlplane] wrote Static Pod manifest for component kube-apiserver to "/etc/kubernetes/manifests/kube-apiserver.yaml"
[controlplane] wrote Static Pod manifest for component kube-controller-manager to "/etc/kubernetes/manifests/kube-controller-manager.yaml"
[controlplane] wrote Static Pod manifest for component kube-scheduler to "/etc/kubernetes/manifests/kube-scheduler.yaml"
[etcd] Wrote Static Pod manifest for a local etcd instance to "/etc/kubernetes/manifests/etcd.yaml"
[init] waiting for the kubelet to boot up the control plane as Static Pods from directory "/etc/kubernetes/manifests" 
[init] this might take a minute or longer if the control plane images have to be pulled
[apiclient] All control plane components are healthy after 44.504002 seconds
[uploadconfig] storing the configuration used in ConfigMap "kubeadm-config" in the "kube-system" Namespace
[kubelet] Creating a ConfigMap "kubelet-config-1.11" in namespace kube-system with the configuration for the kubelets in the cluster
[markmaster] Marking the node ubuntu as master by adding the label "node-role.kubernetes.io/master=''"
[markmaster] Marking the node ubuntu as master by adding the taints [node-role.kubernetes.io/master:NoSchedule]
[patchnode] Uploading the CRI Socket information "/var/run/dockershim.sock" to the Node API object "ubuntu" as an annotation
[bootstraptoken] using token: zxv1pe.tyamjv82ytiqlcle
[bootstraptoken] configured RBAC rules to allow Node Bootstrap tokens to post CSRs in order for nodes to get long term certificate credentials
[bootstraptoken] configured RBAC rules to allow the csrapprover controller automatically approve CSRs from a Node Bootstrap Token
[bootstraptoken] configured RBAC rules to allow certificate rotation for all node client certificates in the cluster
[bootstraptoken] creating the "cluster-info" ConfigMap in the "kube-public" namespace
[addons] Applied essential addon: CoreDNS
[addons] Applied essential addon: kube-proxy

Your Kubernetes master has initialized successfully!

To start using your cluster, you need to run the following as a regular user:

  mkdir -p $HOME/.kube
  sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
  sudo chown $(id -u):$(id -g) $HOME/.kube/config

You should now deploy a pod network to the cluster.
Run "kubectl apply -f [podnetwork].yaml" with one of the options listed at:
  https://kubernetes.io/docs/concepts/cluster-administration/addons/

You can now join any number of machines by running the following on each node
as root:

  kubeadm join 10.211.55.24:6443 --token zxv1pe.tyamjv82ytiqlcle --discovery-token-ca-cert-hash sha256:7440fd587e96520ee8e2c7a6c64f62a6fb5bba7c7c6f89a9bf5cfc8f6261ad7b
```

### 配置

```
mkdir -p $HOME/.kube
sudo cp -i /etc/kubernetes/admin.conf $HOME/.kube/config
sudo chown $(id -u):$(id -g) $HOME/.kube/config

kubectl create -f https://github.com/coreos/flannel/raw/master/Documentation/kube-flannel.yml
```





## 加入salve

```
## 获取token
kubeadm token list

## 获取ca证书sha256编码hash值
openssl x509 -pubkey -in /etc/kubernetes/pki/ca.crt | openssl rsa -pubin -outform der 2>/dev/null | openssl dgst -sha256 -hex | sed 's/^.* //'
0fd95a9bc67a7bf0ef42da968a0d55d92e52898ec37c971bd77ee501d845b538

## join
kubeadm join 10.211.55.24:6443 --token jpqjt7.0joi2pwkvd1p5ag2 --discovery-token-ca-cert-hash sha256:78d314321104920bed45feb78ddec466f9e76cef22265e776808473151b15138
```



## master节点无法看见主机的问题

```
scp /etc/kubernetes/admin.conf lyndon@10.211.55.25:/tmp/admin.conf 

ln -s -f admin.conf kubelet.conf

service kubelet restart
```

