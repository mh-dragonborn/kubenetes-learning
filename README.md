# kubernetes-learning

## 为什么使用 kubernetes
### 服务架构的转变
![image](https://d33wubrfki0l68.cloudfront.net/26a177ede4d7b032362289c6fccd448fc4a91174/eb693/images/docs/container_evolution.svg)

### kubernetes 的结构

![image](https://d33wubrfki0l68.cloudfront.net/2475489eaf20163ec0f54ddc1d92aa8d4c87c96b/e7c81/images/docs/components-of-kubernetes.svg)

#### 控制面板内的组件
这些组件会对集群中的不同事件做出相应的动作
这些组件理论上可以部署在集群中的任意一台机器上，但是为了便于管理，会将这些组件部署让集群中的同一台机器中，并且不会让其他应用部署在这台机器上

- kube-apiserver
kube-apiserver相当于kubernetes集群的前端，给外部提供接口，来调度整个kubernetes集群
- ectd
ectd 是一个高可用的键值对存储工具，用来保存kubernetes集群中所需要的数据
- kube-scheduler
kube-scheduler是在一个调度器，比如给刚创建的pod指派一个node来运行，指派的时候会根据这个pod所需要的资源以及node中所拥有的资源来进行动态计算
- kube-controller-manager
kube-controller-manager管理整个kubernetes集群中的控制器，主要包含以下几个
  - Node controller
    检测node是否存活
  - Job controller
    监测那些Job对象，何时触发，然后创建相应的pod来执行任务
  - EndpointSlice controller
    填充 EndpointSlice 对象（以提供Service和 Pod 之间的链接）。
  - ServiceAccount controller
    为新创建的namespace来创建默认的ServiceAccount
- cloud-controller-manager
当kuberntes集群部署在本地时，是没有这个组件的
只有当集群部署在公有云上面时，这个组件可以让公有云提供商与集群进行通讯和交互

#### 每个节点内的组件
这些组件都存在于kuberntes集群中的每个节点内

- kubelet
kubelet存在于每个节点中，并且管理那些由kubernetes创建的容器，保证他们按照指定的PodSpecs来运行
- kube-proxy
kube-proxy存在于每个节点中，用于管理节点中pod与集群内外的网络通讯
- Container runtime
Container runtime是运行容器的软件，常见的是Containerd,CRI-O

## 了解kubernetes的对象

### 先看一个比较简单的yaml文件

```
  apiVersion: apps/v1
  kind: Deployment
  metadata:
    name: nginx-deployment
    namespace: mynamespace
  spec:
    selector:
      matchLabels:
        app: nginx
    replicas: 2 # tells deployment to run 2 pods matching the template
    template:
      metadata:
        labels:
          app: nginx
      spec:
        containers:
        - name: nginx
          image: nginx:1.14.2
          ports:
          - containerPort: 80
```
- apiVersion：表示使用的apiserver的版本
- kind：表示该资源的类型
- metadata：该资源的元属性，需要有一个唯一的name属性,namespace属性是可选的，不填就是default
- spec: 该资源的具体参数
  - selector： 表示这个deployment之管理label中含有 app:nginx的pod
  - replicas： 表示这个pod会创建两个
  - template： 表示这个deployment管理的pod的具体参数
    - metadata: pod的元属性 里面就包含这个pod带有的label标签
    - spec： pod的具体参数
      - containers： 这个pod所用的容器信息，包含镜像，及其端口号 

