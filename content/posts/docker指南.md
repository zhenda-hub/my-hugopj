+++
title = 'Docker指南'
subtitle = ""
date = 2024-01-09T00:04:35+08:00
draft = true
toc = true
tags = ["docker", "tools"]
+++

[toc]

## 官方文档

-   <https://docs.docker.com/get-started/>
-   <https://docs.docker.com/develop/dev-best-practices/>
-   <https://docs.docker.com/develop/develop-images/instructions/>
-   <https://docs.docker.com/storage/>
-   <https://docs.docker.com/compose/compose-file/05-services/#simple-example>

## 相关教程

-   <https://juejin.cn/post/7154437479955693598>

## docker commend

```bash
docker --help
```

```bash
Management Commands:
  builder     Manage builds
  buildx*     Docker Buildx
  compose*    Docker Compose
  container   Manage containers
  context     Manage contexts
  debug*      Get a shell into any image or container
  desktop*    Docker Desktop commands (Alpha)
  dev*        Docker Dev Environments
  extension*  Manages Docker extensions
  feedback*   Provide feedback, right in your terminal!
  image       Manage images
  init*       Creates Docker-related starter files for your project
  manifest    Manage Docker image manifests and manifest lists
  network     Manage networks
  plugin      Manage plugins
  sbom*       View the packaged-based Software Bill Of Materials (SBOM) for an image
  scout*      Docker Scout
  system      Manage Docker
  trust       Manage trust on Docker images
  volume      Manage volumes
```

```bash
docker compose --help
```

```bash
Usage:  docker compose [OPTIONS] COMMAND

Define and run multi-container applications with Docker

Options:
      --all-resources              Include all resources, even those not used by services
      --ansi string                Control when to print ANSI control characters ("never"|"always"|"auto") (default "auto")
      --compatibility              Run compose in backward compatibility mode
      --dry-run                    Execute command in dry run mode
      --env-file stringArray       Specify an alternate environment file
  -f, --file stringArray           Compose configuration files
      --parallel int               Control max parallelism, -1 for unlimited (default -1)
      --profile stringArray        Specify a profile to enable
      --progress string            Set type of progress output (auto, tty, plain, quiet) (default "auto")
      --project-directory string   Specify an alternate working directory
                                   (default: the path of the, first specified, Compose file)
  -p, --project-name string        Project name

Commands:
  attach      Attach local standard input, output, and error streams to a service's running container
  build       Build or rebuild services
  config      Parse, resolve and render compose file in canonical format
  cp          Copy files/folders between a service container and the local filesystem
  create      Creates containers for a service
  down        Stop and remove containers, networks
  events      Receive real time events from containers
  exec        Execute a command in a running container
  images      List images used by the created containers
  kill        Force stop service containers
  logs        View output from containers
  ls          List running compose projects
  pause       Pause services
  port        Print the public port for a port binding
  ps          List containers
  pull        Pull service images
  push        Push service images
  restart     Restart service containers
  rm          Removes stopped service containers
  run         Run a one-off command on a service
  scale       Scale services 
  start       Start services
  stats       Display a live stream of container(s) resource usage statistics
  stop        Stop services
  top         Display the running processes
  unpause     Unpause services
  up          Create and start containers
  version     Show the Docker Compose version information
  wait        Block until the first service container stops
  watch       Watch build context for service and rebuild/refresh containers when files are updated

Run 'docker compose COMMAND --help' for more information on a command.
```

## single container applications

### 操作方法

1. 可视化工具

- docker desktop
- portainer

2. 常用命令

```bash
# 创建 context
docker context create desktop-linux --description "Docker Desktop" --docker "host=unix:///home/YOUR_USER_NAME/.docker/desktop/docker.sock"

# image
docker images
docker pull username/image_name:tag_version
docker push username/image_name:tag_version
docker rmi image_id

docker built -t username/image_name:tag_version .
docker tag old_image_name new_image_name

# container
docker ps
docker ps -a
docker run -d -p local_ip:container_ip username/image_name:tag_version
docker rm container_id

# volume
docker run -d -v volume_name:container_path username/image_name:tag_version
docker run -d -v local_path:container_path username/image_name:tag_version

# exec, exit退出此模式
docker ps
docker exec -it container_id /bin/bash
```

### 实战示例

python项目的Dockerfile

```Dockerfile
FROM python:3

WORKDIR /usr/src/app

COPY requirements.txt ./
RUN pip install --no-cache-dir -r requirements.txt

COPY . .

CMD [ "python", "./your-daemon-or-script.py" ]
# 或者调用sh文件,执行一系列命令
# CMD [ "bash", "./script.sh" ]  
```

CMD 是容器的默认启动命令

有了Dockerfile后 需要build, run

```bash
docker built -t username/image_name:tag_version .
docker run -d -p local_ip:container_ip username/image_name:tag_version
```


## Docker Compose


1. 创建 compose.yaml

```yaml
services:
  redis:
    image: redis
    volumes:
      - redis-data:/data
    ports:
      - '6379:6379'
  web1:
    restart: on-failure
    build: ./web
    hostname: web1
    ports:
      - '81:5000'
    develop:
      watch:
        - path: ./backend/src
          action: sync
          target: /usr/local/app/src
        - path: ./backend/package.json
          action: rebuild
  web2:
    restart: on-failure
    build: ./web
    hostname: web2
    ports:
      - '82:5000'
    develop:
      watch:
        - path: ./backend/src
          action: sync
          target: /usr/local/app/src
        - path: ./backend/package.json
          action: rebuild
  nginx:
    build: ./nginx
    ports:
    - '80:80'
    depends_on:
    - web1
    - web2
volumes:
  redis-data:
```

command 覆盖 Dockerfile 中的 CMD，适用于特定服务的启动配置。


难点:

- 容器路径的绑定
- 数据保存在volume
- 环境变量的设置

2. 常用命令

```bash
docker compose --help
# 生成image, container
docker compose up --build
# 生成image, container, 后台运行
docker compose up --build -d
# 停止container
docker compose stop
# 销毁container
docker compose down
# 开发环境,查看实时变化, 需要在compose.yaml添加 develop, watch
docker compose watch
# 交互调试, exit退出此模式
docker compose exec service_name /bin/bash
```

## Container Networking

如果两个容器位于同一网络上，它们可以相互通信。如果他们不是，他们就不能

## 完整测试平台

<https://labs.play-with-docker.com/>

可以在这个平台测试镜像是否符合预期


## 项目目录结构推荐

```bash
myproject/
│
├── .git/                 # Git repository metadata
├── .gitignore            # Git ignore file
├── .dockerignore         # Docker ignore file
├── docker-compose.yml    # Docker Compose configuration file
├── web/                  # Web服务（如Django）的目录
│   ├── Dockerfile        # Web服务的Dockerfile
│   ├── requirements.txt  # Python依赖文件
│   ├── manage.py         # Django管理脚本
│   └── myproject/        # Django项目目录
│       ├── __init__.py
│       ├── settings.py
│       ├── urls.py
│       └── wsgi.py
├── db/                   # 数据库服务的目录（可选）
│   ├── Dockerfile        # 数据库服务的Dockerfile（如自定义配置）
│   └── init.sql          # 初始化数据库的SQL脚本（可选）
└── nginx/                # Nginx服务的目录（可选）
    ├── Dockerfile        # Nginx服务的Dockerfile（如自定义配置）
    └── nginx.conf        # Nginx配置文件

```