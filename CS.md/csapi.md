## 1. 网络爬虫相关：

### Scrapy

1. 基本用法（Basic Usage）：
   - 创建 Scrapy 项目：$ `scrapy startproject project_name`
   - 创建 Scrapy 爬虫：$ `scrapy genspider spider_name start_url`
   - 运行 Scrapy 爬虫：$ `scrapy crawl spider_name`
   - 导出爬虫数据为 JSON：$ `scrapy crawl spider_name -o output.json`
2. 爬虫配置（Spider Configuration）：
   - 设置爬取的起始 URL：$ `start_urls = ['url1', 'url2']`
   - 设置允许爬取的域名：$ `allowed_domains = ['example.com']`
   - 设置爬虫的 User-Agent：$ `custom_settings = {'USER_AGENT': 'user_agent_string'}`
3. Spider API：
   - name：定义爬虫的名称。$ `name = 'spider_name'`
   - start_urls：定义爬虫起始 URL 列表。$ `start_urls = ['https://example.com']`
   - allowed_domains：定义允许抓取的域名。$ `allowed_domains = ['example.com']`
   - parse：解析响应并提取数据的默认回调函数。$ `def parse(self, response):`
4. 数据提取方法：
   - response.xpath()：使用 XPath 表达式提取数据。$ `response.xpath('//div[@class="title"]/text()').get()`
   - response.css()：使用 CSS 选择器提取数据。$ `response.css('div.title::text').get()`
   - response.follow()：根据提取的链接继续跟进页面。$ `response.follow(link, callback=self.parse_details)`
5. 数据处理：
   - Item 类：定义要提取和保存的数据结构。$ `class MyItem(Item):`
   - Field 类：定义 Item 的字段和属性。$ `field = Field()`
   - item['field']：访问 Item 的字段值。$ `item['field'] = value`
6. 请求和响应：
   - Request 类：构造新的请求对象。$ `yield Request(url, callback=self.parse)`
   - response.request.headers：获取请求的头部信息。$ `response.request.headers`
   - response.status：获取响应的状态码。$ `response.status`
7. 异步处理：
   - 异步请求和回调函数：使用 async/await 和异步函数处理请求和回调。$ `async def parse(self, response):`
8. Setting文件的配置
   - BOT_NAME：定义爬虫的名称。$ `BOT_NAME = 'your_bot_name'`
   - USER_AGENT：设置用户代理，模拟浏览器访问。$ `USER_AGENT = 'Mozilla/5.0 (Windows NT 10.0; Win64; x64) AppleWebKit/537.36 (KHTML, like Gecko) Chrome/88.0.4324.150 Safari/537.36'`
   - ROBOTSTXT_OBEY：是否遵循 robots.txt 规则。$ `ROBOTSTXT_OBEY = True`
   - CONCURRENT_REQUESTS：同时发送的请求数量。$ `CONCURRENT_REQUESTS = 16`
   - DOWNLOAD_DELAY：下载延迟，单位为秒。$ `DOWNLOAD_DELAY = 1`
   - RANDOMIZE_DOWNLOAD_DELAY：是否随机化下载延迟。$ `RANDOMIZE_DOWNLOAD_DELAY = True`
   - COOKIES_ENABLED：是否启用 cookies。$ `COOKIES_ENABLED = False`
   - LOG_ENABLED：是否启用日志。$ `LOG_ENABLED = True`
   - LOG_LEVEL：日志级别。$ `LOG_LEVEL = 'DEBUG'`
   - ITEM_PIPELINES：配置数据处理的管道。$ `ITEM_PIPELINES = {'your_project.pipelines.YourPipeline': 300}`
   - DOWNLOADER_MIDDLEWARES：配置下载中间件。$ `DOWNLOADER_MIDDLEWARES = {'your_project.middlewares.YourMiddleware': 543}`
   - SPIDER_MIDDLEWARES：配置爬虫中间件。$ `SPIDER_MIDDLEWARES = {'your_project.middlewares.YourMiddleware': 543}`
   - DOWNLOAD_TIMEOUT：下载超时时间，单位为秒。$ `DOWNLOAD_TIMEOUT = 180`
   - HTTPCACHE_ENABLED：是否启用 HTTP 缓存。$ `HTTPCACHE_ENABLED = True`
   - HTTPCACHE_EXPIRATION_SECS：HTTP 缓存过期时间，单位为秒。$ `HTTPCACHE_EXPIRATION_SECS = 86400`
   - HTTPCACHE_DIR：HTTP 缓存存储路径。$ `HTTPCACHE_DIR = 'httpcache'`

### Scrapy-redis

1. REDIS_URL：设置 Redis 连接 URL，用于任务队列和去重集合的存储。在 settings.py 文件中配置。$ `REDIS_URL = 'redis://localhost:6379'`
2. SCHEDULER：设置使用 Scrapy-Redis 的调度器。在 settings.py 文件中配置。$ `SCHEDULER = 'scrapy_redis.scheduler.Scheduler'`
3. DUPEFILTER_CLASS：设置使用 Scrapy-Redis 的去重过滤器。在 settings.py 文件中配置。$ `DUPEFILTER_CLASS = 'scrapy_redis.dupefilter.RFPDupeFilter'`
4. SCHEDULER_PERSIST：设置是否持久化任务队列和去重集合。在 settings.py 文件中配置。$ `SCHEDULER_PERSIST = True`
5. REDIS_START_URLS_KEY：设置任务队列的键名，用于存储初始 URL。在 settings.py 文件中配置。$ `REDIS_START_URLS_KEY = 'myproject:start_urls'`
6. REDIS_ITEMS_KEY：设置存储爬取数据的键名。在 settings.py 文件中配置。$ `REDIS_ITEMS_KEY = 'myproject:items'`
7. RedisSpider：继承自 scrapy_redis.spiders.RedisSpider 类，用于编写基于 Redis 的分布式爬虫。可以在爬虫类中配置 start_urls 和 redis_key。$

```
arduinoCopy codeimport scrapy
from scrapy_redis.spiders import RedisSpider

class MySpider(RedisSpider):
    name = 'myspider'
    redis_key = 'myspider:start_urls'
    ...
```

1. RedisCrawlSpider：继承自 scrapy_redis.spiders.RedisCrawlSpider 类，用于编写基于 Redis 的分布式爬虫，并支持自动跟进链接。可以在爬虫类中配置 redis_key、rules 和 allow_domains 等。$

```
pythonCopy codeimport scrapy
from scrapy_redis.spiders import RedisCrawlSpider
from scrapy.linkextractors import LinkExtractor
from scrapy.spiders import Rule

class MySpider(RedisCrawlSpider):
    name = 'myspider'
    redis_key = 'myspider:start_urls'

    rules = (
        Rule(LinkExtractor(allow=r'/page/\d+'), callback='parse_item', follow=True),
    )

    def parse_item(self, response):
        ...
```

1. from_redis_key(key)：辅助函数，用于从 Redis 的键名中提取参数。$ `from_redis_key('myproject:start_urls')`
2. to_redis_key(*parts)：辅助函数，用于生成 Redis 的键名。$ `to_redis_key('myproject', 'start_urls')`

### Scrapy-Shell

1. fetch(url)：发送 GET 请求获取指定 URL 的响应。$ `fetch("http://example.com")`
2. view(response)：在浏览器中打开指定响应的 HTML 内容。$ `view(response)`
3. response.xpath(expression)：使用 XPath 表达式在响应中提取数据。$ `response.xpath("//h1/text()").get()`
4. response.css(selector)：使用 CSS 选择器在响应中提取数据。$ `response.css("h1::text").get()`
5. response.follow(url)：根据相对链接或绝对链接创建新的请求。$ `response.follow("/page/2")`
6. response.css(selector).attrib["attribute"]：提取指定属性的值。$ `response.css("img").attrib["src"]`
7. response.xpath(expression).getall()：提取所有匹配结果。$ `response.xpath("//a/@href").getall()`
8. response.url：获取响应的 URL。$ `response.url`
9. response.status：获取响应的状态码。$ `response.status`
10. response.headers：获取响应的头部信息。$ `response.headers`
11. response.body：获取响应的原始内容。$ `response.body`
12. response.meta：获取响应的元数据。$ `response.meta`
13. request(url)：发送新的请求并获取响应。$ `request("http://example.com")`
14. request.meta：设置请求的元数据。$ `request.meta["key"] = "value"`
15. items()：查看当前已提取的 Item。$ `items()`

### 链接提取器extractor

在 Scrapy-Redis 中，`rules` 和 `LinkExtractor` 是常用的配置项，用于定义爬取规则和提取链接的方式。下面是一些常见的配置示例：

1. `allow`：允许匹配的链接正则表达式。只有匹配的链接才会被提取。
   
   ```
   pythonCopy code
   Rule(LinkExtractor(allow=r'/page/\d+'), callback='parse_item', follow=True)
   ```

2. `deny`：拒绝匹配的链接正则表达式。匹配的链接将被忽略。
   
   ```
   pythonCopy code
   Rule(LinkExtractor(allow=r'/page/\d+', deny=r'/page/1'), callback='parse_item', follow=True)
   ```

3. `restrict_xpaths`：基于 XPath 表达式的链接提取限制。只有符合限制的链接才会被提取。
   
   ```
   cssCopy code
   Rule(LinkExtractor(restrict_xpaths='//div[@class="pagination"]/a'), callback='parse_item', follow=True)
   ```

4. `restrict_css`：基于 CSS 选择器的链接提取限制。只有符合限制的链接才会被提取。
   
   ```
   lessCopy code
   Rule(LinkExtractor(restrict_css='.pagination a'), callback='parse_item', follow=True)
   ```

5. `callback`：指定回调函数，用于处理提取到的链接对应的响应。
   
   ```
   pythonCopy code
   Rule(LinkExtractor(allow=r'/page/\d+'), callback='parse_item', follow=True)
   ```

6. `follow`：指定是否跟进提取到的链接，继续爬取。设置为 True 则跟进，设置为 False 则不跟进。
   
   ```
   pythonCopy code
   Rule(LinkExtractor(allow=r'/page/\d+'), callback='parse_item', follow=True)
   ```

### Scrapyd

1. 部署项目：使用 Scrapyd 部署 Scrapy 项目。
   
   ```
   cssCopy code
   $ scrapyd-deploy [target] -p project_name
   ```

2. 查看部署的项目列表：查看 Scrapyd 中已经部署的项目列表。
   
   ```
   shellCopy code
   $ curl http://localhost:6800/listprojects.json
   ```

3. 启动爬虫任务：启动一个已部署的 Scrapy 项目的爬虫任务。
   
   ```
   shellCopy code
   $ curl http://localhost:6800/schedule.json -d project=project_name -d spider=spider_name
   ```

4. 查看爬虫任务状态：查看已启动的爬虫任务的运行状态。
   
   ```
   shellCopy code
   $ curl http://localhost:6800/listjobs.json?project=project_name
   ```

5. 取消爬虫任务：取消正在运行的爬虫任务。
   
   ```
   shellCopy code
   $ curl http://localhost:6800/cancel.json -d project=project_name -d job=job_id
   ```

6. 删除爬虫任务：从 Scrapyd 中删除已完成或取消的爬虫任务。
   
   ```
   shellCopy code
   $ curl http://localhost:6800/delproject.json -d project=project_name -d job=job_id
   ```

7. 查看 Scrapyd 系统状态：查看 Scrapyd 服务的系统状态信息。
   
   ```
   shellCopy code
   $ curl http://localhost:6800/daemonstatus.json
   ```

## 2. 容器化和虚拟化：

### Docker

### VirtualBox

**容器操作**

1. 运行容器：创建并运行一个新的容器。$ `docker run [OPTIONS] IMAGE [COMMAND] [ARG...]`
2. 停止容器：停止正在运行的容器。$ `docker stop [OPTIONS] CONTAINER [CONTAINER...]`
3. 删除容器：删除指定的容器。$ `docker rm [OPTIONS] CONTAINER [CONTAINER...]`
4. 查看容器列表：列出当前正在运行的容器。$ `docker ps [OPTIONS]`
5. 进入容器：进入正在运行的容器的 shell 环境。$ `docker exec [OPTIONS] CONTAINER [COMMAND] [ARG...]`

> `OPTIONS` 的具体内容取决于不同的 Docker 命令，以下是一些常见的OPTIONS示例：
> 
> - `-d` 或 `--detach`：在后台运行容器。
> - `-p` 或 `--publish`：将容器的端口映射到主机的端口。
> - `-v` 或 `--volume`：挂载主机上的目录或数据卷到容器中。
> - `--name`：为容器指定一个名称。
> - `-e` 或 `--env`：设置容器的环境变量。
> - `-n` 或 `--network`：指定容器所使用的网络。
> - `-it`：以交互模式运行容器并分配一个伪终端。

**镜像操作**

1. 拉取镜像：从 Docker 镜像仓库中拉取指定的镜像。$ `docker pull [OPTIONS] NAME[:TAG|@DIGEST]`
2. 构建镜像：根据 Dockerfile 构建一个新的镜像。$ `docker build [OPTIONS] PATH | URL | -`
3. 查看镜像列表：列出本地已有的镜像。$ `docker images [OPTIONS]`
4. 删除镜像：删除一个或多个镜像。$ `docker rmi [OPTIONS] IMAGE [IMAGE...]`
5. 清除未使用的镜像：清除本地未被任何容器使用的镜像。$ `docker image prune [OPTIONS]`
6. 强制清除镜像：强制删除一个或多个镜像，包括被容器使用的镜像。$ `docker rmi -f IMAGE [IMAGE...]`
7. 清除所有镜像：清除本地所有镜像，包括被容器使用的镜像。$ `docker rmi $(docker images -q)`

**网络操作**

1. 创建网络：创建一个新的 Docker 网络。$ `docker network create [OPTIONS] NETWORK`
2. 连接网络：将容器连接到指定的 Docker 网络。$ `docker network connect [OPTIONS] NETWORK CONTAINER`
3. 断开网络：将容器从指定的 Docker 网络中断开连接。$ `docker network disconnect [OPTIONS] NETWORK CONTAINER`
4. 列出网络：列出已创建的 Docker 网络。$ `docker network ls [OPTIONS]`

**存储卷操作**

1. 创建存储卷：创建一个新的 Docker 存储卷。$ `docker volume create [OPTIONS] [VOLUME]`
2. 挂载存储卷：将存储卷挂载到容器中的指定路径。$ `docker run -v [VOLUME]:[CONTAINER_PATH] [IMAGE]`

**日志和调试**

1. 查看容器日志：查看容器的日志输出。$ `docker logs [OPTIONS] CONTAINER`
2. 进入容器调试模式：进入正在运行的容器的调试模式。$ `docker exec -it CONTAINER /bin/bash`

**其他操作**

1. 查看 Docker 版本：查看当前安装的 Docker 版本信息。$ `docker version`
2. 查看 Docker 信息：查看 Docker 的系统信息。$ `docker info`
3. 执行容器命令：在运行的容器中执行命令。$ `docker exec [OPTIONS] CONTAINER COMMAND [ARG...]`

Dockerfile 常用语法

1. `FROM`：指定基础镜像。

2. `RUN`：在镜像中执行命令。

3. `COPY`：将文件从主机复制到镜像中。

4. `ADD`：将文件或 URL 复制到镜像中。

5. `WORKDIR`：设置工作目录。

6. `ENV`：设置环境变量。

7. `EXPOSE`：声明容器运行时使用的端口。

8. `CMD`：指定容器启动后要执行的命令。
   
   ```dockerfile
   FROM python:3.9
   
   WORKDIR /app
   
   COPY requirements.txt .
   
   RUN pip install --no-cache-dir -r requirements.txt
   
   COPY . .
   
   EXPOSE 8000
   
   CMD ["python", "app.py"]
   ```

Docker Compose

**简介：是一个用于定义和运行多个容器应用的工具。它使用一个 YAML 文件来配置应用的服务、网络和卷等。**

```yaml
version: '3'
services:
  web:
    build:
      context: .
      dockerfile: Dockerfile
    ports:
      - 8000:8000
    volumes:
      - .:/app
    environment:
      - MYSQL_HOST=db
      - MYSQL_USER=myuser
      - MYSQL_PASSWORD=mypassword
  db:
    image: mysql:5.7
    environment:
      - MYSQL_ROOT_PASSWORD=secret
      - MYSQL_USER=myuser
      - MYSQL_PASSWORD=mypassword
    ports:
      - 3306:3306
    networks:
      mynetwork:
        ipv4_address: 172.18.0.2

networks:
  mynetwork:
    ipam:
      driver: default
      config:
        - subnet: 172.18.0.0/16
```

## 3. Linux 系统：

### Process

### Network

### Disk

### shell

### grep

### Sed

### Rsync

## 4. Python 及其库：

### Numpy

### Pandas

### faker

### Jupyter

### Xpath

**简介： 在 XML 或 HTML 文档中定位元素的语言**

1. 选择节点：
   - `//element`：选取所有名为 element 的节点。
   - `/parent/child`：选取父节点下名为 child 的直接子节点。
   - `/parent/child/grandchild`：选取父节点下名为 child 的直接子节点中的名为 grandchild 的节点。
   - `//*[@id='id']`：根据节点的 id 属性选择节点。
2. 条件筛选：
   - `[@attribute='value']`：根据属性值进行筛选，选取具有属性 attribute 值为 value 的节点。
   - `[position()]`：根据节点在父节点中的位置进行筛选。
   - `[last()]`：选取最后一个节点。
3. 文本内容筛选：
   - `/element[text()='content']`：选取文本内容为 content 的 element 节点。
   - `/element[contains(text(), 'text')]`：选取包含指定文本的 element 节点。
4. 节点操作：
   - `//element/@attribute`：选取节点的属性。
   - `/element/@attribute='value'`：检查节点的属性值是否等于给定值。
   - `/element/@attribute='new_value'`：修改节点的属性值为新值。
5. 配符分类及其示例：
   1. 元素节点通配符：使用 `*` 匹配任意类型的元素节点。 示例：`//*` 匹配文档中的所有元素节点。
   2. 属性节点通配符：使用 `@*` 匹配任意类型的属性节点。 示例：`//@*` 匹配文档中的所有属性节点。
   3. 文本节点通配符：使用 `text()` 匹配文本节点。 示例：`//text()` 匹配文档中的所有文本节点。
   4. 注释节点通配符：使用 `comment()` 匹配注释节点。 示例：`//comment()` 匹配文档中的所有注释节点。
   5. 处理指令节点通配符：使用 `processing-instruction()` 匹配处理指令节点。 示例：`//processing-instruction()` 匹配文档中的所有处理指令节点。

### **Beautifulsoup**

**简介： 最火的爬虫解析库**

1. 解析 HTML 文档：$ `from bs4 import BeautifulSoup soup = BeautifulSoup(html_doc, 'html.parser')`
2. 查找元素：
   - find_all：根据标签名查找所有匹配的元素。$ `elements = soup.find_all('tag_name')`
   - find：根据标签名查找第一个匹配的元素。$ `element = soup.find('tag_name')`
   - 按照属性查找元素：$ `elements = soup.find_all('tag_name', {'attribute_name': 'attribute_value'})`
3. 根据 CSS 选择器查找元素：
   - select：使用 CSS 选择器查找所有匹配的元素。$ `elements = soup.select('css_selector')`
   - select_one：使用 CSS 选择器查找第一个匹配的元素。$ `element = soup.select_one('css_selector')`
4. 提取元素内容和属性：
   - 获取元素文本内容：$ `text = element.get_text()`
   - 获取元素属性值：$ `attr_value = element['attribute_name']`
5. 遍历元素：
   - 遍历查找到的元素列表：$ `for element in elements:`
   - 遍历元素的子元素：$ `for child in element.children:`

### Requests

### Pyquery

1. 创建 PyQuery 对象：$ `from pyquery import PyQuery as pq doc = pq(html_doc)`
2. 查找元素：
   - find：根据选择器查找匹配的元素。$ `elements = doc.find('selector')`
   - children：查找子元素。$ `children = element.children()`
   - eq：根据索引获取指定位置的元素。$ `element = elements.eq(index)`
3. 提取元素内容和属性：
   - text：获取元素的文本内容。$ `text = element.text()`
   - attr：获取元素的属性值。$ `attr_value = element.attr('attribute_name')`
4. 修改元素内容和属性：
   - text：设置元素的文本内容。$ `element.text('new_text')`
   - html：设置元素的 HTML 内容。$ `element.html('new_html')`
   - attr：设置元素的属性值。$ `element.attr('attribute_name', 'attribute_value')`
5. 遍历元素：
   - 遍历查找到的元素列表：$ `for element in elements:`
   - 遍历元素的子元素：$ `for child in element.children():`
6. 操作元素：
   - add_class：为元素添加 CSS 类。$ `element.add_class('class_name')`
   - remove_class：移除元素的 CSS 类。$ `element.remove_class('class_name')`
   - append：在元素内部追加内容。$ `element.append('content')`

简介：基于 jQuery 语法用于解析和操作 HTML 和 XML 文档

[[Selenium]]
### Pypetee

### Splash

### Plotly

### Peewee

## 5. 数据库：

### Redis

**简介：: 高性能K/V数据库**

---

通用分类 (General) 

1. KEYS：获取满足指定模式的键列表。$ `keys pattern`
2. EXISTS：检查键是否存在。$ `exists key`
3. DEL：删除指定键。$ `del key`
4. EXPIRE：设置键的过期时间。$ `expire key seconds`
5. TTL：获取键的剩余过期时间。$ `ttl key`
6. TYPE：获取键的数据类型。$ `type key`
7. RENAME：重命名键。$ `rename key newkey`
8. INCR：将键的值递增。$ `incr key`
9. DECR：将键的值递减。$ `decr key`
10. APPEND：将值追加到键的值末尾。$ `append key value`
11. AUTH：使用密码进行身份验证。$ `auth password`
12. INFO：获取 Redis 服务器的信息。$ `info`
13. SCAN：迭代遍历键。$ `scan cursor [MATCH pattern] [COUNT count]`
14. SAVE：同步将数据保存到磁盘。$ `save`
15. BGSAVE：异步将数据保存到磁盘。$ `bgsave`

字符串 (String)

1. SET：设置指定键的值。$ `set mykey value`
2. GET：获取指定键的值。$ `get mykey`

哈希表 (Hash)

1. HSET：在指定的哈希表中设置字段的值。$ `hset myhash field value`
2. HGET：获取指定哈希表中字段的值。$ `hget myhash field`
3. HMSET：在指定的哈希表中设置多个字段的值。$ `hmset myhash field1 value1 field2 value2`
4. HMGET：获取指定哈希表中多个字段的值。$ `hmget myhash field1 field2`
5. HDEL：删除指定哈希表中的字段。$ `hdel myhash field`

列表 (List)

1. LPUSH：将一个或多个值插入到列表的头部。$ `lpush mylist value1 value2 value3`
2. RPUSH：将一个或多个值插入到列表的尾部。$ `rpush mylist value1 value2 value3`
3. LPOP：移除并返回列表头部的元素。$ `lpop mylist`
4. RPOP：移除并返回列表尾部的元素。$ `rpop mylist`
5. LRANGE：获取列表指定范围内的元素。$ `lrange mylist 0 -1`

集合 (Set)

1. SADD：向集合添加一个或多个成员。$ `sadd myset member1 member2 member3`
2. SMEMBERS：获取集合中的所有成员。$ `smembers myset`
3. SREM：从集合中移除一个或多个成员。$ `srem myset member1 member2`

有序集合 (Sorted Set)

1. ZADD：向有序集合添加一个或多个成员。$ `zadd myzset 1 member1 2 member2 3 member3`
2. ZRANGE：按索引范围获取有序集合的成员。$ `zrange myzset 0 -1`
3. ZRANK：获取有序集合中成员的排名。$ `zrank myzset member`

位图 (Bitmap)

1. SETBIT：设置位图指定位置的值。$ `setbit mybitmap 0 1`
2. GETBIT：获取位图指定位置的值。$ `getbit mybitmap 0`

键操作

1. DEL：删除指定键。$ `del mykey`
2. EXISTS：检查键是否存在。$ `exists mykey`
3. KEYS：获取满足指定模式的键列表。$ `keys pattern`
4. EXPIRE：设置键的过期时间。$ `expire mykey 60`

### MySQL

简介： MySQL 是一种常用的关系型数据库管理系统

```sql
# 用到大多数sql语法, 按需修改使用。
SELECT t1.column1, t1.column2, COUNT(t2.column3) AS count_column3
FROM table1 t1
JOIN table2 t2 ON t1.id = t2.table1_id
WHERE t1.condition1 = 'value1'
  AND t2.condition2 = 'value2'
  AND t1.column3 IN (SELECT column3 FROM table3 WHERE condition3 = 'value3')
GROUP BY t1.column1
HAVING count_column3 > 10
ORDER BY t1.column1 DESC
LIMIT 10 OFFSET 20;
```

数据查询

1. SELECT：从表中查询数据。$ `SELECT * FROM table_name`
2. WHERE：条件筛选数据。$ `SELECT * FROM table_name WHERE condition`
3. ORDER BY：按指定列排序结果。$ `SELECT * FROM table_name ORDER BY column_name`
4. LIMIT：限制返回结果的数量。$ `SELECT * FROM table_name LIMIT 10`

聚合操作

1. GROUP BY：按指定列对结果进行分组。$ `SELECT column, COUNT(*) FROM table_name GROUP BY column`
2. HAVING：对GROUP BY的结果进行筛选。$ `SELECT column, COUNT(*) FROM table_name GROUP BY column HAVING condition`
3. COUNT：计算指定列的行数。$ `SELECT COUNT(column_name) FROM table_name`
4. SUM：计算指定列的总和。$ `SELECT SUM(column_name) FROM table_name`
5. AVG：计算指定列的平均值。$ `SELECT AVG(column_name) FROM table_name`
6. MAX：获取指定列的最大值。$ `SELECT MAX(column_name) FROM table_name`
7. MIN：获取指定列的最小值。$ `SELECT MIN(column_name) FROM table_name`

数据插入

1. INSERT INTO：向表中插入数据。$ `INSERT INTO table_name (column1, column2, ...) VALUES (value1, value2, ...)`

数据更新

1. UPDATE：更新表中的数据。$ `UPDATE table_name SET column1 = value1, column2 = value2 WHERE condition`

数据删除

1. DELETE FROM：从表中删除数据。$ `DELETE FROM table_name WHERE condition`

表操作

1. CREATE TABLE：创建新表。$ `CREATE TABLE table_name (column1 datatype, column2 datatype, ...)`
2. ALTER TABLE：修改表结构。$ `ALTER TABLE table_name ADD column_name datatype`
3. DROP TABLE：删除表。$ `DROP TABLE table_name`

数据库操作：

1. 创建数据库：$`CREATE DATABASE database_name;`
2. 删除数据库：$`DROP DATABASE database_name;`

连接操作

1. INNER JOIN：连接多个表的匹配行。$ `SELECT * FROM table1 INNER JOIN table2 ON condition`
2. LEFT JOIN：连接左表的所有行和右表的匹配行。$ `SELECT * FROM table1 LEFT JOIN table2 ON condition`
3. RIGHT JOIN：连接右表的所有行和左表的匹配行。$ `SELECT * FROM table1 RIGHT JOIN table2 ON condition`
4. FULL JOIN：连接左表和右表的所有行。$ `SELECT * FROM table1 FULL JOIN table2 ON condition`
5. CROSS JOIN：连接两个表的所有行。$ `SELECT * FROM table1 CROSS JOIN table2`
6. SELF JOIN：将表与自身进行连接。$ `SELECT * FROM table1 t1 INNER JOIN table1 t2 ON condition`

用户权限操作！！：

1. 修改用户权限：$`GRANT privileges ON database.table TO 'username'@'host';`
2. 移除用户权限：$`REVOKE privileges ON database.table FROM 'username'@'host';`

用户密码操作！！：

1. 修改用户密码：$`ALTER USER 'username'@'host' IDENTIFIED BY 'new_password';`
2. 重置用户密码：$`SET PASSWORD FOR 'username'@'host' = PASSWORD('new_password');`

远程访问控制！！：

1. 授权远程访问：$`GRANT ALL PRIVILEGES ON database.* TO 'username'@'%' IDENTIFIED BY 'password';`
2. 移除远程访问权限：$`REVOKE ALL PRIVILEGES ON database.* FROM 'username'@'%';`

数据导入导出！！：

1. 导出整个数据库：$`mysqldump -u username -p database_name > backup.sql`
2. 导出指定表数据：$`mysqldump -u username -p database_name table_name > backup.sql`
3. 导出数据并压缩：$`mysqldump -u username -p database_name | gzip > backup.sql.gz`
4. 导入整个数据库：$`mysql -u username -p database_name < backup.sql`
5. 导入指定表数据：$`mysql -u username -p database_name table_name < backup.sql`
6. 导入压缩的数据：$`gzip -d < backup.sql.gz | mysql -u username -p database_name`
7. 导出表结构：$`mysqldump -u username -p --no-data database_name table_name > structure.sql`
8. 导出数据为CSV文件：$`SELECT * INTO OUTFILE '/path/to/file.csv' FIELDS TERMINATED BY ',' FROM table_name;`
9. 导入CSV文件数据：$`LOAD DATA INFILE '/path/to/file.csv' INTO TABLE table_name FIELDS TERMINATED BY ',';`

### MongoDB

数据库操作：

1. SHOW DATABASES：显示所有数据库。$ `show databases`
2. USE：切换到指定数据库。$ `use database_name`

集合操作：

1. SHOW COLLECTIONS：显示当前数据库中的所有集合。$ `show collections`
2. CREATE COLLECTION：创建一个新集合。$ `db.createCollection("collection_name")`
3. DROP COLLECTION：删除指定集合。$ `db.collection_name.drop()`

文档操作：

1. INSERT：插入一个文档。$ `db.collection_name.insert({field1: value1, field2: value2})`
2. FIND：查询文档。$ `db.collection_name.find({field: value})`
3. UPDATE：更新文档。$ `db.collection_name.update({field: value}, {field: newValue})`
4. DELETE：删除文档。$ `db.collection_name.deleteOne({field: value})`

条件查询：

1. 等于（`$eq`）：查询字段等于指定值的文档。$ `db.collection_name.find({field: {$eq: value}})`
2. 不等于（`$ne`）：查询字段不等于指定值的文档。$ `db.collection_name.find({field: {$ne: value}})`
3. 大于（`$gt`）：查询字段大于指定值的文档。$ `db.collection_name.find({field: {$gt: value}})`
4. 大于等于（`$gte`）：查询字段大于等于指定值的文档。$ `db.collection_name.find({field: {$gte: value}})`
5. 小于（`$lt`）：查询字段小于指定值的文档。$ `db.collection_name.find({field: {$lt: value}})`
6. 小于等于（`$lte`）：查询字段小于等于指定值的文档。$ `db.collection_name.find({field: {$lte: value}})`
7. 包含在数组中（`$in`）：查询字段值包含在指定数组中的文档。$ `db.collection_name.find({field: {$in: [value1, value2, ...]}})`
8. 不包含在数组中（`$nin`）：查询字段值不包含在指定数组中的文档。$ `db.collection_name.find({field: {$nin: [value1, value2, ...]}})`
9. 逻辑与（`$and`）：查询满足多个条件的文档。$ `db.collection_name.find({$and: [{condition1}, {condition2}, ...]})`
10. 逻辑或（`$or`）：查询满足任意一个条件的文档。$ `db.collection_name.find({$or: [{condition1}, {condition2}, ...]})`

聚合操作：

1. AGGREGATE：执行聚合操作，对数据进行多阶段的处理。$ `db.collection_name.aggregate([...pipeline stages])`

索引操作：

1. CREATE INDEX：创建索引。$ `db.collection_name.createIndex({field: 1})`
2. DROP INDEX：删除索引。$ `db.collection_name.dropIndex("index_name")`

连接管理：

1. CONNECT：连接到 MongoDB 服务器。$ `mongo --host hostname --port port`
2. DISCONNECT：断开与 MongoDB 服务器的连接。$ `quit()`

认证和授权：

1. AUTHENTICATE：对当前会话进行认证。$ `db.auth(username, password)`
2. CREATE USER：创建新用户。$ `db.createUser({user: "username", pwd: "password", roles: ["role"]})`
3. DROP USER：删除用户。$ `db.dropUser("username")`
4. GRANT ROLES TO USER：授予用户角色权限。$ `db.grantRolesToUser("username", ["role"])`

备份和恢复：

1. BACKUP：备份 MongoDB 数据。$ `mongodump --db database_name --out /path/to/backup_directory`
2. RESTORE：恢复 MongoDB 数据。$ `mongorestore --db database_name /path/to/backup_directory`

## 6. 后端技术：

### Flask

## 7. 前端技术：

### Vue & Vite

### NPM & Yarn

### CSS

### HTML

### JavaScript

### TypeScript

## 8. 中间件：

### Nacos

### Nginx

### RabbitMq

## 9. 其他

### ELK

### Lua

### zabbix

### Celery

### Ansibel

### Talegraf Influxdb Grafana

