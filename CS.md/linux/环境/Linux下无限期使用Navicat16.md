
> 原文链接：[https://www.zhoubotong.site/post/79.html](https://www.zhoubotong.site/post/79.html)  
> linux 下的数据库图形化工具比较好用的有dbeaver完全免费，相比navicat,我还是习惯了使用navicat操作数据库。

截止目前最新版是navicat16-mysql-cs.AppImage，linux网上有很多navicat破解注册码教程，习惯使用了新版本，懒得折腾去破解了。  
Linux 运行：

```bash
chmod +x navicat16-mysql-cs.AppImage
./navicat16-mysql-cs.AppImage
```

快到期之前，可以把之前连接的数据库通过 文件->导出连接，备份之前的数据库连接即可，下次激活后可以直接导入连接。Navicat Premium 16的试用期只有14天，执行下面两个命令，即可无限使用。

1、关闭Navicat程序  
2、删除如下2个文件：

```bash
rm -rf ~/.config/navicat
rm -rf ~/.config/dconf/user
```

```perl
lsof | grep navicat | grep \\.config  #用于列出当前系统打开navicat的工具
```

再次重新启动navicat即可。

无论从事什么行业，只要做好两件事就够了，一个是你的专业、一个是你的人品，专业决定了你的存在，人品决定了你的人脉，剩下的就是坚持，用善良專業和真诚赢取更多的信任。

