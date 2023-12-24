# scrapyd

> # Scrapyd
>
> * 番茄时钟1：了解Scrapyd的概念和基本用法（预计25分钟）
>   在这个番茄钟中，你将学习Scrapyd的概念和基本用法，包括如何安装Scrapyd、如何创建一个Scrapyd项目、如何在Scrapyd中部署和运行爬虫、如何使用Scrapyd的API来管理和监控爬虫等。
> * 番茄时钟2：使用Scrapyd部署本地爬虫（预计45分钟）
>   在这个番茄钟中，你将学习如何使用Scrapyd部署本地爬虫。具体来说，你将学习如何将本地Scrapy爬虫打包成一个Scrapyd项目，并将其部署到本地Scrapyd服务器中运行。
> * 番茄时钟3：使用Scrapyd部署远程爬虫（预计45分钟）
>   在这个番茄钟中，你将学习如何使用Scrapyd部署远程爬虫。具体来说，你将学习如何将本地Scrapy爬虫打包成一个Scrapyd项目，并将其部署到远程Scrapyd服务器中运行。
> * 番茄时钟4：使用Scrapyd的API来管理和监控爬虫（预计45分钟）
>   在这个番茄钟中，你将学习如何使用Scrapyd的API来管理和监控爬虫。具体来说，你将学习如何使用Scrapyd的API来启动和停止爬虫、获取爬虫运行状态和日志等信息，并学习如何使用Scrapyd的web界面来管理和监控爬虫。
>
> ## ：了解Scrapyd的概念和基本用法
>
> ### Scrapyd部署和运行流程
>
> 首要条件是配置scrapy.cfg
>
> ```sql
> [settings]
> default = npr_english.settings
>
> [deploy:npr]     # npr是部署名
> url = http://localhost:6800/        # 如果scrapyd运行在远程服务器， 就改成远程服务器地址
> project = npr_eng  # npr_eng是项目名 
> ```
>
> 1. 1. 1. #### 1. 创建Scrapy项目
>
>          首先需要使用Scrapy框架创建一个新的爬虫项目。可以使用以下命令创建：
>
>          phpCopy code
>
>          ​`scrapy startproject <project_name>`​​
>
>          #### 2. 配置Scrapyd （可以省略用默认的）
>
>          在Scrapyd中部署Scrapy项目之前，需要先进行Scrapyd的配置。可以在Scrapyd配置文件`scrapyd.conf`​​中指定Scrapyd的监听地址和端口，以及允许访问Scrapyd的用户列表等。
>
>          #### 3. 安装Scrapyd-client
>
>          Scrapyd-client是Scrapyd的一个Python客户端，提供了一些命令行工具，方便管理和部署Scrapy项目。可以使用以下命令安装Scrapyd-client：
>
>          ​`pip install scrapyd-client`​​
>
>          #### 4. 部署Scrapy项目到Scrapyd
>
>          使用Scrapyd-client命令行工具将Scrapy项目部署到Scrapyd服务器上。可以使用以下命令：
>
>          ​`scrapyd-deploy <target> -p <project_name>`​​  # target 就是你的部署名
>
>          其中，`<target>`​​为部署目标，一般是在`scrapyd.conf`​​中定义的名称；`<project_name>`​​为Scrapy项目的名称。
>
>          #### 5. 运行Scrapy爬虫
>
>          部署成功后，可以使用Scrapyd提供的API接口运行Scrapy爬虫。可以使用以下命令：
>
>          phpCopy code
>
>          ​`curl http://<Scrapyd_host>:<Scrapyd_port>/schedule.json -d project=<project_name> -d spider=<spider_name>`​​
>
>          其中，`<Scrapyd_host>`​​和`<Scrapyd_port>`​​分别为Scrapyd的监听地址和端口；`<project_name>`​​和`<spider_name>`​​分别为Scrapy项目和爬虫的名称。
>
>          #### 6. 查看Scrapy爬虫运行状态
>
>          可以使用以下命令查看Scrapy爬虫的运行状态：
>
>          phpCopy code
>
>          ​`curl http://<Scrapyd_host>:<Scrapyd_port>/jobs/list.json?project=<project_name>`​​
>
>          其中，`<Scrapyd_host>`​​和`<Scrapyd_port>`​​分别为Scrapyd的监听地址和端口；`<project_name>`​​为Scrapy项目的名称。
>
>          #### 7. scrapy-client 常用命令
>
>          1. ​`scrapyd-deploy`​: 部署Scrapy项目到Scrapyd服务端。
>          2. ​`scrapyd-client`​: 调用Scrapyd服务端API。
>          3. ​`scrapyd-ctl`​: 启动或停止Scrapyd服务端。
>          4. ​`scrapyd-deploy [target]`​: 部署指定目标的Scrapy项目。
>          5. ​`scrapyd-deploy -l`​: 列出Scrapyd服务端上已有的Scrapy项目。
>          6. ​`scrapyd-deploy -v [target]`​: 显示详细的Scrapyd部署日志。
>          7. ​`scrapyd-client schedule [project] [spider]`​: 在Scrapyd服务端上调度运行指定的Scrapy爬虫。
>          8. ​`scrapyd-client listprojects`​: 列出Scrapyd服务端上已有的Scrapy项目。
>          9. ​`scrapyd-client listspiders [project]`​: 列出指定Scrapy项目下的所有爬虫。
>          10. ​`scrapyd-client cancel [jobid]`​: 取消指定的Scrapy爬虫任务。
>
> ### 
>
> ：安装Scrapyd并配置项目
>
> ## ：学习如何使用Scrapyd部署和管理爬虫项目
>
> 1. 创建项目
>
> 使用命令行工具，在需要创建项目的目录下执行以下命令：
>
> phpCopy code
>
> ​`scrapyd-deploy <target> -p <project_name>`​
>
> 其中，`<target>`​为目标部署名，可以为任意名称，用于区分不同的部署目标，例如本地部署、测试服务器部署、生产服务器部署等等。`<project_name>`​为项目名称，可以自定义。
>
> 2. 删除项目
>
> 使用Scrapyd API接口，可以删除指定的项目，命令为：
>
> bashCopy code
>
> ​`curl http://localhost:6800/delproject.json -d project=<project_name>`​
>
> 其中，`<project_name>`​为要删除的项目名称。
>
> 3. 列出项目
>
> 使用Scrapyd API接口，可以列出当前所有项目，命令为：
>
> bashCopy code
>
> ​`curl http://localhost:6800/listprojects.json`​
>
> 4. 列出版本
>
> 使用Scrapyd API接口，可以列出指定项目的所有版本，命令为：
>
> bashCopy code
>
> ​`curl http://localhost:6800/listversions.json -d project=<project_name>`​
>
> 其中，`<project_name>`​为要列出版本的项目名称。
>
> 5. 删除版本
>
> 使用Scrapyd API接口，可以删除指定项目的指定版本，命令为：
>
> bashCopy code
>
> ​`curl http://localhost:6800/delversion.json -d project=<project_name> -d version=<version>`​
>
> 其中，`<project_name>`​为要删除版本的项目名称，`<version>`​为要删除的版本号。
>
> 6. 启动爬虫
>
> 使用Scrapyd API接口，可以启动指定项目的指定爬虫，命令为：
>
> bashCopy code
>
> ​`curl http://localhost:6800/schedule.json -d project=<project_name> -d spider=<spider_name>`​
>
> 其中，`<project_name>`​为要启动爬虫的项目名称，`<spider_name>`​为要启动的爬虫名称。
>
> 7. 取消爬虫
>
> 使用Scrapyd API接口，可以取消指定项目的指定爬虫，命令为：
>
> bashCopy code
>
> ​`curl http://localhost:6800/cancel.json -d project=<project_name> -d job=<job_id>`​
>
> 其中，`<project_name>`​为要取消爬虫的项目名称，`<job_id>`​为要取消的爬虫任务ID。
>
> 以上是Scrapyd项目管理的一些常用操作，还有许多其他操作可以通过Scrapyd API接口进行。
>
> ## ：深入了解Scrapyd的高级特性和调试技巧
>
> 1. Scrapyd API：了解Scrapyd API的使用，包括通过API上传爬虫、部署爬虫、启动爬虫等功能，可以方便地对Scrapyd进行管理和监控。
> 2. Scrapyd Client：了解Scrapyd Client的使用，包括通过命令行工具对Scrapyd进行管理和监控，例如查看Scrapyd状态、列出Scrapyd上的项目、部署爬虫、启动爬虫等功能。
> 3. Scrapyd Web：了解Scrapyd Web的使用，可以方便地通过Web界面对Scrapyd进行管理和监控，例如查看Scrapyd状态、列出Scrapyd上的项目、部署爬虫、启动爬虫等功能。
> 4. Scrapyd Logs：了解Scrapyd Logs的使用，包括如何查看Scrapyd上爬虫的日志信息，以及如何在Scrapyd上设置日志级别和日志输出格式。
> 5. Scrapyd Settings：了解Scrapyd Settings的使用，包括如何在Scrapyd中配置各种设置，例如爬虫运行的并发数、下载延迟、下载超时等。
> 6. Scrapyd Deploy：了解Scrapyd Deploy的使用，包括如何使用Scrapyd Deploy工具部署爬虫到Scrapyd服务器上，并掌握Scrapyd Deploy的高级特性，例如如何指定Scrapyd服务器地址、如何使用SSH进行部署等。
> 7. Scrapyd Debug：了解Scrapyd Debug的使用，包括如何在Scrapyd中进行调试，例如如何在Scrapyd上查看代码运行过程中的变量和状态信息，如何使用pdb进行调试等。
> 8. Scrapyd Scale：了解Scrapyd Scale的使用，包括如何在多个Scrapyd节点之间进行负载均衡和任务分配，以及如何使用Docker进行Scrapyd集群部署等。
> 9. Scrapyd优化：了解Scrapyd的性能优化技巧，包括如何优化爬虫代码、如何设置Scrapyd的并发数、如何使用Scrapyd的内存管理等。
> 10. Scrapyd安全：了解Scrapyd的安全机制和防护技巧，包括如何防止恶意爬虫、如何设置Scrapyd的权限、如何防止Scrapyd被攻击等。
>
> ## 使用Nginx反向代理配置scrapyd集群
>
> ```nginx
> 安装Nginx
> 在您的服务器上安装Nginx，可以通过包管理器进行安装，例如在Ubuntu上使用以下命令：
>
> sql
> Copy code
> sudo apt-get update
> sudo apt-get install nginx
> 配置Nginx
> 打开Nginx配置文件/etc/nginx/nginx.conf，并添加以下内容：
>
> css
> Copy code
> http {
>     upstream scrapyd {
>         server 192.168.10.2:6800;
>         server 192.168.10.3:6800;
>         server 192.168.10.4:6800;
>         server 192.168.10.5:6800;
>         server 192.168.10.6:6800;
>     }
>
>     server {
>         listen 80;
>         server_name scrapyd.example.com;
>         location / {
>             proxy_pass http://scrapyd;
>             proxy_set_header Host $host;
>             proxy_set_header X-Real-IP $remote_addr;
>         }
>     }
> }
> 在以上配置中，upstream指令定义了一个Scrapyd服务器池，每个Scrapyd服务器的IP和端口在server指令中进行定义。listen指令定义了Nginx监听的端口和域名。location指令定义了Scrapyd API的路径，并将请求转发到Scrapyd服务器池，同时传递必要的请求头信息。
>
> 重启Nginx
> 完成Nginx配置后，使用以下命令重启Nginx：
>
> Copy code
> sudo systemctl restart nginx
> 现在，您可以通过访问http://scrapyd.example.com来访问Scrapyd服务了。如果您使用的是不同的端口和IP地址，可以相应地修改上面的配置。
> ```
>
> 别忘了更改scrapy.cfg 的配置文件的url 为上面nginx配置的`server_name scrapyd.example.com`​;
>
> ```properties
> # https://scrapyd.readthedocs.io/en/latest/deploy.html
>
> [settings]
> default = npr_english.settings
>
> [deploy:npr]
> url = erver_name scrapyd.example.com
> project = npr_eng
>
> ```

a

​![](https://zhuanlan.zhihu.com/p/160185662)​

[](https://www.zhihu.com/)切换模式

写文章

登录/注册

​![基于 Docker 的 Scrapyd 服务部署](https://pic1.zhimg.com/v2-87ae315f56400f53a327aa66d5ba9701_720w.jpg?source=172ae18b)​

# 基于 Docker 的 Scrapyd 服务部署

​![slyrx](https://picx.zhimg.com/v2-825b4015f91c3a8f3ce15c08f990339e_l.jpg?source=172ae18b)[https://www.zhihu.com/people/slyrx](https://www.zhihu.com/people/slyrx)

[slyrx](https://www.zhihu.com/people/slyrx)

## 目标

将 Scrapyd 打包制作成一个 Docker 镜像。

**准备工作**

安装 Docker

首先新建一个 Scrapy 项目，然后新建一个 scrapyd.conf，即 Scrapyd 的配置文件，内容如下：

```text
[scrapyd]
eggs_dir    = eggs
logs_dir    = logs
items_dir   =
jobs_to_keep = 5
dbs_dir     = dbs
max_proc    = 0
max_proc_per_cpu = 10
finished_to_keep = 100
poll_interval = 5.0
bind_address = 0.0.0.0
http_port   = 6800
debug       = off
runner      = scrapyd.runner
application = scrapyd.app.application
launcher    = scrapyd.launcher.Launcher
webroot     = scrapyd.website.Root

[services]
schedule.json     = scrapyd.webservice.Schedule
cancel.json       = scrapyd.webservice.Cancel
addversion.json   = scrapyd.webservice.AddVersion
listprojects.json = scrapyd.webservice.ListProjects
listversions.json = scrapyd.webservice.ListVersions
listspiders.json  = scrapyd.webservice.ListSpiders
delproject.json   = scrapyd.webservice.DeleteProject
delversion.json   = scrapyd.webservice.DeleteVersion
listjobs.json     = scrapyd.webservice.ListJobs
daemonstatus.json = scrapyd.webservice.DaemonStatus
```

接下来新建一个 requirements.txt ，将一些 Scrapy 项目常用的库都列进去，内容如下：

```text
requests
selenium
aiohttp
beautifulsoup4
pyquery
pymysql
redis
pymongo
flask
django
scrapy
scrapyd
scrapyd-client
scrapy-redis
scrapy-splash
```

最后我们新建一个 Dockerfile，内容如下：

```text
FROM python:3.6
ADD . /code
WORKDIR /code
COPY ./scrapyd.conf /etc/scrapyd/
EXPOSE 6800
RUN pip3 install -r requirements.txt
CMD scrapyd
```

**构建镜像**

```text
docker build -t scrapyd:latest .
```

查看一下构建的镜像：

```text
docker images
```

**运行测试**

```text
docker run -d -p 6800:6800 scrapyd
```

**推送至 Docker Hub**

```text
docker tag scrapyd:latest xxxx/scrapyd:latest
docker push xxxx/scrapyd:latest
```

## 远程服务器设置

在远程服务器上，只需要执行运行测试的命令：

```text
docker run -d -p 6800:6800 xxxx/scrapyd
```

发布于 2020-07-15 22:45

[scrapy](https://www.zhihu.com/topic/19950086)

[Docker](https://www.zhihu.com/topic/19950993)

[python爬虫](https://www.zhihu.com/topic/20086364)

赞同添加评论分享

喜欢收藏申请转载

赞同

‍

分享

写下你的评论...

还没有评论，发表第一个评论吧

### 推荐阅读

​![Docker(六)：Docker 三剑客之 Docker Swarm](https://picx.zhimg.com/v2-78b9328008da76f5ee90b53f93405997_250x0.jpg?source=172ae18b)[Docker(六)：Docker 三剑客之 Docker Swarm纯洁的微笑](https://zhuanlan.zhihu.com/p/35961230)​[**发表于纯洁的微笑**](https://zhuanlan.zhihu.com/p/35961230)![基于 Docker 打造前端持续集成开发环境](https://picx.zhimg.com/v2-cba0af7c4e96520a08753f0ca7412ffe_250x0.jpg?source=172ae18b)[基于 Docker 打造前端持续集成开发环境王力国](https://zhuanlan.zhihu.com/p/37961402)![【实战】docker-compose 编排 多个docker 组成一个集群并做负载](https://picx.zhimg.com/v2-f0fe26bc311689a140dac310ff226cc0_250x0.jpg?source=172ae18b)[【实战】docker-compose 编排 多个docker 组成一个集群并做负载平凡人笔记](https://zhuanlan.zhihu.com/p/142304437)​[**发表于平凡人笔记**](https://zhuanlan.zhihu.com/p/142304437)​[如何使用 docker 高效部署前端docker 变得越来越流行，它可以轻便灵活地隔离环境，进行扩容，方便运维管理。对开发者也更方便开发，测试与部署。 最重要的是， 当你面对一个陌生的项目，你可以照着 Dockerfile，甚至不看…山月](https://zhuanlan.zhihu.com/p/97510494)​[**发表于个人服务器...**](https://zhuanlan.zhihu.com/p/97510494)

[ ]

​![](chrome-extension://ofpnmcalabcbjgholdjcjblkibolbppb/static/monicaLogo.png)Ctrl+M5

​![](chrome-extension://ofpnmcalabcbjgholdjcjblkibolbppb/static/monicaLogo.png)解释

**快捷指令**

**摘要**

**语法**

**解释**

**解释代码**

**重写**

**翻译**

**问答**

**扩写**

禁用浮动按钮

​![](chrome-extension://ofpnmcalabcbjgholdjcjblkibolbppb/static/monicaLogo.png)摘要

​![](https://static.zhihu.com/heifetz/assets/liukanshan-peek.a71ecf3e.png)登录即可查看 **超5亿** **专业优质内容**超 5 千万创作者的优质提问、专业回答、深度文章和精彩视频尽在知乎。

立即登录/注册
