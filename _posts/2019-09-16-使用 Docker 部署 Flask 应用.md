---
layout: post
title: '使用 Docker 部署 Flask 应用'
subtitle: '一次简单的 Docker 实践'
date: 2019-09-16
categories: 技术
cover: '../assets/img/how-to-deploy-java-apps-with-docker-a-quick-tutorial@3x-1560x760.png'
tags: Docker 技术
---

## 简介

最近用 Flask 写了一个考试的题库系统，功能十分简单，甚至是简陋，不过本着“又不是不能用的精神”，还是用上了，直接拿着开发服务器在一台 Winodws 2008 上运行。问题也暴露出来，服务的运行不足够健壮。

说来，这也是第一个我开发的凑合能用的那么一个 Flask 应用程序，因此也尝试一下使用 Docker 来部署这个应用。

你可以在 GitHub 上看到整个项目的结构

https://github.com/Loner1024/QuestionBank

## 安装 Docker

安装 Docker 的方法可以参考这篇文章 http://1024.services/index.php/archives/20/，基本涵盖了主流的操作系统。

我自己并没有在服务器上去跑一个 Docker，而是只在本地进行了测试，因此本文所有的内容其实都是在 macOS 上进行的。

在 macOS 上安装 Docker 十分简单，我们只需要使用 Homebrew 就可以：

```shell
brew cask install docker
```

当然，你也可以去官网下载一个安装包来安装，不过 Homebrew 更方便。

安装过程可能需要输入密码，给它就好。

安装好后你可以在状态栏看到 Docker 的鲸鱼图标。

## 配置 Docker

国内的网络环境会导致 Docker 拉去镜像很慢，因此我们现在要做的唯一一项配置个工作就是为 Docker 配置镜像加速器。

国内有很多云服务厂商提供了镜像加速器服务，我这里列出两个，你也可以在网上寻找其他的。

Azure 中国镜像 `https://dockerhub.azk8s.cn`

科大镜像站 `https://docker.mirrors.ustc.edu.cn`

在任务栏点击 Docker Desktop 应用图标 -> Perferences... -> Daemon -> Registry mirrors。在列表中填写加速器地址 `https://dockerhub.azk8s.cn`。修改完成之后，点击 `Apply & Restart` 按钮，Docker 就会重启并应用配置的镜像地址了。

## 配置程序依赖

我们的 Python 程序一般都要依赖一些包和库，一个经典的做法是用一个 `requirements.txt` 来记录使用了那些依赖。

在这个程序中的 `requirements.txt  `文件内容如下：

```
flask
flask-wtf
flask-migrate
flask-login
Flask-SQLAlchemy
gunicorn
gevent
```

运行如下命令安装依赖：

```
pip install -r requirements.txt
```

## 配置 Gunicorn

首先需要安装 Gunicorn 和 gevent

```shell
pip install gunicorn gevent
```

这两个库是什么作用这里不再赘述。

然后在项目最上层的目录新建一个 `gunicorn.conf.py`文件，并填入如下内容：

```python
workers = 5    # 定义同时开启的处理请求的进程数量，根据网站流量适当调整
worker_class = "gevent"   # 采用gevent库，支持异步处理请求，提高吞吐量
bind = "0.0.0.0:80"  # ip和端口
```

然后我们可以通过 gunicorn 命令来测试是否可以正确运行

```bash
gunicorn app:app -c gunicorn.conf.py
```

## 配置 Dockerfile

项目最上层的目录新建一个 `Dockerfile`文件，并填入如下内容：

```dockerfile
FROM python:3.7
WORKDIR /demo
COPY requirements.txt ./
RUN pip install -r requirements.txt -i https://pypi.tuna.tsinghua.edu.cn/simple

COPY . .

CMD ["gunicorn", "app:app", "-c", "./gunicorn.conf.py"]
```

这个文件的内容，差不离应该能看懂在做什么事情，至于具体的 Dockerfile 文件应该怎么写，不是这里要说的。

## 构建 Docker 镜像

只需要一行命令就可以构建 Docker 镜像：

```shell
docker build -t 'questionbank' .
```

注意：**Docker 镜像名称不能使用大写字母**，以及，**不要忘记命令后面的点**

然后就是等待。

镜像构建完成后，你可以用 `docker images`来查看镜像列表。

## 运行镜像

```shell
docker run -it --rm -p 80:80 questionbank
```

上面这条命令只能用于临时运行，因为它是显式的，一旦关闭命令窗口，也就停止运行了。

要使用在生产环境，我们可以这样：

```shell
docker run -d -p 80:80 --name test-questionbank questionbank
```

这样，你的服务就运行在后台了。

## 管理

要看到当前正在运行的镜像，我们可以使用这条命令：

```shell
docker container ls
```

终止状态的容器可以用 `docker container ls -a` 命令看到。

```shell
docker container start # 启动容器
docker container stop # 停止容器
docker container restart # 重启容器
```

注意，以上启动、停止、重启命令在后面都要带上`ls`出来的容器的 id，例如：`docker container stop 978a14e958ae`