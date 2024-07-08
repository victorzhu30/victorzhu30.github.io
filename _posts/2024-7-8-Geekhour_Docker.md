---
layout: post
title: "Geekhour Docker学习笔记"
date:   2024-7-8
tags: [notice]
comments: true
author: VictorZhu
---

## Docker

应用隔离、环境配置、安装部署、持续集成、持续发布、DevOps
![](2024-07-07-19-33-09.png)

### Docker简介
Docker是一个用于构建(build)、运行(run)和传送(share)应用程序的平台。\
有了Docker，我们就可以将应用程序和它运行时所需要的各种依赖包、第三方软件库和配置文件等打包在一起，以便在任何环境中都可以正确地运行。
![](2024-07-08-13-20-48.png)

#### 为什么要使用Docker
![](2024-07-08-13-22-51.png)
![](2024-07-08-13-23-11.png)

#### Docker和虚拟机的区别
![](2024-07-08-13-24-12.png)
这是通过一种叫做虚拟化的技术来实现的，虚拟化技术是一种将物理资源虚拟为多个逻辑资源的技术，它可以将一台物理服务器虚拟成多个逻辑服务器，每个逻辑服务器都有自己的操作系统、CPU、内存、硬盘和网络接口等。它们之间是完全隔离的，可以独立运行。虚拟机在一定程度上实现了资源的整合，可以将一台服务器的计算能力、存储能力、网络资源分配给多个逻辑服务器，实现多台服务器的功能。
![](2024-07-08-13-31-46.png)
**缺点：** 每台虚拟机都需要占用大量的资源，启动速度非常慢
![](2024-07-08-13-33-01.png)

Docker和容器是两个不同的概念。Docker是容器的一种实现，是一个容器化的解决方案和平台。

容器是一种虚拟化技术，与虚拟机类似，也是一个独立的环境，可以在这个环境中运行应用程序。和虚拟机不同的是，它并不需要一个完整的操作系统，而是使用宿主机的操作系统，所以启动速度非常快。同时因为需要的资源更少，可以在一台物理服务器上运行更多的容器。
![](2024-07-08-13-34-20.png)


1. **虚拟机（Virtual Machine）**：
   - 虚拟机运行在一个完整的虚拟操作系统上，每个虚拟机都有自己的操作系统内核和系统资源。这通常会带来较高的资源开销。
   - 虚拟机通过一个虚拟机管理程序（Hypervisor，如 VMware、VirtualBox）来管理和分配硬件资源。

2. **容器（Container）**：
   - 容器共享宿主操作系统的内核，但隔离了用户空间。这意味着每个容器包含应用程序及其所有依赖项，但不包含独立的操作系统内核。
   - 容器使用更少的资源，因为它们不需要完整的操作系统，启动速度非常快，接近于直接运行在宿主操作系统上。
   - 容器化技术如 Docker 利用了 Linux 内核的功能，如 cgroups 和 namespaces，实现资源隔离和限制。

### 基本原理和概念
![](2024-07-08-13-41-44.png)
镜像是一个只读的模板，它可以用来创建容器。容器是Docker的运行实例，它提供了一个独立的可移植的环境，可以在这个环境中运行应用程序。

镜像和容器的关系就像Java中类和实例的关系一样。

Docker仓库是用来存储Docker镜像的地方，最流行和最常用的仓库就是Dockerhub。它是一个公共的Docker仓库，用来集中存储和管理Docker镜像。我们可以在这里下载各种镜像，也可以将自己的镜像上传到这里，这样就可以实现镜像的共享和复用。

在终端或命令行中输入`docker version`可以看到Docker的版本信息。如果只能看到Client，说明Docker没有启动，只有启动Docker之后才能看到Server

### Docker的体系架构
Docker使用Client-Server架构模式。Docker Daemon和Docker Client之间通过Socket或者RESTful API进行通信。

Docker 的体系架构主要包括以下几个组件：

1. **Docker Daemon (Docker守护进程)**：
   - 这是 Docker 的后台服务，负责管理容器。它监听 Docker API 请求并管理 Docker 对象（如图像、容器、网络和卷）。Docker Daemon 可以运行在你的本地计算机上，也可以运行在远程服务器上。

2. **Docker Client (Docker CLI, Docker客户端)**：
   - 这是用户与 Docker 交互的主要工具。用户通过 Docker 客户端发送命令到 Docker Daemon。

3. **Docker Registry (Docker注册表)**：
   - Docker Registry 是存储 Docker 镜像的地方。Docker Hub 是一个公共的注册表，任何人都可以使用它来存储和获取镜像。你也可以设置私有的注册表来存储自己的镜像。

4. **Docker Image (Docker镜像)**：
   - Docker 镜像是一个只读模板，包含了运行应用程序所需的所有内容（如代码、运行时、库和配置文件）。镜像可以从 Docker Registry 拉取，并用来创建 Docker 容器。

5. **Docker Container (Docker容器)**：
   - 容器是 Docker 镜像的运行实例。它们是轻量级、独立的执行环境，与主机系统共享操作系统内核，但彼此之间是隔离的。容器可以启动、停止、移动和删除，且每个容器都是独立的。

6. **Dockerfile**：
   - Dockerfile 是一个文本文件，包含了构建 Docker 镜像的所有命令。通过 Dockerfile，你可以定义镜像的内容和启动时的配置。

7. **Docker Compose**：
   - Docker Compose 是一个用于定义和运行多容器 Docker 应用的工具。你可以使用 YAML 文件来配置应用所需的服务，并通过一个命令启动所有服务。

下图展示了 Docker 的基本架构：

```
+---------------------+
|    Docker Client    |
+---------------------+
           |
           v
+---------------------+
|    Docker Daemon    |
|  (docker Engine)    |
+---------------------+
           |
           v
+---------------------+
|   Docker Containers |
|  (Running instances)|
+---------------------+
           |
           v
+---------------------+
|   Docker Images     |
|  (Templates)        |
+---------------------+
           |
           v
+---------------------+
|  Docker Registry    |
|  (Image storage)    |
+---------------------+
```

通过上述架构，Docker 提供了一个高效的方式来管理和部署应用程序，使得应用程序的开发和运维更加灵活和高效。

#### Docker Desktop应用程序

Docker Desktop 是一个 GUI 应用程序，提供了图形界面来管理 Docker 容器和镜像。它集成了 Docker Daemon、Docker CLI 和 Docker Compose，使得管理容器化应用变得简单直观。

1. **Docker Daemon (Docker守护进程)**：
   - 在 Docker Desktop 中，Docker Daemon 运行在后台，负责管理容器的创建、运行和删除等操作。对于 Windows 系统，Docker Desktop 使用一个轻量级的 Linux 虚拟机（WSL 2 或 Hyper-V）来运行 Docker Daemon。

2. **Docker CLI (命令行界面)**：
   - Docker CLI 是与 Docker 交互的主要工具。你可以通过终端或命令提示符运行 `docker` 命令来管理镜像、容器、网络和卷。例如，命令 `docker run` 可以用来启动一个新容器。

3. **Docker Compose**：
   - Docker Compose 是一个用于定义和运行多容器 Docker 应用的工具。它允许你使用一个 `docker-compose.yml` 文件来定义应用的服务，并通过一个命令启动所有服务。

4. **Docker Hub**：
   - Docker Hub 是一个公共的 Docker 镜像注册表，用户可以从这里拉取镜像或将自己的镜像推送到这里。Docker Desktop 默认连接到 Docker Hub，但你也可以配置它连接到私有的注册表。

#### Docker Desktop 工作流程

1. **安装 Docker Desktop**：
   - 从 Docker 官方网站下载并安装 Docker Desktop。安装完成后，启动 Docker Desktop 应用程序，Docker Daemon 会在后台运行。

2. **拉取镜像**：
   - 使用 Docker CLI 或 Docker Desktop GUI 从 Docker Hub 拉取所需的镜像。例如，使用命令 `docker pull ubuntu` 拉取 Ubuntu 镜像。

3. **运行容器**：
   - 使用 Docker CLI 运行容器。例如，使用命令 `docker run -it ubuntu` 启动一个交互式的 Ubuntu 容器。在 Docker Desktop 的 GUI 中，你也可以看到所有运行中的容器，并对其进行管理。

4. **管理容器和镜像**：
   - Docker Desktop 提供了一个直观的界面来查看和管理本地的容器和镜像。你可以在 GUI 中启动、停止、删除容器，以及拉取和删除镜像。

5. **使用 Docker Compose**：
   - 创建一个 `docker-compose.yml` 文件，定义你的多容器应用。使用命令 `docker-compose up` 启动所有服务。Docker Desktop 的 GUI 也会显示通过 Docker Compose 启动的容器。

#### 示例 `docker-compose.yml` 文件

下面是一个简单的 `docker-compose.yml` 示例，用于启动一个包含两个服务（web 和 redis）的应用：

```yaml
version: '3'
services:
  web:
    image: nginx
    ports:
      - "80:80"
  redis:
    image: redis
```

使用 `docker-compose up` 命令，你可以启动这个应用，Nginx 服务将运行在端口 80，Redis 服务则在后台运行。

#### Docker Register和Repository

##### Docker Registry

Docker Registry 是一个存储和分发 Docker 镜像的服务。它可以被看作是一个镜像的仓库管理系统。官方的 Docker Hub 就是一个公共的 Docker Registry，你也可以搭建自己的私有 Docker Registry 来存储和分发自己的镜像。

主要功能包括：
- 存储 Docker 镜像。
- 处理用户的拉取和推送请求。
- 提供镜像的版本管理。

常见的 Docker Registry 有：
- **Docker Hub**：官方公共镜像存储服务。
- **Azure Container Registry**：微软提供的容器镜像服务。
- **Amazon Elastic Container Registry (ECR)**：亚马逊提供的容器镜像服务。
- **私有 Docker Registry**：可以在自己的服务器上搭建私有镜像存储服务。

##### Docker Repository

Docker Repository 是 Docker Registry 中的一个逻辑集合，包含了一组相关的镜像。通常，一个仓库会包含一个软件的多个版本，每个版本对应一个镜像。

例如，在 Docker Hub 上，`nginx` 仓库可能包含多个版本的 Nginx 镜像，如 `nginx:latest`、`nginx:1.19`、`nginx:1.18` 等等。每个版本都是一个独立的镜像，但它们都属于同一个仓库。

##### 关系与区别

- **Registry 是管理和存储 Docker Repository 的服务**。一个 Registry 中可以包含多个 Repository。
- **Repository 是具体的软件集合**，包含该软件的多个版本的镜像。

##### 示例

假设你在 Docker Hub 上有一个名为 `myapp` 的仓库，它包含了不同版本的 `myapp` 镜像：

- `myapp:latest`
- `myapp:1.0`
- `myapp:1.1`

这些镜像都存储在 Docker Hub 这个 Registry 中，但它们属于同一个 Repository `myapp`。

##### 操作示例

1. **推送镜像到 Docker Registry**：
   ```sh
   docker tag myapp:1.0 mydockerhubusername/myapp:1.0
   docker push mydockerhubusername/myapp:1.0
   ```

2. **从 Docker Registry 拉取镜像**：
   ```sh
   docker pull mydockerhubusername/myapp:1.0
   ```

3. **列出本地镜像**：
   ```sh
   docker images
   ```

4. **运行容器**：
   ```sh
   docker run -d mydockerhubusername/myapp:1.0
   ```

### 容器化和Dockerfile
容器化顾名思义就是将应用程序打包成容器，然后再容器中运行应用程序的过程。这个过程简单来说可以分成三个步骤：
- 首先需要创建一个Dockerfile，来告诉Docker构建应用程序镜像所需要的步骤和配置。
- 然后使用Dockerfile来构建镜像
- 接下来就可以使用这个镜像来创建和运行容器
![](2024-07-08-19-02-41.png)

Dockfile是一个文本文件，里面包含了一条条的指令，用来告诉Docker如何来构建镜像。一般来说包括下面这些内容：
- 精简版的操作系统，比如Alpine
- 应用程序的运行时环境，比如Node.js，Java，Python
- 应用程序，比如SpringBoot打包好的jar包
- 应用程序的第三方依赖库或者包
- 应用程序的配置文件，环境变量

一般来说我们会在项目的根目录下创建一个叫Dockerfile的文件，在这个文件中写入构建镜像所需要的各种指令之后，Docker就会根据这个Dockerfile文件来构建一个镜像。

### 实践环节
Node.js是一个运行环境，它可以让我们在浏览器之外的地方运行JavaScript代码。Node.js和JavaScript的关系就像Java和JRE的关系一样。
![](2024-07-08-20-26-16.png)

我们需要先指定一个基础镜像，镜像是按层次结构来构建的，每一层都是基于上一层的。我们可以从一个最基础的Linux镜像开始，然后在这个镜像基础上安装Node.js和我们的应用程序，或者我们也可以直接使用Node.js的镜像，因为这个镜像已经是基于Linux来构建的了。

查看 Windows 上安装的 Node.js 版本:按 Win + R，输入 cmd 或 powershell，然后按回车来打开，输入以下命令并按回车：
```sh
node -v
```
```sh
node --version
```
这将显示你安装的 Node.js 的版本号。例如，如果你安装的是 Node.js 14.17.0，输出将会是：
```sh
v14.17.0
```

E.g.
```dockerfile
FROM node:21.4-alpine
这行指定了基础镜像为 node:21.4-alpine。
alpine和RedHat，CentOS等都是Linux发行版。
node:21.4-alpine 是一个轻量级的 Alpine Linux 镜像，包含 Node.js 21.4 版本。
WORKDIR /app
这行设置容器内部的工作目录为 /app。之后的命令都将在这个目录下执行。
COPY public/ /app/public
COPY src/ /app/src
COPY package.json /app
这些命令将你的本地文件和目录复制到容器内的 /app 目录。
具体来说，将 public 目录复制到 /app/public，将 src 目录复制到 /app/src，将 package.json 文件复制到 /app。
RUN npm install
这行在容器内运行 npm install 命令，根据 package.json 文件安装所有的依赖包。
CMD ["npm", "start"]
这行指定了容器启动时运行的命令，即 npm start。这通常会运行你的应用程序。
```

#### 构建镜像

在 HelloDocker 文件夹中运行 `docker build -t hello-docker .` 命令：

```sh
cd HelloDocker
docker build -t hello-docker .
```

- **cd HelloDocker**：切换到 HelloDocker 文件夹。
- **docker build -t hello-docker .**：在当前目录（`.`）中查找 Dockerfile，构建镜像并命名为 `hello-docker`。

#### 镜像存储
构建的 Docker 镜像会存储在 Docker 引擎的本地镜像库中，而不是在文件系统的特定目录下。你可以使用 Docker 命令来查看和管理这些镜像。

#### 查看构建的镜像

你可以使用以下命令查看你本地的所有 Docker 镜像，包括刚刚构建的 `hello-docker` 镜像：

```sh
docker images
```

这个命令会列出所有在本地存储的 Docker 镜像。例如：

```sh
REPOSITORY        TAG       IMAGE ID       CREATED          SIZE
hello-docker      latest    98fd150074fd   10 minutes ago   123MB
node              20.11.1   bf77dc26e48e   2 weeks ago      90MB
```

#### 运行构建的镜像

构建镜像之后，你可以使用 `docker run` 命令来运行这个镜像。例如：

```sh
docker run -d -p 3000:3000 --name my-hello-docker hello-docker
```

解释一下这个命令：

- **-d**：在后台运行容器。
- **-p 3000:3000**：将容器的端口 3000 映射到主机的端口 3000。
- **--name my-hello-docker**：为容器指定一个名称 `my-hello-docker`。
- **hello-docker**：使用 `hello-docker` 镜像来创建容器。

#### 重新构建镜像
当修改 `Dockerfile` 或项目文件（如 `index.js`）之后，重新构建镜像时，会创建一个新的镜像，并覆盖之前的镜像（如果你使用相同的标签名）。具体来说，当你运行 `docker build` 命令并使用相同的镜像标签名时，Docker 会生成一个新的镜像，并且使用该标签指向新的镜像。

##### 注意事项

- **构建时间**：每次构建镜像时，Docker 都会从头开始处理 `Dockerfile` 中的每一步。这意味着如果基础镜像没有缓存，可能需要一些时间来下载和解压。
- **标签管理：如果你希望保留旧版本的镜像，可以使用不同的标签名（例如，`hello-docker:v1` 和 `hello-docker:v2`），而不是覆盖现有的标签。**
- **清理旧镜像**：如果你不再需要旧的镜像，可以使用以下命令来删除它们：
  ```sh
  docker image rm [IMAGE_ID]
  ```

#### 悬空镜像
你看到 `<none>` 的镜像表示这是一个悬空镜像（dangling image）。这种情况通常发生在你重新构建镜像时，新的镜像使用了相同的标签名，而旧的镜像被解除关联，但仍然存在于本地存储中。悬空镜像没有任何标签引用，因此显示为 `<none>`。

##### 处理悬空镜像

你可以选择清理这些悬空镜像，以释放磁盘空间。以下是一些常用的 Docker 命令来管理和清理悬空镜像：

###### 列出所有悬空镜像

```sh
docker images -f "dangling=true"
```

这将列出所有悬空镜像。

###### 删除所有悬空镜像

你可以使用以下命令删除所有悬空镜像：

```sh
docker image prune
```

这个命令将删除**所有没有被任何标签引用的**悬空镜像。执行此命令前，Docker 会提示你确认删除操作。

###### 删除特定的悬空镜像

如果你想只删除特定的悬空镜像，可以使用以下命令：

```sh
docker rmi [IMAGE_ID]
```

例如：

```sh
docker rmi 98fd150074fd
```

#### 查看容器
要查看正在运行的 Docker 容器，可以使用 `docker ps` 命令。这个命令会列出所有当前正在运行的容器。

##### 查看正在运行的容器

```sh
docker ps
```

这个命令将显示一个包含正在运行的容器的信息表，包括容器 ID、镜像、创建时间、状态、端口映射等。

###### 示例输出

```sh
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS          PORTS                    NAMES
c3fb8a6a7b3e   hello-docker   "node /index.js"         2 minutes ago    Up 2 minutes    0.0.0.0:3000->3000/tcp   blissful_galois
```

##### 查看所有容器（包括停止的）

如果你想查看所有容器，包括已经停止的，可以使用 `-a` 选项：

```sh
docker ps -a
```

###### 示例输出

```sh
CONTAINER ID   IMAGE          COMMAND                  CREATED          STATUS                      PORTS                    NAMES
c3fb8a6a7b3e   hello-docker   "node /index.js"         2 minutes ago    Up 2 minutes                0.0.0.0:3000->3000/tcp   blissful_galois
a9f8e6b7d8c9   hello-docker   "node /index.js"         3 minutes ago    Exited (0) 2 minutes ago                              relaxed_hopper
```

##### 进一步操作

###### 停止正在运行的容器

如果你需要停止一个正在运行的容器，可以使用以下命令：

```sh
docker stop [CONTAINER_ID]
```

例如：

```sh
docker stop c3fb8a6a7b3e
```

###### 删除停止的容器

删除一个停止的容器可以使用以下命令：

```sh
docker rm [CONTAINER_ID]
```

例如：

```sh
docker rm a9f8e6b7d8c9
```

###### 删除所有停止的容器

如果你想删除所有已经停止的容器，可以使用以下命令：

```sh
docker container prune
```

#### 将本地生成的镜像上传到Docker Hub
将你本地生成的 `hello-docker` 镜像上传到 Docker Hub，可以按照以下步骤进行：

##### 前提条件

1. **Docker Hub 账号**：你需要有一个 Docker Hub 账号。如果没有，可以在 [Docker Hub 官网](https://hub.docker.com/) 免费注册一个。
2. **Docker CLI 登录**：你需要通过 Docker CLI 登录到 Docker Hub。

##### 步骤

###### 1. 登录 Docker Hub

在命令行中运行以下命令，并根据提示输入你的 Docker Hub 用户名和密码：

```sh
docker login
```

###### 2. 为镜像打标签

Docker Hub 上的镜像名称格式为 `username/repository:tag`。为你的 `hello-docker` 镜像打标签，例如你的 Docker Hub 用户名是 `victorzhu030`：

```sh
docker tag hello-docker victorzhu1030/hello-docker:latest
```

###### 3. 推送镜像到 Docker Hub

使用 `docker push` 命令将打标签的镜像推送到 Docker Hub：

```sh
docker push victorzhu1030/hello-docker:latest
```

##### 查看推送的镜像

推送完成后，你可以在 Docker Hub 的个人页面上查看到你推送的 `hello-docker` 镜像。

##### 总结

1. **登录 Docker Hub**：`docker login`
2. **为镜像打标签**：`docker tag hello-docker victor/hello-docker:latest`
3. **推送镜像到 Docker Hub**：`docker push victor/hello-docker:latest`

通过这些步骤，你可以将本地的 Docker 镜像上传到 Docker Hub，方便在其他地方拉取和使用。

### play-with-docker
[play-with-docker的网址](https://labs.play-with-docker.com/)

这个错误信息表明你在尝试从Docker Hub拉取`geekhour/hello-docker`镜像时，已经达到了未认证用户的拉取速率限制。Docker Hub对未认证用户和免费账户的拉取速率进行了限制，以控制资源的使用。

具体含义如下：
1. **toomanyrequests**: 这是错误码，表示你的请求过多，超过了允许的速率。
2. **You have reached your pull rate limit**: 你已经达到了允许的拉取速率限制。
3. **You may increase the limit by authenticating and upgrading**: 你可以通过认证（登录）和升级账户来提高拉取速率限制。

要解决这个问题，可以考虑以下几个方法：

1. **登录Docker Hub账户**：通过认证可以提高拉取速率限制。你可以在终端中运行以下命令登录Docker Hub：
   ```sh
   docker login
   ```
   然后按照提示输入Docker Hub的用户名和密码。

2. **升级Docker Hub账户**：如果你需要更高的拉取速率，可以考虑升级到Docker Hub的付费计划，具体信息可以参考[Docker Hub订阅计划](https://www.docker.com/increase-rate-limit)。

3. **尝试在非高峰时段拉取镜像**：有时候在高峰时段请求量大，会更容易达到速率限制，可以尝试在其他时段再拉取镜像。

4. **使用镜像缓存或私有注册表**：如果你频繁使用某些镜像，可以考虑使用镜像缓存或者设置私有的Docker注册表，以减少对Docker Hub的请求。

### Docker 的优点

- **轻量级**：由于容器共享宿主操作系统的内核，不需要为每个容器加载一个独立的操作系统，资源开销较小。
- **启动速度快**：容器的启动不需要初始化一个完整的操作系统，因此启动时间通常是秒级甚至毫秒级的。
- **高密度**：因为容器占用的资源较少，一台物理服务器或虚拟机上可以运行更多的容器，从而提高资源利用率。
- **一致性**：容器打包了应用程序及其所有依赖项，确保在不同环境中运行一致性，不再有“在我的机器上可以运行”的问题。
- **隔离性**：每个容器在运行时相互隔离，这提供了一个安全、独立的环境，减少了应用程序之间的冲突。

