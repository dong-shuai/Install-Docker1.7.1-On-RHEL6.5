Redhat 6.5 离线安装Docker1.7.1

1. 安装
将安装包上传到目标服务器，分别执行以下指令进行依赖包安装，最后安装docker1.7.1
```shell
rpm -ivh libcgroup-0.40.rc1-12.el6.x86_64.rpm
rpm -ivh lxc-libs-1.0.9-1.el6.x86_64.rpm
rpm -ivh lua-alt-getopt-0.7.0-1.el6.noarch.rpm
rpm -ivh lua-filesystem-1.4.2-1.el6.x86_64.rpm
rpm -ivh lua-lxc-1.0.9-1.el6.x86_64.rpm  
rpm -ivh lxc-1.0.9-1.el6.x86_64.rpm  
# 安装device-mapper-libs,不安装，或版本较低时，后面启动docker会报错(忽略依赖安装)
rpm -ivh device-mapper-libs-1.02.117-12.el6.x86_64.rpm --force --nodeps
rpm -ivh docker-io-1.7.1-2.el6.x86_64.rpm
```

2. 启动、测试Docker
```shell
root@virtual-machine:~# service docker start
Starting cgconfig service:                                 [  OK  ]
Starting docker:                                       [  OK  ]
root@virtual-machine:~# docker ps
CONTAINER ID        IMAGE               COMMAND             CREATED             STATUS              PORTS               NAMES 
root@virtual-machine:~# service docker status
docker (pid  25053) 正在运行...
root@virtual-machine:~# pstree -p | grep docker
        |-docker(25053)-+-{docker}(25056)
        |               |-{docker}(25057)
        |               |-{docker}(25058)
        |               |-{docker}(25059)
        |               `-{docker}(25095)
```


3. 加入开机启动项
```shell
root@virtual-machine:~# chkconfig docker on
```

4. 编辑/etc/sysconfig/docker配置文件。
在other_args配置项中添加加速器配置（Docker官方中国区加速器）
other_args=--registry-mirror=https://registry.docker-cn.com
最后执行 `service docker restart` 重启docker daemon。

5. 安装docker compose. [Redhat6.5 不支持新版本的docker-compose，不过1.5.1及以下可以]
(https://stackoverflow.com/questions/42669552/how-to-install-docker-compose-on-linux-rhel-6-6)
将docker-compose-Linux-x86_64_1.5.2上传到服务器。执行以下命令：

```shell
root@virtual-machine:~# move docker-compose-Linux-x86_64_1.5.2 /usr/local/bin/docker-compose
root@virtual-machine:~# chmod +x /usr/local/bin/docker-compose
#测试
root@virtual-machine:~# docker-compose  --version
```

