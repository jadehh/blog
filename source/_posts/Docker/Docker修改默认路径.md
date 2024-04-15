---
title: Docker修改默认路径
categories: source/_posts/Docker
author: 简得辉
tags:
  - source/_posts/Docker
keywords: Docker修改默认路径
abbrlink: 20240412094456
data: 2024/4/12 9:44
---
[Toc]
### 1. 通常docker当前镜像路径可以通过 docker info 来查看，下图是未修改前的 docker 信息：
```
docker info
```
如下图

```text

Containers: 0
 Running: 0
 Paused: 0
 Stopped: 0
Images: 1
Server Version: 18.09.0
Storage Driver: overlay2
 Backing Filesystem: extfs
 Supports d_type: true
 Native Overlay Diff: true
Logging Driver: json-file
Cgroup Driver: cgroupfs
Hugetlb Pagesize: 2MB
Plugins:
 Volume: local
 Network: bridge host macvlan null overlay
 Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
Swarm: inactive
Runtimes: runc
Default Runtime: runc
Init Binary: docker-init
containerd version: d4c0ca4cf1a94aaf270e7eece79079e2315c6407
runc version: 8b826806605390950bb95c7e161124f5018f6181
init version: N/A (expected: )
Security Options:
 seccomp
  Profile: default
Kernel Version: 4.19.90-vhulk2206.1.0.h1194.eulerosv2r10.aarch64
Operating System: EulerOS 2.0 (SP10)
OSType: linux
Architecture: aarch64
CPUs: 4
Total Memory: 7.713GiB
Name: Euler
ID: S65E:PLW4:YTNO:Z3WU:CHQN:QXPE:NVBT:BZFG:5FST:WCD4:K35V:7QOY
Docker Root Dir: /var/lib/docker
Debug Mode (client): false
Debug Mode (server): false
Registry: https://index.docker.io/v1/
Labels:
Experimental: false
Insecure Registries:
 127.0.0.0/8
Live Restore Enabled: true

```


### 2.首先停止docker服务

```
sudo service docker stop
```

### 3.新建文件夹
```
sudo mkdir /etc/systemd/system/docker.service.d
```
### 4.修改 docker-overlay.conf 文件，如果没有请自行创建；
```
cd /etc/systemd/system/docker.service.d
sudo vi docker-overlay.conf
```
### 5.在文件中添加内容
```
[Service]
ExecStart=
ExecStart=/usr/bin/dockerd --graph=/home/jade/sda3/Docker
```
### 6.重启docker
```
systemctl daemon-reload
sudo service docker start
```

### 7.查看docker 信息，确认是否已经修改成功
```
sudo docker info
```


```text

Containers: 1
 Running: 0
 Paused: 0
 Stopped: 1
Images: 1
Server Version: 18.09.0
Storage Driver: overlay2
 Backing Filesystem: extfs
 Supports d_type: true
 Native Overlay Diff: true
Logging Driver: json-file
Cgroup Driver: cgroupfs
Hugetlb Pagesize: 2MB
Plugins:
 Volume: local
 Network: bridge host macvlan null overlay
 Log: awslogs fluentd gcplogs gelf journald json-file local logentries splunk syslog
Swarm: inactive
Runtimes: runc
Default Runtime: runc
Init Binary: docker-init
containerd version: d4c0ca4cf1a94aaf270e7eece79079e2315c6407
runc version: 8b826806605390950bb95c7e161124f5018f6181
init version: N/A (expected: )
Security Options:
 seccomp
  Profile: default
Kernel Version: 4.19.90-vhulk2206.1.0.h1194.eulerosv2r10.aarch64
Operating System: EulerOS 2.0 (SP10)
OSType: linux
Architecture: aarch64
CPUs: 4
Total Memory: 7.713GiB
Name: Euler
ID: S65E:PLW4:YTNO:Z3WU:CHQN:QXPE:NVBT:BZFG:5FST:WCD4:K35V:7QOY
Docker Root Dir: /root/sda2/Docker
Debug Mode (client): false
Debug Mode (server): false
Registry: https://index.docker.io/v1/
Labels:
Experimental: false
Insecure Registries:
 127.0.0.0/8
Live Restore Enabled: false
```