### 安装redis

1.安装Redis

```
sudo apt update
sudo apt install redis-server
```

2.Redis配置

```
sudo vim /etc/redis/redis.conf
```

2.1更改配置文件

```conf
supervised systemd
requirepass yang
```

2.2重启以加载配置

```
sudo service redis restart
```

2.3一些命令

```properties
# 查看redis运行状态
sudo systemctl status redis
#启动redis服务
sudo service redis start
#关闭redis服务
sudo service redis stop
#重启redis服务
sudo service redis restart
```

3.redis客户端

```
redis-cli
```



### 安装可视化工具

```xml
apt-get install zliblg-dev
apt-get -f insatll
#未安装libpng12-0错误
http://ftp.cn.debian.org/debian/pool/main/libp/libpng/
```

