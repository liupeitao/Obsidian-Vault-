# settings

### scrapy配置

1. ​`BOT_NAME`​: 爬虫的名称。

当有多个爬虫项目需要使用 `scrapy-redis`​ 进行任务调度时，可以使用不同的 `BOT_NAME`​ 来标识不同的爬虫项目

1. ​`USER_AGENT`​: 发送给服务器的 User-Agent 值。
2. ​`ROBOTSTXT_OBEY`​: 是否遵守 robots.txt 规则。
3. ​`DOWNLOAD_DELAY`​: 下载延迟时间。
4. ​`COOKIES_ENABLED`​: 是否启用 Cookies。
5. ​`LOG_LEVEL`​: 日志级别。
6. ​`ITEM_PIPELINES`​: 项目管道列表。
7. ​`FEED_URI`​: 导出数据的 URI。
8. ​`FEED_FORMAT`​: 导出数据的格式。
9. ​`HTTPCACHE_ENABLED`​: 是否启用 HTTP 缓存。
10. ​`HTTPCACHE_EXPIRATION_SECS`​: HTTP 缓存的过期时间。
11. ​`HTTPCACHE_DIR`​: 缓存目录。
12. ​`HTTPCACHE_IGNORE_HTTP_CODES`​: 忽略的 HTTP 状态码。
13. ​`HTTPCACHE_STORAGE`​: 缓存存储插件。
14. ​`DOWNLOAD_TIMEOUT`​: 下载超时时间。
15. ​`CONCURRENT_REQUESTS`​: 并发请求数量。
16. ​`AUTOTHROTTLE_ENABLED`​: 是否启用自动限速。
17. ​`AUTOTHROTTLE_START_DELAY`​: 自动限速的启动延迟时间。
18. ​`AUTOTHROTTLE_TARGET_CONCURRENCY`​: 自动限速的目标并发数量。
19. ​`AUTOTHROTTLE_MAX_DELAY`​: 自动限速的最大延迟时间。

‍

### scrapy-redis

1. ​`REDIS_HOST`​​: Redis服务器的主机名。
2. ​`REDIS_PORT`​​: Redis服务器的端口号。
3. ​`REDIS_DB`​​: Redis服务器上的数据库编号。
4. ​`REDIS_PASSWORD`​​: Redis服务器的密码。
5. ​`SCHEDULER`​​: Scrapy调度器使用的类。
6. ​`DUPEFILTER_CLASS`​​: Scrapy去重器使用的类。
7. ​`SCHEDULER_PERSIST`​​: 是否在关闭Spider后保留Redis中的去重集和任务队列。
8. ​`SCHEDULER_FLUSH_ON_START`​​: 是否在每次启动Spider时清空任务队列和去重集。
9. ​`SCHEDULER_IDLE_BEFORE_CLOSE`​​: 空闲关闭调度器之前的时间。
10. ​`ITEM_PIPELINES`​​: 需要启用的管道的名称及其顺序。
11. ​`LOG_LEVEL`​​: 日志级别。
12. ​`REDIS_ENCODING`​​: Redis中存储的值的编码格式。
13. ​`REDIS_START_URLS_BATCH_SIZE`​​: Redis中起始URL的批量大小。
14. ​`REDIS_START_URLS_KEY`​​: 存储起始URL的Redis键。
15. ​`REDIS_ITEMS_KEY`​​: 存储抓取的项的Redis键。
16. ​`REDIS_QUEUE_NAME`​​: 存储任务的Redis队列的名称。
17. ​`REDIS_QUEUE_SERIALIZER`​​: Redis队列中的对象序列化程序。
18. ​`REDIS_MAX_BATCH`​​: 一次从Redis中获取的最大任务数。
19. ​`REDIS_BATCH_SIZE`​​: 一次向Redis中发送的最大任务数。
20. ​`REDIS_PARAMS`​​: Redis连接参数，如超时时间、连接池大小等

## Scrapy配置Scrapy-redis

```sql
# 使用 scrapy_redis 的调度器替代 Scrapy 原版的调度器
SCHEDULER = "scrapy_redis.scheduler.Scheduler"
 
# 允许暂停和恢复，redis 中断后可以保留现场
SCHEDULER_PERSIST = True
 
# Scrapy Redis 的去重组件
DUPEFILTER_CLASS = "scrapy_redis.dupefilter.RFPDupeFilter"
 
# 设置 Redis 数据库连接信息
REDIS_HOST = 'localhost'
REDIS_PORT = 6379
REDIS_PASSWORD = 'password'
 
# 设置 Item Pipeline
ITEM_PIPELINES = {
    'scrapy_redis.pipelines.RedisPipeline': 300,
}

```

‍
