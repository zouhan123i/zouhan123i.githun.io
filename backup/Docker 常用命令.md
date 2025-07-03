以下是 **Docker 常用命令分类整理**，涵盖镜像管理、容器操作、网络存储等核心场景，适合开发与运维日常使用：

---

### **一、镜像管理**

### 1. **拉取与查看镜像**

```
docker pull nginx:latest          # 拉取官方镜像（默认从Docker Hub）
docker images                     # 查看本地镜像列表
docker image inspect nginx        # 查看镜像详细信息（元数据、层信息）
```

### 2. **构建与删除镜像**

```
docker build -t myapp:1.0 .       # 根据当前目录Dockerfile构建镜像
docker rmi nginx:latest           # 删除本地镜像（需先停止关联容器）
docker image prune                # 删除未被使用的镜像（悬空镜像）
```

### 3. **导入与导出**

```
docker save -o nginx.tar nginx:latest   # 导出镜像为tar文件
docker load -i nginx.tar                # 从tar文件导入镜像
```

---

### **二、容器操作**

### 1. **启动与停止**

```
docker run -d -p 8080:80 --name my-nginx nginx  # 后台运行容器（映射端口80→8080）
docker start my-nginx               # 启动已停止的容器
docker stop my-nginx                # 优雅停止容器（发送SIGTERM）
docker restart my-nginx             # 重启容器
docker rm -f my-nginx               # 强制删除运行中的容器
```

### 2. **查看与进入容器**

```
docker ps                          # 查看运行中的容器
docker ps -a                       # 查看所有容器（包括已停止的）
docker logs -f my-nginx            # 实时查看容器日志（类似tail -f）
docker exec -it my-nginx /bin/bash # 进入容器的交互式终端
docker stats my-nginx              # 查看容器资源占用（CPU/内存）
```

### 3. **限制资源**

```
docker run -it --cpus=0.5 --memory=512m ubuntu  # 限制CPU和内存
docker update --memory=1g my-nginx    # 动态调整运行中容器的内存限制
```

---

### **三、网络管理**

### 1. **基础网络操作**

```
docker network ls                  # 查看所有网络
docker network create my-net       # 创建自定义网络
docker run --network=my-net nginx  # 指定容器加入自定义网络
```

### 2. **端口与连接测试**

```
docker port my-nginx               # 查看容器的端口映射
docker run --rm curlimages/curl curl http://my-nginx  # 临时容器测试网络连通性
```

---

### **四、数据卷与存储**

### 1. **数据卷操作**

```
docker volume create my-vol        # 创建数据卷
docker run -v my-vol:/data nginx   # 挂载数据卷到容器
docker volume inspect my-vol       # 查看数据卷详情
```

### 2. **绑定宿主机目录**

```
docker run -v /host/path:/container/path nginx  # 挂载宿主机目录
```

---

### **五、Docker Compose 常用命令**

```
docker-compose up -d               # 启动所有服务（后台运行）
docker-compose down                # 停止并删除所有容器
docker-compose logs -f service1    # 查看指定服务的日志
docker-compose ps                  # 查看服务状态
docker-compose restart service1    # 重启单个服务
```

---

### **六、系统与清理**

### 1. **资源监控**

```
docker system df                  # 查看磁盘占用（镜像、容器、卷）
docker top my-nginx                # 查看容器内进程
```

### 2. **清理无用资源**

```
docker system prune               # 删除所有未使用的镜像、容器、网络
docker system prune -a            # 额外删除未被任何容器引用的镜像
```

---

### **七、常用场景示例**

### 1. **快速测试MySQL**

```
docker run -d --name mysql -e MYSQL_ROOT_PASSWORD=123456 -p 3306:3306 mysql:8.0
```

### 2. **备份容器数据**

```
docker run --rm --volumes-from my-nginx -v $(pwd):/backup ubuntu tar cvf /backup/nginx_data.tar /data
```

### 3. **调试容器DNS**

```
docker run --rm busybox nslookup google.com
```

---

### **总结**

- **镜像管理**：`pull`、`build`、`rmi`
- **容器操作**：`run`、`exec`、`logs`、`rm`
- **网络存储**：`network`、`volume`
- **生产工具**：`docker-compose`、`system prune`

建议搭配 **别名（alias）** 提升效率（如 `alias dps='docker ps --format "table {{.ID}}\t{{.Names}}\t{{.Status}}\t{{.Ports}}"'`）。