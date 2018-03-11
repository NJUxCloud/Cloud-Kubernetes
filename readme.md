NJUCloud-Kubernetes部署说明
--------------------------

## 运行环境
Centos7-单核-2G内存-40G


## 配置步骤

#### Step1
配置docker环境

```
yum install -y docker
```
```
systemctl enable docker && systemctl start docker
```

#### Step2
安装kubeadm

```
cat <<EOF > /etc/yum.repos.d/kubernetes.repo
[kubernetes]
name=Kubernetes
baseurl=https://packages.cloud.google.com/yum/repos/kubernetes-el7-x86_64
enabled=1
gpgcheck=1
repo_gpgcheck=1
gpgkey=https://packages.cloud.google.com/yum/doc/yum-key.gpg https://packages.cloud.google.com/yum/doc/rpm-package-key.gpg
EOF
```
```
setenforce 0
```
```
yum install -y kubelet kubeadm kubectl
```

```
systemctl enable kubelet && systemctl start kubelet
```

#### Step3
初始化集群:（此处使用flannel网络框架）

```
kubeadm init --apiserver-advertise-address=120.78.176.4  --pod-network-cidr=10.244.0.0/16
```

#### Step4
初始化网络框架

```
kubectl apply -f https://raw.githubusercontent.com/coreos/flannel/v0.9.1/Documentation/kube-flannel.yml
```

## 项目集群情况
#### 版本 
	1.8.2
	
#### 集群结构
	k8s-master 120.78.176.4
  	k8s-node-1 120.77.154.50
  	k8s-node-2 120.77.216.56 
  	k8s-node-3 119.23.51.139
  	
#### 分布式节点分配情况
	由于服务器网络原因，目前将分布式tensorflow部署于 k8s-node-3上，分PS和Worker训练节点
	分布式训练环境中，python为2.7版本，PS负责发布任务，Worker节点进行训练生成结果
	
#### 初始化文件*.yaml
	*.yaml文件为k8s集群部署时的初始化文件
	
#### 实现
	节点任务分配，训练，结果获取均由web端实现
	
	
### 注意，一切镜像源自Google，要保证服务器可以科学上网～