# Nginx配置scrapyd

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

‍
