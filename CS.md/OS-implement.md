
## make a img 

首先引入依赖
```
yay -S qemu 
yay -S nasm
```


```asm
org 0x7c00

mov si, hello_message
call print_string

jmp $

print_string:
  lodsb
  or al, al
  jz return
  mov ah, 0x0e
  int 0x10
  jmp print_string

return:
  ret

hello_message db 'Hello, World!', 0

times 510-($-$$) db 0
dw 0xaa55
```

這段代碼將打印 "Hello, World!"，然後無限循環。在最後，我們確保文件大小為 512 字節，並以 0xaa55 結尾，以滿足引導區的要求。

```

nasm -f bin -o bootloader.bin bootloader.asm
接下來，創建一個空的 1.44MB img 文件，並將 `bootloader.bin` 寫入其中：
````shell
dd if=/dev/zero of=ihelloos.img bs=512 count=2880
dd if=bootloader.bin of=ihelloos.img conv=notrunc
```

1.   if you compile succeed ， you will get a binary file named `bootloader.bin`
2.  divide a blank space （1.44 MB）to  make img file， 
3. place `bootloader.bin` in the front of that img file

if there procedures are done 。you can run fellow Command: 
```shell
qemu-system-i386 -drive format=raw,file=ihelloos.img -display gtk
```
the result is : 
.... ...
to do 
........


congratulation!!! ,  you have got a right img file, 
this img layout look like 
+-----------------------+
|      引导扇区 (512 字节)  |
+-----------------------+
|      保留区域 (62 个扇区)|
+-----------------------+
|                                      |
|      根目录区域                |
|                                      |
+-----------------------+
|                                      |
|      数据区域                   |
|                                      |
+-----------------------+

It is worth mentioning that loader section layout is like below： 
+-----------------------+
|      引导加载程序            |
|                                      |
+-----------------------+
|     填充字节 (填充为0)    |
+-----------------------+
|      标识符 (0xaa55)       |
+-----------------------+

引导加载程序可以是简单的程序， 比如 仅仅打印 hello world 
这个加载程序 可以把其他代码 加载到内存。这就是著名的 操作 系统引导程序 `bootloader`


![[Pasted image 20231110112514.png]]
GDT  IDT
中断向量表是一个通过索引（中断号）来访问的数据结构，它将中断号与相应的处理程序地址关联起来。可以将中断向量表视为一个由中断号索引的地址向量，每个索引对应一个中断处理程序。


