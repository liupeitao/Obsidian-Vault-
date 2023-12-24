利用 yield 实现异步爬虫

回调函数有很多缺点，我们可以尝试对上一节实验中的 I/O 异步程序进行优化，在一个方法中完成整个爬取过程，无需跳转到其它方法中。在前面的课程中介绍过协程，协程可以暂停执行，让出控制权，使用 send 方法又可以再次启动协程，继续执行。我们下面使用协程来实现爬虫。

将以下代码写入 spider_yield.py 文件：

```python
# File Name: spider_yield.py

import os
import time
import socket
from urllib.parse import urlparse
from selectors import DefaultSelector, EVENT_WRITE, EVENT_READ

# 异步事件对象
selector = DefaultSelector()
# 本程序直接使用变量作为信号
stopped = False

urls = ['https://dn-simplecloud.shiyanlou.com/ncn1.jpg',
        'https://dn-simplecloud.shiyanlou.com/ncn110.jpg',
        'https://dn-simplecloud.shiyanlou.com/ncn109.jpg',
        'https://dn-simplecloud.shiyanlou.com/1548126810319.png',
        'https://dn-simplecloud.shiyanlou.com/1517282865454.png',
        'https://dn-simplecloud.shiyanlou.com/1543913883545.png',
        'https://dn-simplecloud.shiyanlou.com/1502778396172.png',
        'https://dn-simplecloud.shiyanlou.com/1540965522764.png',
        'https://dn-simplecloud.shiyanlou.com/1546500900109.png',
        'https://dn-simplecloud.shiyanlou.com/1547620906601.png'
]
```

以上代码与 I/O 异步程序无异，唯一区别是使用变量 stopped 替代了 Signal 类，使用后者在多线程模式中是唯一的选择，单线程程序可以使用简单的全局变量来控制事件循环的终止。

接下来，编写用于存放程序运行中转移数据的 Future 类，回调函数会调用其中的 set_value 方法：

```python
class Future:
    def __init__(self):
        self.value = None       # 该属性用于存放服务器返回的数据片段
        self._step_list = []    # 该列表用于存放控制协程的方法，其实也就存一个

    # 该方法只运行一次，在 Task 类的实例的 step 方法里运行
    # 作用是把控制协程的方法放到列表里，以备运行
    # func 的值其实就是 Task 类的实例的 step 方法
    def add_done_callback(self, func):
        self._step_list.append(func)

    # 设置实例的 value 属性并执行控制协程的方法
    # 每次该方法运行时，_step_list 里有且只有一个 step 方法
    def set_value(self, value):
        self.value = value
        for func in self._step_list:
            # 运行控制协程的方法，让协程进入下一步，注意参数为 Future 实例自身
            func(self)
```

下一步，创建爬虫类 Crawler ，该类实例用于创建客户端套接字、收发数据、存储图片。回调函数写入类中的 fetch 方法：

```python
class Crawler:
    def __init__(self, url):
        self._url = url
        self.url = urlparse(self._url)
        self.response = b''

    def fetch(self):
        global stopped
        start_time = time.time()
        sock = socket.socket()
        # 将客户端套接字设置为非阻塞模式
        sock.setblocking(False)
        try:
            # 客户端套接字向图片所在服务器发送链接请求
            sock.connect((self.url.netloc, 80))
        except BlockingIOError:
            pass
        # 创建 Future 实例
        f = Future()
        # 该函数为回调函数，连接服务器成功后写事件就绪，调用此方法
        def writable():
            # 运行 Future 实例对象的 set_value 方法，控制协程向下运行
            f.set_value(None)
        # 向 selector 中注册套接字的可写事件
        # 参数为套接字的文件描述符、事件常数、回调函数
        # 当连接服务器成功后，可写事件会立即就绪，然后自动执行对应的回调函数
        selector.register(sock.fileno(), EVENT_WRITE, writable)
        # yield 关键字说明 fetch 函数是协程函数，其函数调用为协程
        # 使用 Python 内置的 next 方法或者协程本身的 send 方法预激协程
        # 在遇到 yield 时暂停执行，控制权会返回到调用方，但会保留上下文环境
        yield f
        # 连接服务器成功后，可写事件就绪，继续向下执行
        # 注销 selector 对可写事件的监听
        selector.unregister(sock.fileno())
        get = 'GET {0} HTTP/1.1\r\nHost: {1}\r\nConnection: close\r\n\r\n' \
                .format(self.url.path, self.url.netloc)
        # 向服务端发送数据
        sock.send(get.encode())
        # 该函数为回调函数，服务器传回数据时可读事件就绪，调用此方法
        def readable():
            # 将接收到的数据存放到 future 对象的 value 属性中
            f.set_value(sock.recv(4096))
        # 注册监听客户端套接字的可读事件
        selector.register(sock.fileno(), EVENT_READ, readable)
        # 此 while 循环会循环运行多次以处理服务器返回的数据，直到接收完全部数据
        while True:
            f = Future()
            # 让出控制权，返回到调用方，在协程执行 send 方法时再继续执行
            # send 方法的参数赋值给 value 变量
            value = yield f
            if value:
                self.response += value
            # 如果数据接收完毕
            else:
                # 从 URL 列表中移除此 URL
                urls.remove(self._url)
                # 如果 URL 列表为空，表示全部图片爬取完毕
                if not urls:
                    # 修改信号，停止 loop 事件循环函数
                    stopped = True
                # 数据接收完毕，注销对客户端套接字的事件监听
                selector.unregister(sock.fileno())
                # 这个客户端套接字的任务已全部完成，关闭套接字
                sock.close()
                # 将收到的数据的图片部分写入文件
                with open('pic' + self.url.path, 'wb') as file:
                    file.write(self.response.split(b'\r\n\r\n')[1])
                print("URL: {0}, 耗时: {1:.3f}s".format(
                        self._url, time.time() - start_time))
                # 此爬虫实例的任务已完成，退出 while 循环
                break
```

接下来编写 Task 类和事件循环函数。Task 的 step 方法用于控制 Crawler 类的 fetch 方法的暂停与恢复，step 方法由 Future 类的 set_value 方法调用：

```python
class Task:
    def __init__(self, coro):
        self.coro = coro    # 将协程赋值给 coro 属性
        f = Future()        # 创建 Future 实例
        self.step(f)        # 控制协程运行

    # 控制协程继续执行，参数为 Future 实例
    def step(self, future):
        try:
            # 控制协程继续执行，并向协程发送数据
            # 协程返回的是 Future 实例对象
            # 首次运行此方法时，future.value 的值为 None
            # 此时 send 方法的作用是预激协程
            # send 方法的返回值为 coro 的 fetch 方法的第一个 yield 后面的对象
            # 即 new_future 的值为 Future 类的新实例
            new_future = self.coro.send(future.value)
        except StopIteration:
            return
        # 将 step 方法自身加入到新 future 的回调函数列表里
        # 每次协程在 yield 处暂停，这行代码是最后执行的代码
        new_future.add_done_callback(self.step)


# 事件循环函数
def loop():
    while not stopped:
        # selector.select 方法为阻塞运行
        # 轮询被监听事件，如有事件就绪，立即返回就绪事件列表
        events = selector.select()
        # 事件列表中每个事件是一个元组，元组里有俩元素
        # 分别是 SelectorKey 对象和事件常数
        # 事件常数暂时无用，用一个下划线接收它
        for event_key, _ in events:
            # SelectorKey 对象的 data 属性值是回调函数
            # 回调函数是 writable 或 readable
            callback = event_key.data
            callback()
```

定义好所需类和方法，就可以安排主函数了，程序运行由此函数来布置：

```python
def main():
    os.system('mkdir -p pic')
    start = time.time()
    for url in urls:
        crawler = Crawler(url)
        # crawler.fetch 方法是协程，调用此方法返回一个协程对象
        # Task 类初始化，首次运行 step 方法预激协程
        # 预激协程的过程中，会创建客户端套接字向图片所在服务器发送连接请求
        # 并将套接字的可写事件注册监听
        Task(crawler.fetch())
    # 全部套接字的可写事件被监听后，开始进入事件循环
    loop()
    print('总耗时：{:.2f}s'.format(time.time() - start))


if __name__ == '__main__':
    main()
```

将以上代码按顺序写入到 spider_yield.py 文件中，在终端运行文件：

```bash
$ python3 spider_yield.py
URL: https://dn-simplecloud.shiyanlou.com/ncn1.jpg, 耗时: 0.287s
URL: https://dn-simplecloud.shiyanlou.com/1502778396172.png, 耗时: 0.269s
URL: https://dn-simplecloud.shiyanlou.com/1540965522764.png, 耗时: 0.299s
URL: https://dn-simplecloud.shiyanlou.com/ncn109.jpg, 耗时: 0.322s
URL: https://dn-simplecloud.shiyanlou.com/ncn110.jpg, 耗时: 0.340s
URL: https://dn-simplecloud.shiyanlou.com/1543913883545.png, 耗时: 0.379s
URL: https://dn-simplecloud.shiyanlou.com/1546500900109.png, 耗时: 0.425s
URL: https://dn-simplecloud.shiyanlou.com/1547620906601.png, 耗时: 0.478s
URL: https://dn-simplecloud.shiyanlou.com/1517282865454.png, 耗时: 0.490s
URL: https://dn-simplecloud.shiyanlou.com/1548126810319.png, 耗时: 0.491s
总耗时：0.51s
```

由于图片数量少、体积小，所以使用 yield 编写的异步程序和使用 selectors + 回调函数的模式相比多线程模式并无优势，我们要学习的是它们的编程逻辑。

#### 执行过程

简单描述程序的执行过程：

- 首先创建存储图片的目录 pic ，其中 -p 选项可以保证 mkdir 命令不抛异常，如果目录已经存在，没有此选项的话会报错
- 为每个图片的爬取创建 Crawler 实例
- Task 类初始化，将 Crawler 实例的 fetch 方法的调用作为参数，fetch 方法调用即为协程对象
- Task 类在初始化时运行 ___init__ 方法，将协程对象赋值给实例的 coro 属性
- 然后创建一个 Future 类的实例，将此实例作为 step 方法的参数运行 step 方法
- fetch 协程启动，创建 socket 对象，设置为非阻塞的，向服务端发送连接请求，不会进行挂起而是继续向下执行
- 创建一个 future 对象，用来存放未来需要传递的数据
- selector 注册监听客户端套接字的可写事件，并且传入 writable 作为回调函数
- yield 会中断协程执行，返回到启动协程的地方，就是 Task 类中 step 方法中协程调用 send 方法的地方，然后继续向下执行
- send 方法返回 fetch 协程中创建的 future 对象，future 对象接着将 Task 的 step 方法加入到 _step_list 列表里，等待后面调用
- 然后启动下一个爬虫，在所有的 crawler 都启动之后，开始进入 loop 循环
- selector.select 接收到内核的可写事件就绪之后，调用 writable 回调函数，这时又回到了 fetch 协程中
- 由于协程执行过程会保存上下文环境，所以 writable 方法可以调用 future 的 set_result 方法
- 在 writable 方法中设置结果，同时执行 _step_list 列表里的 step 方法
- Task 的 step 方法又会调用 fetch 协程的 send 方法，重新启动协程执行，并且将 future 的 result 值发送给协程
- fetch 方法又获得了控制权，开始从 yield 继续向下执行
- selector 注销 socket 描述符，避免重复事件发生
- 下面注册读事件，进入事件循环，和上面流程一样，直到 fetch 方法执行完毕

使用协程之后，整个执行过程是在一个方法中完成的，回调函数被隐藏了起来，相比于同步模式，逻辑稍微复杂了一点。
‍
