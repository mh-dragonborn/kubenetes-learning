如何在不使用docker desktop的情况下打包镜像
https://dev.to/fanmixco/how-to-compile-docker-images-in-windows-without-docker-desktop-using-wsl2-1pk9

kuboard安装地址
https://kuboard.cn/

docker 使用http的 harbor地址
https://docs.docker.com/registry/insecure/

deployment拉取镜像需要先指定docker的secret

kubectl create secret -n <namespaceName> docker-registry <secretName> \
  --docker-server=<harborServer> \
  --docker-username=<username> \
  --docker-password=<password> \
  --docker-email=<email>

 修改/etc/containerd/config.toml文件


How to enable kube-system/metrics-server from status "False (MissingEndpoints)"?

  1. wget https://github.com/kubernetes-sigs/metrics-server/releases/latest/download/components.yaml
  2. kubectl delete -f components.yaml
  3. Edit downloaded file and add - --kubelet-insecure-tls flag:
    labels:
      k8s-app: metrics-server
    spec:
      containers:
      - args:
        - --cert-dir=/tmp
        - --secure-port=443
        - --kubelet-preferred-address-types=InternalIP,ExternalIP,Hostname
        - --kubelet-use-node-status-port
        - --metric-resolution=15s
        - --kubelet-insecure-tls
  4.  kubectl apply -f components.yaml
 
