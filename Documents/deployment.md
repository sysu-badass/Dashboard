# 项目部署
## 在Ubuntu上下载并部署项目
```
git clone https://github.com/sysu-badass/BackEndServer.git
cd BackEndServer
sudo ./build.sh app
sudo ./build.sh db
sudo docker swarm init
sudo ./deploy.sh
```

## docker-compose.yml文件的编写
[docker-compose.yml](https://github.com/sysu-badass/Dashboard/blob/master/Documents/docker-compose.yml)
