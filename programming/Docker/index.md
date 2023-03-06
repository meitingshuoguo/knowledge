# [安装](https://yeasy.gitbook.io/docker_practice/install/centos)

```bash
# 卸载
sudo yum remove docker \
                  docker-client \
                  docker-client-latest \
                  docker-common \
                  docker-latest \
                  docker-latest-logrotate \
                  docker-logrotate \
                  docker-selinux \
                  docker-engine-selinux \
                  docker-engine
                  
# yum 安装
sudo yum install -y yum-utils

# 执行下面的命令添加 yum 软件源
sudo yum-config-manager \
    --add-repo \
    https://mirrors.aliyun.com/docker-ce/linux/centos/docker-ce.repo
sudo sed -i 's/download.docker.com/mirrors.aliyun.com\/docker-ce/g' /etc/yum.repos.d/docker-ce.repo

# 官方源
# $ sudo yum-config-manager \
#     --add-repo \
#     https://download.docker.com/linux/centos/docker-ce.repo

# 安装 docker-ce

```

# [安装 Docker-Compose](https://www.myfreax.com/how-to-install-and-use-docker-compose-on-centos-7/)

```bash
sudo curl -L "https://github.com/docker/compose/releases/download/1.23.1/docker-compose-$(uname -s)-$(uname -m)" -o /usr/local/bin/docker-compose

sudo chmod +x /usr/local/bin/docker-compose
docker-compose --version
```

# [安装sentry](https://develop.sentry.dev/self-hosted/)

端口：9000

```bash
 git clone https://github.com/getsentry/self-hosted.git sentry
 cd sentry
 sudo ./install.sh
 docker compose up -d
```

# [安装sonarqube](https://docs.sonarqube.org/latest/setup/install-server/)

端口：9100  docker pull sonarqube:7.9.5-community

gitlab person token: tW2AmdYFjB6ZJLjQ-wDv

### 创建卷

```bash
# 创建卷
docker volume create --name sonarqube_data
docker volume create --name sonarqube_logs
docker volume create --name sonarqube_extensions

docker volume create --name postgresql
docker volume create --name postgresql_data    
```

### 普通方式

```bash
# 下载并启动
# docker run -d --name sonarqube \
    -p 9100:9000 \
    -e SONAR_JDBC_URL=... \
    -e SONAR_JDBC_USERNAME=... \
    -e SONAR_JDBC_PASSWORD=... \
    -v sonarqube_data:/opt/sonarqube/data \
    -v sonarqube_extensions:/opt/sonarqube/extensions \
    -v sonarqube_logs:/opt/sonarqube/logs \
    sonarqube:9.7-community
```



### 使用Docker-Compose安装

1. docker-compose.yml

```bash
version: "3"

services:
  sonarqube:
    image: sonarqube:9.7-community
    depends_on:
      - db
    environment:
      SONAR_JDBC_URL: jdbc:postgresql://db:5432/sonar
      SONAR_JDBC_USERNAME: sonar
      SONAR_JDBC_PASSWORD: sonar
    volumes:
      - sonarqube_data:/opt/sonarqube/data
      - sonarqube_extensions:/opt/sonarqube/extensions
      - sonarqube_logs:/opt/sonarqube/logs
    ports:
      - "9100:9000"
  db:
    image: postgres:12
    environment:
      POSTGRES_USER: sonar
      POSTGRES_PASSWORD: sonar
    volumes:
      - postgresql:/var/lib/postgresql
      - postgresql_data:/var/lib/postgresql/data
    ports:
      - "5432:5432"

volumes:
  sonarqube_data:
  sonarqube_extensions:
  sonarqube_logs:
  postgresql:
  postgresql_data:
```

2. 后台运行
   - `docker-compose up -d`
   - 查看输出 `docker-compose logs -f` 

# 卸载

```bash
# 删除无主的数据卷
docker volume prune
# 删除 sonarqube
docker container rm -f [container ID]

```

