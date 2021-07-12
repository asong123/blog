---
title: JavaSE-IO文件
urlname: pgubxp
date: '2021-07-12 12:50:55 +0800'
author: 阿松
top: false
hide: false
cover: false
toc: false
mathjax: false
summary: 生活中，你肯定经历过这样的场景。当你编辑一个文本文件，忘记了`ctrl+s` ，可能文件就白白编辑了。当你电脑上插入一个 U 盘，可以把一个视频，拷贝到你的电脑硬盘里。那么数据都是在哪些设备上的呢？键盘、内存、硬盘、外接设备等等。我们把这种数据的传输，可以看做是一种数据的流动，按照流动的方向，以内存为基准，分为`输入input` 和`输出output` ，即流向内存是输入流，流出内存的输出流。
categories: IO
tags:
  - java
  - JavaSE
  - IO
---

# 总结

![](https://cdn.nlark.com/yuque/0/2020/png/1281683/1601637781407-905f92a8-0019-4009-8fc2-997eb3de3f3a.png)

# IO 概述

## 什么是 IO

生活中，你肯定经历过这样的场景。当你编辑一个文本文件，忘记了`ctrl+s` ，可能文件就白白编辑了。当你电脑上插入一个 U 盘，可以把一个视频，拷贝到你的电脑硬盘里。那么数据都是在哪些设备上的呢？键盘、内存、硬盘、外接设备等等。我们把这种数据的传输，可以看做是一种数据的流动，按照流动的方向，以内存为基准，分为`输入input` 和`输出output` ，即流向内存是输入流，流出内存的输出流。
Java 中 I/O 操作主要是指使用`java.io`包下的内容，进行输入、输出操作。**输入**也叫做**读取**数据，**输出**也叫做作**写出**数据。

## IO 的分类

根据数据的流向分为：**输入流**和**输出流**。

- **输入流** ：把数据从`其他设备`上读取到`内存`中的流。
- **输出流** ：把数据从`内存` 中写出到`其他设备`上的流。

格局数据的类型分为：**字节流**和**字符流**。

- **字节流** ：以字节为单位，读写数据的流。
- **字符流** ：以字符为单位，读写数据的流。

## IO 的流向说明图解

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596501042704-a86e00c9-cff1-45c4-aeed-2f7843316608.png#height=205&id=HKhu8&margin=%5Bobject%20Object%5D&name=image.png&originHeight=409&originWidth=621&originalType=binary∶=1&size=264036&status=done&style=none&width=310.5)        ![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596501271739-11adfe07-cfa7-4152-8a75-d422820c1027.png#height=166&id=QluUg&margin=%5Bobject%20Object%5D&name=image.png&originHeight=257&originWidth=549&originalType=binary∶=1&size=14701&status=done&style=none&width=354)

## 1.4 顶级父类们

|                  | **输入流** | 输出流 |
| ---------------- | ---------- | ------ |
| **字节流**       | 字节输入流 |
| **InputStream**  | 字节输出流 |
| **OutputStream** |
| **字符流**       | 字符输入流 |
| **Reader**       | 字符输出流 |
| **Writer**       |

# File 类

## 概述

`java.io.File` 类是文件和目录路径名的抽象表示，主要用于文件和目录的创建、查找和删除等操作。

## 构造方法

- `public File(String pathname)` ：通过将给定的**路径名字符串**转换为抽象路径名来创建新的 File 实例。
- `public File(String parent, String child)` ：从**父路径名字符串和子路径名字符串**创建新的 File 实例。
- `public File(File parent, String child)` ：从**父抽象路径名和子路径名字符串**创建新的 File 实例。
- 构造举例，代码如下：

```java
package IO.File类.demo01;
/**测试构造方法**/
import java.io.File;
public class FileTest {
    public static void main(String[] args) {
        /*文件路径名*/
        String pathname = "E:\\CodePlace\\Java\\idea\\狂神说Java\\JavaSE\\a.txt";
        File file = new File(pathname);

        /*父路径+子路径*/
        String parent = "E:\\CodePlace\\Java\\idea\\狂神说Java\\JavaSE";
        String child = "a.txt";
        File file1 = new File(parent,child);

        /*父级对象和子路径*/
        File parentDir = new File("E:\\CodePlace\\Java\\idea\\狂神说Java\\JavaSE");
        child = "a.txt";
        File file2 = new File(parentDir,child);
    }
}
```

> 小贴士：
> 一个 File 对象代表硬盘中实际存在的一个文件或者目录。
> 无论该路径下是否存在文件或者目录，都不影响 File 对象的创建。

## 常用方法

### 获取功能的方法

- `public String getAbsolutePath()` ：返回此 File 的绝对路径名字符串。
- `public String getPath()` ：将此 File 转换为路径名字符串。
- `public String getName()` ：返回由此 File 表示的文件或目录的名称。
- `public long length()` ：返回由此 File 表示的文件的长度。
  方法演示，代码如下：

```java
package IO.File类.demo01;

import java.io.File;

public class FileMethodTest {
    public static void main(String[] args) {
        File file = new File("E:\\CodePlace\\Java\\idea\\狂神说Java\\JavaSE\\基础语法\\src\\IO\\File类\\demo01\\a.txt");
        //绝对路径名
        System.out.println(file.getAbsolutePath());
        //将file转换为路径名字字符
        System.out.println(file.getPath());
        //文件名
        System.out.println(file.getName());
        //长度
        System.out.println(file.length());

    }
}


E:\CodePlace\Java\idea\狂神说Java\JavaSE\基础语法\src\IO\File类\demo01\a.txt
E:\CodePlace\Java\idea\狂神说Java\JavaSE\基础语法\src\IO\File类\demo01\a.txt
a.txt
26
```

> API 中说明：length()，表示文件的长度。但是 File 对象表示目录，则返回值未指定。

### 绝对路径和相对路径

- **绝对路径**：从盘符开始的路径，这是一个完整的路径。
- **相对路径**：相对于项目目录的路径，这是一个便捷的路径，开发中经常使用。

```java
public class FilePath {
    public static void main(String[] args) {
        // D盘下的bbb.java文件
        File f = new File("D:\\bbb.java");
        System.out.println(f.getAbsolutePath());

        // 项目下的bbb.java文件
        File f2 = new File("bbb.java");
        System.out.println(f2.getAbsolutePath());
    }
}
输出结果：
D:\bbb.java
D:\idea_project_test4\bbb.java
```

### 判断功能的方法

- `public boolean exists()` ：此 File 表示的文件或目录是否实际存在。
- `public boolean isDirectory()` ：此 File 表示的是否为目录。
- `public boolean isFile()` ：此 File 表示的是否为文件。

方法演示，代码如下：

```java
package IO.File类.demo01;

import java.io.File;

public class FileIS {
    public static void main(String[] args) {
        File f = new File("E:\\Data\\1\\income.xlsx");
        System.out.println(f.exists());
        System.out.println(f.isFile());
        System.out.println(f.isDirectory());
        System.out.println(f.length());
    }
}

```

### 创建删除功能的方法

- `public boolean createNewFile()` ：当且仅当具有该名称的文件尚不存在时，创建一个新的空文件。
- `public boolean delete()` ：删除由此 File 表示的文件或目录。
- `public boolean mkdir()` ：创建由此 File 表示的目录。
- `public boolean mkdirs()` ：创建由此 File 表示的目录，包括任何必需但不存在的父目录。

方法演示，代码如下：

```java
package IO.File类.demo01;

import java.io.File;
import java.io.IOException;

public class FileCreateDelete {
    public static void main(String[] args) throws IOException {

        File f = new File("E:\\Data\\1\\income2.xlsx");

        if(f.exists()){
            f.delete();
        }
        System.out.println(f.exists());

        if(!f.exists()){
            f.createNewFile();
        }
        System.out.println(f.exists());

        File f2 = new File("E:\\Data\\1\\income");
        if(!f2.exists()){
            f2.mkdir();
        }
        if(f2.exists()){
            f2.delete();
        }
    }
}
```

> API 中说明：delete 方法，如果此 File 表示目录，则目录必须为空才能删除。

## 目录的遍历

- `public String[] list()` ：返回一个 String 数组，表示该 File 目录中的所有子文件或目录。
- `public File[] listFiles()` ：返回一个 File 数组，表示该 File 目录中的所有的子文件或目录。

```java
package IO.File类.demo02;

import java.io.File;

public class DiGuiFileFor {
    public static void main(String[] args) {
        File dir = new File("E:\\Data");
        printDir(dir);
    }
    public static void printDir(File dir){
        File[] files = dir.listFiles();
        for(File file: files){
            if(file.isFile()){	//是文件
                System.out.println(file.getAbsoluteFile());	//输出文件
            }else {	//是目录，进入递归
                printDir(file);
            }
        }
    }
}
```

> 小贴士：
> 调用 listFiles 方法的 File 对象，表示的必须是实际存在的目录，否则返回 null，无法进行遍历。

# 字节流

## 字节输出流【OutputStream】

`java.io.OutputStream`抽象类是表示字节输出流的所有类的超类，将指定的字节信息写出到目的地。它定义了字节输出流的基本共性功能方法。

- `public void close()` ：关闭此输出流并释放与此流相关联的任何系统资源。
- `public void flush()` ：刷新此输出流并强制任何缓冲的输出字节被写出。
- `public void write(byte[] b)`：将 b.length 字节从指定的字节数组写入此输出流。
- `public void write(byte[] b, int off, int len)` ：从指定的字节数组写入 len 字节，从偏移量 off 开始输出到此输出流。
- `public abstract void write(int b)` ：将指定的字节输出流。
  > 小贴士：
  > close 方法，当完成流的操作时，必须调用此方法，释放系统资源。

## FileOutputStream 类

`OutputStream`有很多子类，我们从最简单的一个子类开始。
`java.io.FileOutputStream`类是文件输出流，用于将数据写出到文件。

### 构造方法

- `public FileOutputStream(File file)`：创建文件输出流以写入由指定的 File 对象表示的文件。
- `public FileOutputStream(String name)`： 创建文件输出流以指定的名称写入文件。

当你创建一个流对象时，必须传入一个文件路径。该路径下，如果没有这个文件，会创建该文件。如果有这个文件，会清空这个文件的数据。

### 写出字节数据

1. **写出字节**：`write(int b)` 方法，每次可以写出一个字节数据，代码使用演示：

   > 小贴士：
   >
   > 1. 虽然参数为 int 类型四个字节，但是只会保留一个字节的信息写出。
   > 1. 流操作完毕后，必须释放系统资源，调用 close 方法，千万记得。

2. **写出字节数组**：`write(byte[] b)`，每次可以写出数组中的数据，代码使用演示：
3. **写出指定长度字节数组**：`write(byte[] b, int off, int len)` ,每次写出从 off 索引开始，len 个字节，代码使用演示：

### 数据追加续写

经过以上的演示，每次程序运行，创建输出流对象，都会清空目标文件中的数据。如何保留目标文件中数据，还能继续添加新数据呢？

- `public FileOutputStream(File file, boolean append)`： 创建文件输出流以写入由指定的 File 对象表示的文件。
- `public FileOutputStream(String name, boolean append)`： 创建文件输出流以指定的名称写入文件。

这两个构造方法，参数中都需要传入一个 boolean 类型的值，`true` 表示追加数据，`false` 表示清空原有数据。这样创建的输出流对象，就可以指定是否追加续写了，代码使用演示：

### 写出换行

Windows 系统里，换行符号是`\r\n` 。把
以指定是否追加续写了，代码使用演示：

> - 回车符`\r`和换行符`\n` ：
>   - 回车符：回到一行的开头（return）。
>   - 换行符：下一行（newline）。
> - 系统中的换行：
>   - Windows 系统里，每行结尾是 `回车+换行` ，即`\r\n`；
>   - Unix 系统里，每行结尾只有 `换行` ，即`\n`；
>   - Mac 系统里，每行结尾是 `回车` ，即`\r`。从 Mac OS X 开始与 Linux 统一。

```java
package IO.字节流.demo01;

import java.io.File;
import java.io.FileOutputStream;
import java.io.IOException;

/*输出流*/
public class FileOutputStreamTest {
    public static void main(String[] args) throws IOException {
        File file= new File("E:\\Data\\1\\a.txt");
        FileOutputStream fos = new FileOutputStream(file);
        //FileOutputStream fos = new FileOutputStream(file,true); //true追加写，默认false:清空后在写
        fos.write(97);
        fos.write(98);
        fos.write(99);

        byte[] b = "学习Java IO流".getBytes();
        fos.write(b);


        byte[] c = "abcde".getBytes();
        fos.write(c,2,2);
        fos.write("\r\n".getBytes());
        byte[] words = {97,98,99,100,101};
        for (int i = 0; i <words.length ; i++) {
            fos.write(words[i]);
            fos.write("\r\n".getBytes());
        }

        fos.close();
    }
}

```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596508800813-2ffd1501-75f9-4de3-ac08-d9792768c6bc.png#height=187&id=y9Zcw&margin=%5Bobject%20Object%5D&name=image.png&originHeight=208&originWidth=415&originalType=binary∶=1&size=11237&status=done&style=none&width=374)

## 字节输入流【InputStream】

`java.io.InputStream`抽象类是表示字节输入流的所有类的超类，可以读取字节信息到内存中。它定义了字节输入流的基本共性功能方法。

- `public void close()` ：关闭此输入流并释放与此流相关联的任何系统资源。
- `public abstract int read()`： 从输入流读取数据的下一个字节。
- `public int read(byte[] b)`： 从输入流中读取一些字节数，并将它们存储到字节数组 b 中 。
  > 小贴士：
  > close 方法，当完成流的操作时，必须调用此方法，释放系统资源。

## FileInputStream 类

`java.io.FileInputStream`类是文件输入流，从文件中读取字节。

### 构造方法

- `FileInputStream(File file)`： 通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的 File 对象 file 命名。
- `FileInputStream(String name)`： 通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的路径名 name 命名。

当你创建一个流对象时，必须传入一个文件路径。该路径下，如果没有该文件,会抛出`FileNotFoundException` 。

- 构造举例，代码如下：

### 读取字节数据

1. **读取字节**：`read`方法，每次可以读取一个字节的数据，提升为 int 类型，读取到文件末尾，返回`-1`，代码使用演示：

   > 小贴士：
   >
   > 1. 虽然读取了一个字节，但是会自动提升为 int 类型。
   > 1. 流操作完毕后，必须释放系统资源，调用 close 方法，千万记得。

2. **使用字节数组读取**：`read(byte[] b)`，每次读取 b 的长度个字节到数组中，返回读取到的有效字节个数，读取到末尾时，返回`-1` ，代码使用演示：
   > 小贴士：
   > 使用数组读取，每次读取多个字节，减少了系统间的 IO 操作次数，从而提高了读写的效率，建议开发中使用。

```java
package IO.字节流.demo01;
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

public class FileInputStreamTest {
    public static void main(String[] args) throws IOException {
        File file = new File("E:\\Data\\1\\b.txt");
        FileInputStream fis = new FileInputStream(file);
        int b;
        while((b = fis.read())!=-1){
            System.out.println((char)b);    ;
        }
        fis.close();
    }
}



package IO.字节流.demo01
import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

public class FISRead {
    public static void main(String[] args) throws IOException {
        File file = new File("E:\\Data\\1\\b.txt");
        FileInputStream fis = new FileInputStream(file);

        int len;
        byte[] c = new byte[2];
        while((len = fis.read(c))!=-1){
            //System.out.println(new String(c));
            System.out.println(new String(c,0,len));
        }
        fis.close();
    }
}
```

## 字节流练习：图片复制

### 复制原理图解

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596519739625-3d16b698-8443-48fc-915d-48ece14269fc.png#height=267&id=XnG4e&margin=%5Bobject%20Object%5D&name=image.png&originHeight=533&originWidth=1148&originalType=binary∶=1&size=370877&status=done&style=none&width=574)

### 案例实现

复制图片文件，代码使用演示：

```java
package IO.字节流.demo01;
import java.io.File;
import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class PictureCopy {
    public static void main(String[] args) throws IOException {
        File file = new File("E:\\Data\\1\\test.png");
        File file2 = new File("E:\\Data\\1\\testcopy.png");
        FileInputStream fis = new FileInputStream(file);
        FileOutputStream fos = new FileOutputStream(file2);
        int b;
        while((b = fis.read())!=-1){
            fos.write(b);
        }
        fis.close();
        fos.close();
    }
}
```

> 小贴士：
> 流的关闭原则：先开后关，后开先关。

# 字符流

#

    当使用字节流读取文本文件时，可能会有一个小问题。**就是遇到中文字符时，可能不会显示完整的字符**，那是因为一个中文字符可能占用多个字节存储。所以Java提供一些字符流类，以字符为单位读写数据，专门用于处理文本文件。

## 字符输入流【Reader】

`java.io.Reader`抽象类是表示用于读取字符流的所有类的超类，可以读取字符信息到内存中。它定义了字符输入流的基本共性功能方法。

- `public void close()` ：关闭此流并释放与此流相关联的任何系统资源。
- `public int read()`： 从输入流读取一个字符。
- `public int read(char[] cbuf)`： 从输入流中读取一些字符，并将它们存储到字符数组 cbuf 中 。

## FileReader 类

`java.io.FileReader`类是读取字符文件的便利类。构造时使用系统默认的字符编码和默认字节缓冲区。

> 小贴士：
>
> 1. 字符编码：字节与字符的对应规则。Windows 系统的中文编码默认是 GBK 编码表。
>    idea 中 UTF-8
> 1. 字节缓冲区：一个字节数组，用来临时存储字节数据。

### 构造方法

- `FileReader(File file)`： 创建一个新的 FileReader ，给定要读取的 File 对象。
- `FileReader(String fileName)`： 创建一个新的 FileReader ，给定要读取的文件的名称。

当你创建一个流对象时，必须传入一个文件路径。类似于 FileInputStream 。

- 构造举例，代码如下：

### 读取字符数据

1. **读取字符**：`read`方法，每次可以读取一个字符的数据，提升为 int 类型，读取到文件末尾，返回`-1`，循环读取，代码使用演示：

   > 小贴士：虽然读取了一个字符，但是会自动提升为 int 类型。

2. **使用字符数组读取**：`read(char[] cbuf)`，每次读取 b 的长度个字符到数组中，返回读取到的有效字符个数，读取到末尾时，返回`-1` ，代码使用演示：

```java
package IO.字符流;

import java.io.File;
import java.io.FileReader;
import java.io.IOException;

public class FileReaderTest {
    public static void main(String[] args) throws IOException {
        File file = new File("E:\\Data\\1\\c.txt");
        FileReader fr = new FileReader(file);

        int b;
        while((b = fr.read())!= -1){
            System.out.print((char)b);
        }
        fr.close();
        fr = new FileReader(file);
        int len;
        char[] cbuf  = new char[1024];
        while((len = fr.read(cbuf)) != -1){
            System.out.println(new String(cbuf,0,len));
        }
        fr.close();
    }
}
```

## 字符输出流【Writer】

`java.io.Writer`抽象类是表示用于写出字符流的所有类的超类，将指定的字符信息写出到目的地。它定义了字节输出流的基本共性功能方法。

- `void write(int c)` 写入单个字符。
- `void write(char[] cbuf)`写入字符数组。
- `abstract void write(char[] cbuf, int off, int len)`写入字符数组的某一部分,off 数组的开始索引,len 写的字符个数。
- `void write(String str)`写入字符串。
- `void write(String str, int off, int len)` 写入字符串的某一部分,off 字符串的开始索引,len 写的字符个数。
- `void flush()`刷新该流的缓冲。
- `void close()` 关闭此流，但要先刷新它。

## FileWriter 类

`java.io.FileWriter`类是写出字符到文件的便利类。构造时使用系统默认的字符编码和默认字节缓冲区。

### 构造方法

- `FileWriter(File file)`： 创建一个新的 FileWriter，给定要读取的 File 对象。
- `FileWriter(String fileName)`： 创建一个新的 FileWriter，给定要读取的文件的名称。

当你创建一个流对象时，必须传入一个文件路径，类似于 FileOutputStream。

- 构造举例，代码如下：

### 基本写出数据

**写出字符**：`write(int b)` 方法，每次可以写出一个字符数据，代码使用演示：

> 小贴士：
> 虽然参数为 int 类型四个字节，但是只会保留一个字符的信息写出。
> 未调用 close 方法，数据只是保存到了缓冲区，并未写出到文件中。

### 关闭和刷新

因为内置缓冲区的原因，如果不关闭输出流，无法写出字符到文件中。但是关闭的流对象，是无法继续写出数据的。如果我们既想写出数据，又想继续使用流，就需要`flush` 方法了。

- `flush` ：刷新缓冲区，流对象可以继续使用。
- `close`:先刷新缓冲区，然后通知系统释放资源。流对象不可以再被使用了。

代码使用演示：

> 小贴士：即便是 flush 方法写出了数据，操作的最后还是要调用 close 方法，释放系统资源。

### 写出其他数据

**1.写出字符数组** ：`write(char[] cbuf)` 和 `write(char[] cbuf, int off, int len)` ，每次可以写出字符数组中的数据，用法类似 FileOutputStream，代码使用演示：
**2.写出字符串**：`write(String str)` 和 `write(String str, int off, int len)` ，每次可以写出字符串中的数据，更为方便，代码使用演示：
**3.续写和换行**：操作类似于 FileOutputStream。

> 小贴士：字符流，只能操作文本文件，不能操作图片，视频等非文本文件。
> 当我们单纯读或者写文本文件时 使用字符流 其他情况使用字节流

```java
package IO.字符流;

import java.io.File;
import java.io.FileWriter;
import java.io.IOException;

public class FileWriterTest {
    public static void main(String[] args) throws IOException {
        File file = new File("E:\\Data\\1\\d.txt");
        FileWriter fw = new FileWriter(file);

        /*
        写入单个字符
         */
        fw.write(97);
        fw.write('b');
        fw.write('c');
        fw.write(30000);
        //fw.flush();   //刷新缓冲区数据到文件
        fw.close();  //不刷新也不不关闭的话，数据只能保存在缓冲区，并未写入文件

        /*
        写入字符数组
         */
        fw = new FileWriter(file,true);
        char[] wbuf = "Java程序员".toCharArray();
        fw.write(wbuf);
        fw.write('学');
        fw.write(wbuf,0,4);
        fw.close();
    }
}
```

# 属性集

## 概述

`java.util.Properties` 继承于`**Hashtable**`\*\* \*\*，来表示一个持久的属性集。它使用键值结构存储数据，每个键及其对应值都是一个字符串。该类也被许多 Java 类使用，比如获取系统属性时，`System.getProperties` 方法就是返回一个`Properties`对象。

## Properties 类

### 构造方法

- `public Properties()` :创建一个空的属性列表。

### 基本的存储方法

- `public Object setProperty(String key, String value)` ： 保存一对属性。
- `public String getProperty(String key)` ：使用此属性列表中指定的键搜索属性值。
- `public Set<String> stringPropertyNames()` ：所有键的名称的集合。

```java
package IO.属性集;

import java.util.Properties;
import java.util.Set;

public class ProDemo {
    public static void main(String[] args) {
        Properties properties = new Properties();
        properties.setProperty("filename","a.txt");
        properties.setProperty("length","1024");
        properties.setProperty("location","E:\\Data\\1");
        System.out.println(properties);
        Set<String> strings = properties.stringPropertyNames();
        for(String key: strings){
            System.out.println(key+" ---- "+properties.getProperty(key));
        }
    }
}

输出结果：
{filename=a.txt, length=209385038, location=D:\a.txt}
a.txt
209385038
D:\a.txt
filename -- a.txt
length -- 209385038
location -- D:\a.txt
```

### 与流相关的方法

- `public void load(InputStream inStream)`： 从字节输入流中读取键值对。

参数中使用了字节输入流，通过流对象，可以关联到某文件上，这样就能够加载文本中的数据了。文本数据格式:

```java
name=张三
age=20
sex=男
```

加载代码演示：

```java
package IO.属性集;


import java.io.FileReader;
import java.io.IOException;
import java.util.Properties;
import java.util.Set;

public class ProDemo02 {
    public static void main(String[] args) throws IOException {
        Properties properties = new Properties();
        properties.load(new FileReader("ccc.txt"));

        Set<String> strings = properties.stringPropertyNames();
        for(String key:strings){
            System.out.println(key+" ---- "+properties.getProperty(key));
        }
    }
}
输出结果：
sex ---- 男
name ---- 张三
age ---- 20
```

> 小贴士：文本中的数据，必须是键值对形式，可以使用空格、等号、冒号等符号分隔。

# ​

# 缓冲流

## 概述

缓冲流,也叫高效流，是对 4 个基本的`FileXxx` 流的增强，所以也是 4 个流，按照数据类型分类：

- **字节缓冲流**：`BufferedInputStream`，`BufferedOutputStream`
- **字符缓冲流**：`BufferedReader`，`BufferedWriter`

**缓冲流的基本原理: 是在创建流对象时，会创建一个内置的默认大小的缓冲区数组，通过缓冲区读写，减少系统 IO 次数，从而提高读写的效率。**

## 字节缓冲流

- `public BufferedInputStream(InputStream in)` ：创建一个 新的缓冲输入流。
- `public BufferedOutputStream(OutputStream out)`： 创建一个新的缓冲输出流。

​

下面通过一个实例比较不适用缓冲流和使用缓冲流的效率

- 使用字节流

```java
package IO.缓冲流;

import java.io.FileInputStream;
import java.io.FileNotFoundException;
import java.io.FileOutputStream;
import java.io.IOException;

public class BufferredDemo {
    public static void main(String[] args) {
        long start = System.currentTimeMillis();
        try(
                FileInputStream fis = new FileInputStream("E:\\Data\\1\\diamonds.csv");
                FileOutputStream fos = new FileOutputStream("E:\\Data\\1\\diamonds副本.csv")
        ){
          int b;
          while ((b = fis.read())!=-1){
              fos.write(b);
          }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
        long end = System.currentTimeMillis();
        System.out.println("time:"+ (end-start)+" ms");
    }
}
time:12990 ms
```

- 使用字节缓冲流

```java
package IO.缓冲流;

import java.io.*;

public class BufferedDemo2 {
    public static void main(String[] args) {
        long start = System.currentTimeMillis();
        try(
                BufferedInputStream bis = new BufferedInputStream( new FileInputStream("E:\\Data\\1\\diamonds.csv"));
                BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("E:\\Data\\1\\diamonds副本.csv"))
        ){
            int b;
            while ((b = bis.read())!=-1){
                bos.write(b);
            }
        } catch (FileNotFoundException e) {
            e.printStackTrace();
        } catch (IOException e) {
            e.printStackTrace();
        }
        long end = System.currentTimeMillis();
        System.out.println("time:"+ (end-start)+" ms");
    }
}

time:189 ms
```

**很明显字节缓冲流的速度比字节流快了数倍。**
**​**

## 字符缓冲流

- `public BufferedReader(Reader in)` ：创建一个 新的缓冲输入流。
- `public BufferedWriter(Writer out)`： 创建一个新的缓冲输出流。

##

下面通过一个例子看看字符缓冲流：**请将文本信息恢复顺序。**
**案例分析**

```
3.侍中、侍郎郭攸之、费祎、董允等，此皆良实，志虑忠纯，是以先帝简拔以遗陛下。愚以为宫中之事，事无大小，悉以咨之，然后施行，必得裨补阙漏，有所广益。
8.愿陛下托臣以讨贼兴复之效，不效，则治臣之罪，以告先帝之灵。若无兴德之言，则责攸之、祎、允等之慢，以彰其咎；陛下亦宜自谋，以咨诹善道，察纳雅言，深追先帝遗诏，臣不胜受恩感激。
4.将军向宠，性行淑均，晓畅军事，试用之于昔日，先帝称之曰能，是以众议举宠为督。愚以为营中之事，悉以咨之，必能使行阵和睦，优劣得所。
2.宫中府中，俱为一体，陟罚臧否，不宜异同。若有作奸犯科及为忠善者，宜付有司论其刑赏，以昭陛下平明之理，不宜偏私，使内外异法也。
1.先帝创业未半而中道崩殂，今天下三分，益州疲弊，此诚危急存亡之秋也。然侍卫之臣不懈于内，忠志之士忘身于外者，盖追先帝之殊遇，欲报之于陛下也。诚宜开张圣听，以光先帝遗德，恢弘志士之气，不宜妄自菲薄，引喻失义，以塞忠谏之路也。
9.今当远离，临表涕零，不知所言。
6.臣本布衣，躬耕于南阳，苟全性命于乱世，不求闻达于诸侯。先帝不以臣卑鄙，猥自枉屈，三顾臣于草庐之中，咨臣以当世之事，由是感激，遂许先帝以驱驰。后值倾覆，受任于败军之际，奉命于危难之间，尔来二十有一年矣。
7.先帝知臣谨慎，故临崩寄臣以大事也。受命以来，夙夜忧叹，恐付托不效，以伤先帝之明，故五月渡泸，深入不毛。今南方已定，兵甲已足，当奖率三军，北定中原，庶竭驽钝，攘除奸凶，兴复汉室，还于旧都。此臣所以报先帝而忠陛下之职分也。至于斟酌损益，进尽忠言，则攸之、祎、允之任也。
5.亲贤臣，远小人，此先汉所以兴隆也；亲小人，远贤臣，此后汉所以倾颓也。先帝在时，每与臣论此事，未尝不叹息痛恨于桓、灵也。侍中、尚书、长史、参军，此悉贞良死节之臣，愿陛下亲之信之，则汉室之隆，可计日而待也。
```

1. 逐行读取文本信息。
1. 解析文本信息到集合中。
1. 遍历集合，按顺序，写出文本信息。

**案例实现**

```java
package IO.缓冲流;

import java.io.*;
import java.util.HashMap;

public class ChuShiBiaoTest {
    public static void main(String[] args) throws IOException {
        HashMap<String,String> linemap = new HashMap<>();
        BufferedReader br = new BufferedReader(new FileReader("E:\\Data\\1\\出师表.txt"));
        BufferedWriter bw = new BufferedWriter(new FileWriter("E:\\Data\\1\\出师表-排序.txt"));

        String line = null;
        while((line = br.readLine())!=null){
            String[] spilt = line.split("\\.");
            linemap.put(spilt[0],spilt[1]);
        }
        br.close();
        for (int i = 1; i <=linemap.size(); i++) {
            String key = String.valueOf(i);
            String value = linemap.get(key);
            bw.write(key+"."+value);	//排序写入
        }
        bw.close();
    }
}
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596530084306-b094a6f6-c871-4507-8097-de3fac536902.png#height=249&id=AxIfN&margin=%5Bobject%20Object%5D&name=image.png&originHeight=498&originWidth=1557&originalType=binary∶=1&size=300172&status=done&style=none&width=778.5)

# 转换流

## 字符编码和字符集

**字符编码**
计算机中储存的信息都是用二进制数表示的，而我们在屏幕上看到的数字、英文、标点符号、汉字等字符是二进制数转换之后的结果。按照某种规则，将字符存储到计算机中，称为**编码** 。反之，将存储在计算机中的二进制数按照某种规则解析显示出来，称为**解码** 。比如说，按照 A 规则存储，同样按照 A 规则解析，那么就能显示正确的文本符号。反之，按照 A 规则存储，再按照 B 规则解析，就会导致乱码现象。
**编码:    **字符(能看懂的)--字节(看不懂的)
**解码:    **字节(看不懂的)-->字符(能看懂的)

- **字符编码**`**Character Encoding**` : 就是一套自然语言的字符与二进制数之间的对应规则。
  编码表: 生活中文字和计算机中二进制的对应规则

**字符集**

- **字符集 **`**Charset**`：也叫编码表。是一个系统支持的所有字符的集合，包括各国家文字、标点符号、图形符号、数字等。

计算机要准确的存储和识别各种字符集符号，需要进行字符编码，一套字符集必然至少有一套字符编码。常见字符集有 ASCII 字符集、GBK 字符集、Unicode 字符集等。
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596530228525-c9f8168c-f926-4358-b42d-53b97c8d1eb3.png#height=185&id=wDDrr&margin=%5Bobject%20Object%5D&name=image.png&originHeight=369&originWidth=1013&originalType=binary∶=1&size=178016&status=done&style=none&width=506.5)
可见，当指定了**编码**，它所对应的**字符集**自然就指定了，所以**编码**才是我们最终要关心的。

- **ASCII 字符集** ：
  - ASCII（American Standard Code for Information Interchange，美国信息交换标准代码）是基于拉丁字母的一套电脑编码系统，用于显示现代英语，主要包括控制字符（回车键、退格、换行键等）和可显示字符（英文大小写字符、阿拉伯数字和西文符号）。
  - 基本的 ASCII 字符集，使用 7 位（bits）表示一个字符，共 128 字符。ASCII 的扩展字符集使用 8 位（bits）表示一个字符，共 256 字符，方便支持欧洲常用字符。
- **ISO-8859-1 字符集**：
  - 拉丁码表，别名 Latin-1，用于显示欧洲使用的语言，包括荷兰、丹麦、德语、意大利语、西班牙语等。
  - ISO-8859-1 使用单字节编码，兼容 ASCII 编码。
- **GBxxx 字符集**：
  - GB 就是国标的意思，是为了显示中文而设计的一套字符集。
  - **GB2312**：简体中文码表。一个小于 127 的字符的意义与原来相同。但两个大于 127 的字符连在一起时，就表示一个汉字，这样大约可以组合了包含 7000 多个简体汉字，此外数学符号、罗马希腊的字母、日文的假名们都编进去了，连在 ASCII 里本来就有的数字、标点、字母都统统重新编了两个字节长的编码，这就是常说的"全角"字符，而原来在 127 号以下的那些就叫"半角"字符了。
  - **GBK**：最常用的中文码表。是在 GB2312 标准基础上的扩展规范，使用了双字节编码方案，共收录了 21003 个汉字，完全兼容 GB2312 标准，同时支持繁体汉字以及日韩汉字等。
  - **GB18030**：最新的中文码表。收录汉字 70244 个，采用多字节编码，每个字可以由 1 个、2 个或 4 个字节组成。支持中国国内少数民族的文字，同时支持繁体汉字以及日韩汉字等。
- **Unicode 字符集** ：
  - Unicode 编码系统为表达任意语言的任意字符而设计，是业界的一种标准，也称为统一码、标准万国码。
  - 它最多使用 4 个字节的数字来表达每个字母、符号，或者文字。有三种编码方案，UTF-8、UTF-16 和 UTF-32。最为常用的 UTF-8 编码。
  - UTF-8 编码，可以用来表示 Unicode 标准中任何字符，它是电子邮件、网页及其他存储或传送文字的应用中，优先采用的编码。互联网工程工作小组（IETF）要求所有互联网协议都必须支持 UTF-8 编码。所以，我们开发 Web 应用，也要使用 UTF-8 编码。它使用一至四个字节为每个字符编码，编码规则：
    1. 128 个 US-ASCII 字符，只需一个字节编码。
    1. 拉丁文等字符，需要二个字节编码。
    1. 大部分常用字（含中文），使用三个字节编码。
    1. 其他极少使用的 Unicode 辅助字符，使用四字节编码。

**编码引出的问题**
在 IDEA 中，使用`FileReader` 读取项目中的文本文件。由于 IDEA 的设置，都是默认的`UTF-8`编码，所以没有任何问题。但是，当读取 Windows 系统中创建的文本文件时，由于 Windows 系统的默认是 GBK 编码，就会出现乱码。

```java
public class ReaderDemo {
    public static void main(String[] args) throws IOException {
        FileReader fileReader = new FileReader("E:\\File_GBK.txt");
        int read;
        while ((read = fileReader.read()) != -1) {
            System.out.print((char)read);
        }
        fileReader.close();
    }
}

输出结果：
���
```

那么如何读取 GBK 编码的文件呢？

## InputStreamReader 类

转换流`java.io.InputStreamReader`，是 Reader 的子类，是从字节流到字符流的桥梁。它读取字节，并使用指定的字符集将其解码为字符。它的字符集可以由名称指定，也可以接受平台的默认字符集。
**构造方法**

- `InputStreamReader(InputStream in)`: 创建一个使用默认字符集的字符流。
- `InputStreamReader(InputStream in, String charsetName)`: 创建一个指定字符集的字符流。

构造举例，代码如下：
` InputStreamReader`` ``isr`` ``=`` ``new`` ``InputStreamReader``(``new`` ``FileInputStream``(``"in.txt"``)); `
` InputStreamReader`` ``isr2`` ``=`` ``new`` ``InputStreamReader``(``new`` ``FileInputStream``(``"in.txt"``) , ``"GBK"``); `
**指定编码读取**

```java
public class ReaderDemo2 {
    public static void main(String[] args) throws IOException {
        // 定义文件路径,文件为gbk编码
        String FileName = "E:\\file_gbk.txt";
        // 创建流对象,默认UTF8编码
        InputStreamReader isr = new InputStreamReader(new FileInputStream(FileName));
        // 创建流对象,指定GBK编码
        InputStreamReader isr2 = new InputStreamReader(new FileInputStream(FileName) , "GBK");
        // 定义变量,保存字符
        int read;
        // 使用默认编码字符流读取,乱码
        while ((read = isr.read()) != -1) {
            System.out.print((char)read); // ��Һ�
        }
        isr.close();

        // 使用指定编码字符流读取,正常解析
        while ((read = isr2.read()) != -1) {
            System.out.print((char)read);// 大家好
        }
        isr2.close();
    }
}
```

## OutputStreamWriter 类

转换流`java.io.OutputStreamWriter` ，是 Writer 的子类，是从字符流到字节流的桥梁。使用指定的字符集将字符编码为字节。它的字符集可以由名称指定，也可以接受平台的默认字符集。
**构造方法**

- `OutputStreamWriter(OutputStream in)`: 创建一个使用默认字符集的字符流。
- `OutputStreamWriter(OutputStream in, String charsetName)`: 创建一个指定字符集的字符流。

构造举例，代码如下：
` OutputStreamWriter`` ``isr`` ``=`` ``new`` ``OutputStreamWriter``(``new`` ``FileOutputStream``(``"out.txt"``)); `
` OutputStreamWriter`` ``isr2`` ``=`` ``new`` ``OutputStreamWriter``(``new`` ``FileOutputStream``(``"out.txt"``) , ``"GBK"``); `
**指定编码写出**

```java
public class OutputDemo {
    public static void main(String[] args) throws IOException {
        // 定义文件路径
        String FileName = "E:\\out.txt";
        // 创建流对象,默认UTF8编码
        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream(FileName));
        // 写出数据
        osw.write("你好"); // 保存为6个字节
        osw.close();

        // 定义文件路径
        String FileName2 = "E:\\out2.txt";
        // 创建流对象,指定GBK编码
        OutputStreamWriter osw2 = new OutputStreamWriter(new FileOutputStream(FileName2),"GBK");
        // 写出数据
        osw2.write("你好");// 保存为4个字节
        osw2.close();
    }
}
```

**转换流理解图解**
![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596530279010-415d349f-81d8-47e2-8af0-47df06d9fed3.png#height=173&id=W0Q9c&margin=%5Bobject%20Object%5D&name=image.png&originHeight=345&originWidth=1209&originalType=binary∶=1&size=200486&status=done&style=none&width=604.5)

## 练习：转换文件编码

将 GBK 编码的文本文件，转换为 UTF-8 编码的文本文件。

1.  指定 GBK 编码的转换流，读取文本文件。
1.  使用 UTF-8 编码的转换流，写出文本文件。

```java
package IO.转换流;

import java.io.*;

public class TransDemo {
    public static void main(String[] args) throws IOException{

        String srcFile = "file_gbk.txt";
        String destFile = "file_utf8.txt";

        InputStreamReader isr = new InputStreamReader(new FileInputStream(srcFile),"GBK");
        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream(destFile));

        char[] cbuf = new char[1024];
        int len;
        while ((len = isr.read(cbuf))!=-1) {
            osw.write(cbuf,0,len);
        }

        osw.close();
        isr.close();
    }
}
```

![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596531888789-cac83808-2e8b-427d-a972-9283a361ab2f.png#height=69&id=MZakd&margin=%5Bobject%20Object%5D&name=image.png&originHeight=137&originWidth=607&originalType=binary∶=1&size=11369&status=done&style=none&width=303.5)![image.png](https://cdn.nlark.com/yuque/0/2020/png/1281683/1596531907587-c478e397-ad8a-4057-8767-f751941d3ed2.png#height=73&id=P6m4N&margin=%5Bobject%20Object%5D&name=image.png&originHeight=146&originWidth=489&originalType=binary∶=1&size=11302&status=done&style=none&width=244.5)

# 序列化

​

Java 提供了一种对象**序列化**的机制。用一个字节序列可以表示一个对象，该字节序列包含该`对象的数据`、`对象的类型`和`对象中存储的属性`等信息。字节序列写出到文件之后，相当于文件中**持久保存**了一个对象的信息。
反之，该字节序列还可以从文件中读取回来，重构对象，对它进行**反序列化**。`对象的数据`、`对象的类型`和`对象中存储的数据`信息，都可以用来在内存中创建对象。看图理解序列化：

## ObjectOutputStream 类

`java.io.ObjectOutputStream` 类，将 Java 对象的原始数据类型写出到文件,实现对象的持久存储。
**构造方法**

- `public ObjectOutputStream(OutputStream out)`： 创建一个指定 OutputStream 的 ObjectOutputStream。

构造举例，代码如下：  
` FileOutputStream`` ``fileOut`` ``=`` ``new`` ``FileOutputStream``(``"employee.txt"``); `
` ObjectOutputStream`` ``out`` ``=`` ``new`` ``ObjectOutputStream``(``fileOut``); `
**序列化操作**

1. 一个对象要想序列化，必须满足两个条件:

- 该类必须实现`java.io.Serializable` 接口，`Serializable` 是一个标记接口，不实现此接口的类将不会使任何状态序列化或反序列化，会抛出`NotSerializableException` 。
- 该类的所有属性必须是可序列化的。如果有一个属性不需要可序列化的，则该属性必须注明是瞬态的，使用`transient` 关键字修饰。

```java
public class Employee implements java.io.Serializable {
    public String name;
    public String address;
    public transient int age; // transient瞬态修饰成员,不会被序列化
    public void addressCheck() {
        System.out.println("Address  check : " + name + " -- " + address);
    }
}
```

2.写出对象方法

- `public final void writeObject (Object obj)` : 将指定的对象写出。

```java
public class SerializeDemo{
    public static void main(String [] args)   {
        Employee e = new Employee();
        e.name = "zhangsan";
        e.address = "beiqinglu";
        e.age = 20;
        try {
            // 创建序列化流对象
          ObjectOutputStream out = new ObjectOutputStream(new FileOutputStream("employee.txt"));
            // 写出对象
            out.writeObject(e);
            // 释放资源
            out.close();
            fileOut.close();
            System.out.println("Serialized data is saved"); // 姓名，地址被序列化，年龄没有被序列化。
        } catch(IOException i)   {
            i.printStackTrace();
        }
    }
}
输出结果：
Serialized data is saved
```

## ObjectInputStream 类

ObjectInputStream 反序列化流，将之前使用 ObjectOutputStream 序列化的原始数据恢复为对象。
**构造方法**

- `public ObjectInputStream(InputStream in)`： 创建一个指定 InputStream 的 ObjectInputStream。

**反序列化操作 1**
如果能找到一个对象的 class 文件，我们可以进行反序列化操作，调用`ObjectInputStream`读取对象的方法：

- `public final Object readObject ()` : 读取一个对象。

```java
public class DeserializeDemo {
   public static void main(String [] args)   {
        Employee e = null;
        try {
             // 创建反序列化流
             FileInputStream fileIn = new FileInputStream("employee.txt");
             ObjectInputStream in = new ObjectInputStream(fileIn);
             // 读取一个对象
             e = (Employee) in.readObject();
             // 释放资源
             in.close();
             fileIn.close();
        }catch(IOException i) {
             // 捕获其他异常
             i.printStackTrace();
             return;
        }catch(ClassNotFoundException c)  {
            // 捕获类找不到异常
             System.out.println("Employee class not found");
             c.printStackTrace();
             return;
        }
        // 无异常,直接打印输出
        System.out.println("Name: " + e.name);  // zhangsan
        System.out.println("Address: " + e.address); // beiqinglu
        System.out.println("age: " + e.age); // 0
    }
}
```

**对于 JVM 可以反序列化对象，它必须是能够找到 class 文件的类。如果找不到该类的 class 文件，则抛出一个 **`**ClassNotFoundException**`** 异常。**
**反序列化操作 2**
**另外，当 JVM 反序列化对象时，能找到 class 文件，但是 class 文件在序列化对象之后发生了修改，那么反序列化操作也会失败，抛出一个**`**InvalidClassException**`**异常。**发生这个异常的原因如下：

- 该类的序列版本号与从流中读取的类描述符的版本号不匹配
- 该类包含未知数据类型
- 该类没有可访问的无参数构造方法

`Serializable` 接口给需要序列化的类，提供了一个序列版本号。`serialVersionUID` 该版本号的目的在于验证序列化的对象和对应类是否版本匹配。

```java
public class Employee implements java.io.Serializable {
     // 加入序列版本号
     private static final long serialVersionUID = 1L;
     public String name;
     public String address;
     // 添加新的属性 ,重新编译, 可以反序列化,该属性赋为默认值.
     public int eid;
     public void addressCheck() {
         System.out.println("Address  check : " + name + " -- " + address);
     }
}
```

## 练习：序列化集合

1. 将存有多个自定义对象的集合序列化操作，保存到`list.txt`文件中。
1. 反序列化`list.txt` ，并遍历集合，打印对象信息。

**案例分析**

1. 把若干学生对象 ，保存到集合中。
1. 把集合序列化。
1. 反序列化读取时，只需要读取一次，转换为集合类型。
1. 遍历集合，可以打印所有的学生信息

**案例实现**

```java
public class SerTest {
    public static void main(String[] args) throws Exception {
        // 创建 学生对象
        Student student = new Student("老王", "laow");
        Student student2 = new Student("老张", "laoz");
        Student student3 = new Student("老李", "laol");
        ArrayList<Student> arrayList = new ArrayList<>();
        arrayList.add(student);
        arrayList.add(student2);
        arrayList.add(student3);
        // 序列化操作
        // serializ(arrayList);

        // 反序列化
        ObjectInputStream ois  = new ObjectInputStream(new FileInputStream("list.txt"));
        // 读取对象,强转为ArrayList类型
        ArrayList<Student> list  = (ArrayList<Student>)ois.readObject();

        for (int i = 0; i < list.size(); i++ ){
            Student s = list.get(i);
            System.out.println(s.getName()+"--"+ s.getPwd());
        }
    }
    private static void serializ(ArrayList<Student> arrayList) throws Exception {
        // 创建 序列化流
        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("list.txt"));
        // 写出对象
        oos.writeObject(arrayList);
        // 释放资源
        oos.close();
    }
}
```

# 打印流

平时我们在控制台打印输出，是调用`print`方法和`println`方法完成的，这两个方法都来自`java.io.PrintStream`类，该类能够方便地打印各种数据类型的值，是一种便捷的输出方式。

## PrintStream 类

**构造方法**

- `public PrintStream(String fileName)`： 使用指定的文件名创建一个新的打印流。

构造举例，代码如下：  
`PrintStream ps = new PrintStream("ps.txt")；`
**改变打印流向**
`System.out`就是`PrintStream`类型的，只不过它的流向是系统规定的，打印在控制台上。不过，既然是流对象，我们就可以玩一个"小把戏"，改变它的流向。

```java
public class PrintDemo {
    public static void main(String[] args) throws IOException {
 	// 调用系统的打印流,控制台直接输出97
        System.out.println(97);

 	// 创建打印流,指定文件的名称
        PrintStream ps = new PrintStream("ps.txt");

       // 设置系统的打印流流向,输出到ps.txt
        System.setOut(ps);
       // 调用系统的打印流,ps.txt中输出97
        System.out.println(97);
    }
}
```

# IO 异常的处理

### JDK7 前处理

之前的入门练习，我们一直把异常抛出，而实际开发中并不能这样处理，建议使用`try...catch...finally` 代码块，处理异常部分，代码使用演示：

```java
public class HandleException1 {
    public static void main(String[] args) {
        // 声明变量
        FileWriter fw = null;
        try {
            //创建流对象
            fw = new FileWriter("fw.txt");
            // 写出数据
            fw.write("黑马程序员"); //黑马程序员
        } catch (IOException e) {
            e.printStackTrace();
        } finally {
            try {
                if (fw != null) {
                    fw.close();
                }
            } catch (IOException e) {
                e.printStackTrace();
            }
        }
    }
}
```

### JDK7 的处理(扩展知识点了解内容)

还可以使用 JDK7 优化后的`try-with-resource` 语句，该语句确保了每个资源在语句结束时关闭。所谓的资源（resource）是指在程序完成后，必须关闭的对象。格式：

```java
try (创建流对象语句，如果多个,使用';'隔开) {
    // 读写数据
} catch (IOException e) {
    e.printStackTrace();
}
代码使用演示：
public class HandleException2 {
    public static void main(String[] args) {
        // 创建流对象
        try ( FileWriter fw = new FileWriter("fw.txt"); ) {
            // 写出数据
            fw.write("黑马程序员"); //黑马程序员
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```

### JDK9 的改进(扩展知识点了解内容)

JDK9 中`try-with-resource` 的改进，对于**引入对象**的方式，支持的更加简洁。被引入的对象，同样可以自动关闭，无需手动 close，我们来了解一下格式。

```java
改进前格式：
// 被final修饰的对象
final Resource resource1 = new Resource("resource1");
// 普通对象
Resource resource2 = new Resource("resource2");
// 引入方式：创建新的变量保存
try (Resource r1 = resource1;
     Resource r2 = resource2) {
     // 使用对象
}
改进后格式：
// 被final修饰的对象
final Resource resource1 = new Resource("resource1");
// 普通对象
Resource resource2 = new Resource("resource2");
// 引入方式：直接引入
try (resource1; resource2) {
     // 使用对象
}
改进后，代码使用演示：
public class TryDemo {
    public static void main(String[] args) throws IOException {
        // 创建流对象
        final  FileReader fr  = new FileReader("in.txt");
        FileWriter fw = new FileWriter("out.txt");
        // 引入到try中
        try (fr; fw) {
            // 定义变量
            int b;
            // 读取数据
            while ((b = fr.read())!=-1) {
                // 写出数据
                fw.write(b);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }
    }
}
```
