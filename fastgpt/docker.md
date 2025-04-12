# 安装 Docker
```shell
curl -fsSL https://get.docker.com | bash -s docker --mirror Aliyun

systemctl enable --now docker
```

# 安装 docker-compose
```shell
curl -L https://github.com/docker/compose/releases/download/v2.20.3/docker-compose-`uname -s`-`uname -m` -o /usr/local/bin/docker-compose

chmod +x /usr/local/bin/docker-compose
```

# 验证安装
```shell
docker -v
docker-compose -v
```

# 如失效，自行百度~

# docker镜像配置
```shell
mkdir -p /etc/docker

tee /etc/docker/daemon.json <<-'EOF'
{
  "registry-mirrors": ["https://docker.1ms.run"]
}
EOF

systemctl daemon-reload
systemctl restart docker
```