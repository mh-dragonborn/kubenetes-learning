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
  
  
 
