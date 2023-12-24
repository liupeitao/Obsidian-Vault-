# java_lang包

![img](assets/document-uid18510labid1118timestamp1500451929664-20221010104650-hqm0366.png)

![两大接口](assets/document-uid79144labid1102timestamp1435844913664-20221010104650-so44ch4.png)

![I/O 概述](assets/document-uid79144labid1113timestamp1436110985475-20221010104650-re736w0.png)

![java.util 层次](assets/document-uid79144labid1089timestamp1435767072006-20221010104650-sngk2nq.png)

![缓冲区](assets/document-uid79144labid1113timestamp1436119302929-20221010104650-06emwto.png)

构造方法：

```java
//[ ] 里的内容代表选填
BufferedInputStream(InputStream in [, int size])
BufferedOutputStream(OutputStream out [, int size])
```

举个例子，将缓冲流与文件流相接：

```java
FileInputStream in = new FileInputStream("file.txt");
FileOutputStream out = new FileOutputStream("file2.txt");
//设置输入缓冲区大小为 256 字节
BufferedInputStream bin = new BufferedInputStream(in,256)
BufferedOutputStream bout = new BufferedOutputStream(out,256)
int len;
byte bArray[] = new byte[256];
len = bin.read(bArray); //len 中得到的是实际读取的长度，bArray 中得到的是数据
```

![缓冲流](assets/document-uid79144labid1113timestamp1436118879920-20221010104650-xewewqy.png)

对于 BufferedOutputStream，只有缓冲区满时，才会将数据真正送到输出流，但可以使用 `flush()` 方法人为地将尚未填满的缓冲区中的数据送出。

例如方法 copy():

```java
public void copy(InputStream in, OutputStream out) throws IOException {
    out = new BufferedOutputStream(out, 4096);
    byte[] buf = new byte[4096];
    int len = in.read(buf);
    while (len != -1) {
    out.write(buf, 0, len);
    len = in.read(buf);
    }
    //最后一次读取得数据可能不到 4096 字节
    out.flush();
}
```
