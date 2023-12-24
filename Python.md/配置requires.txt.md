#  配置requires.txt

> ### 配置requires.txt
>
>> pip freeze > requirements.txt
>>
>
> ```bash
> # 基础镜像
> FROM python:3.8-slim-buster
>
> # 设置工作目录
> WORKDIR /app
>
> # 将当前目录的内容复制到容器的/app目录中
> COPY . /app
>
> # 安装项目依赖
> RUN pip install -r requirements.txt
>
> # 设置环境变量
> ENV PYTHONUNBUFFERED=1
>
> # 运行爬虫
> CMD ["scrapy", "crawl", "npr_crawl "]
>
> ```
