## JavaSE_第14章 File类与IO流

### 1 概述

File类是[http://java.io](https://link.zhihu.com/?target=http%3A//java.io)包下代表与平台无关的文件和目录，也就是说如果希望在程序中操作文件和目录都可以通过File类来完成，File类能新建、删除、重命名文件和目录。

在API中File的解释是文件和目录路径名的抽象表示形式，即File类是文件或目录的路径，而不是文件本身，因此File类不能直接访问文件内容本身，如果需要访问文件内容本身，则需要使用输入/输出流。

### 2 构造方法

- `public File(String pathname)` ：通过将给定的**路径名字符串**转换为抽象路径名来创建新的 File实例。
- `public File(String parent, String child)` ：从**父路径名字符串和子路径名字符串**创建新的 File实例。
- `public File(File parent, String child)` ：从**父抽象路径名和子路径名字符串**创建新的 File实例。
- 构造举例，代码如下：

```java
package com.atguigu.file;

import java.io.File;

public class FileObjectTest {
    public static void main(String[] args) {
        // 文件路径名
        String pathname = "D:\\aaa.txt";
        File file1 = new File(pathname);

        // 文件路径名
        String pathname2 = "D:\\aaa\\bbb.txt";
        File file2 = new File(pathname2);

        // 通过父路径和子路径字符串
        String parent = "d:\\aaa";
        String child = "bbb.txt";
        File file3 = new File(parent, child);

        // 通过父级File对象和子路径字符串
        File parentDir = new File("d:\\aaa");
        String childFile = "bbb.txt";
        File file4 = new File(parentDir, childFile);
    }
}
```

> 小贴士：

1. 一个File对象代表硬盘或网络中可能存在的一个文件或者目录。
2. 无论该路径下是否存在文件或者目录，都不影响File对象的创建。
3. 如果File对象代表的文件或目录存在，则File对象实例初始化时，就会用硬盘中对应文件或目录的属性信息（例如，时间、类型等）为File对象的属性赋值，否则除了路径和名称，File对象的其他属性将会保留默认值。



### 3 常用方法

### 1、获取文件和目录基本信息的方法

- `public String getName()` ：返回由此File表示的文件或目录的名称。
- `public long length()` ：返回由此File表示的文件的长度。
- `public String getPath()` ：将此File转换为路径名字符串。
- `public long lastModified()`：返回File对象对应的文件或目录的最后修改时间（毫秒值）

### 2、各种路径问题

- `public String getPath()` ：将此File转换为路径名字符串。
- `public String getAbsolutePath()` ：返回此File的绝对路径名字符串。
- `String getCanonicalPath()`：返回此File对象所对应的规范路径名。

File类可以使用文件路径字符串来创建File实例，该文件路径字符串既可以是绝对路径，也可以是相对路径。

默认情况下，系统总是依据用户的工作路径来解释相对路径，这个路径由系统属性“user.dir”指定，通常也就是运行Java虚拟机时所作的路径。

- **绝对路径**：从盘符开始的路径，这是一个完整的路径。
- **相对路径**：相对于**项目目录**的路径，这是一个便捷的路径，开发中经常使用。
- **规范路径**：所谓规范路径名，即对路径中的“..”等进行解析后的路径名
- window的路径分隔符使用“\”，而Java程序中的“\”表示转义字符，所以在Windows中表示路径，需要用“\”。或者直接使用“/”也可以，Java程序支持将“/”当成平台无关的路径分隔符。或者直接使用File.separator常量值表示。
- 把构造File对象指定的文件或目录的路径名，称为构造路径，它可以是绝对路径，也可以是相对路径
- 当构造路径是绝对路径时，那么getPath和getAbsolutePath结果一样
- 当构造路径是相对路径时，那么getAbsolutePath的路径 = user.dir的路径 + 构造路径
- 当路径中不包含".."和"/开头"等形式的路径，那么规范路径和绝对路径一样，否则会将..等进行解析。路径中如果出现“..”表示上一级目录，路径名如果以“/”开头，表示从“根目录”下开始导航。

### 3、判断功能的方法

- `public boolean exists()` ：此File表示的文件或目录是否实际存在。
- `public boolean isDirectory()` ：此File表示的是否为目录。
- `public boolean isFile()` ：此File表示的是否为文件。

### 4、创建删除功能的方法

- `public boolean createNewFile()` ：当且仅当具有该名称的文件尚不存在时，创建一个新的空文件。
- `public boolean delete()` ：删除由此File表示的文件或目录。 只能删除空目录。
- `public boolean mkdir()` ：创建由此File表示的目录。
- `public boolean mkdirs()` ：创建由此File表示的目录，包括任何必需但不存在的父目录。

### 5、列出目录的下一级

- `public String[] list()` ：返回一个String数组，表示该File目录中的所有子文件或目录。
- `public File[] listFiles()` ：返回一个File数组，表示该File目录中的所有的子文件或目录。
- `public File[] listFiles(FileFilter filter)`：返回所有满足指定过滤器的文件和目录。如果给定 filter 为 null，则接受所有路径名。否则，当且仅当在路径名上调用过滤器的 FileFilter.accept(File pathname)方法返回 true 时，该路径名才满足过滤器。如果当前File对象不表示一个目录，或者发生 I/O 错误，则返回 null。
- `public String[] list(FilenameFilter filter)`：返回返回所有满足指定过滤器的文件和目录。如果给定 filter 为 null，则接受所有路径名。否则，当且仅当在路径名上调用过滤器的 FilenameFilter .accept(File dir, String name)方法返回 true 时，该路径名才满足过滤器。如果当前File对象不表示一个目录，或者发生 I/O 错误，则返回 null。
- `public File[] listFiles(FilenameFilter filter)`：返回返回所有满足指定过滤器的文件和目录。如果给定 filter 为 null，则接受所有路径名。否则，当且仅当在路径名上调用过滤器的 FilenameFilter .accept(File dir, String name)方法返回 true 时，该路径名才满足过滤器。如果当前File对象不表示一个目录，或者发生 I/O 错误，则返回 null。

### 4 IO的分类

根据数据的流向分为：**输入流**和**输出流**。

- **输入流** ：把数据从`其他设备`上读取到`内存`中的流。
- 以InputStream,Reader结尾
- **输出流** ：把数据从`内存` 中写出到`其他设备`上的流。
- 以OutputStream、Writer结尾

根据数据的类型分为：**字节流**和**字符流**。

- **字节流** ：以字节为单位，读写数据的流。
- 以InputStream和OutputStream结尾
- **字符流** ：以字符为单位，读写数据的流。
- 以Reader和Writer结尾

根据IO流的角色不同分为：**节点流**和**处理流**。

- **节点流**：可以从或向一个特定的地方（节点）读写数据。如FileReader.
- **处理流**：是对一个已存在的流进行连接和封装，通过所封装的流的功能调用实现数据读写。如BufferedReader.处理流的构造方法总是要带一个其他的流对象做参数。一个流对象经过其他流的多次包装，称为流的链接。

> 这种设计是装饰模式（Decorator Pattern）也称为包装模式（Wrapper Pattern），其使用一种对客户端透明的方式来动态地扩展对象的功能，它是通过继承扩展功能的替代方案之一。在现实生活中你也有很多装饰者的例子，例如：人需要各种各样的衣着，不管你穿着怎样，但是，对于你个人本质来说是不变的，充其量只是在外面加上了一些装饰，有，“遮羞的”、“保暖的”、“好看的”、“防雨的”....

**常用的节点流：**

- 文 件 FileInputStream FileOutputStrean FileReader FileWriter 文件进行处理的节点流。
- 字符串 StringReader StringWriter 对字符串进行处理的节点流。
- 数 组 ByteArrayInputStream ByteArrayOutputStream CharArrayReader CharArrayWriter 对数组进行处理的节点流（对应的不再是文件，而是内存中的一个数组）。
- 管 道 PipedInputStream、PipedOutputStream、PipedReader、PipedWriter对管道进行处理的节点流。

**常用处理流：**

- 缓冲流：BufferedInputStream、BufferedOutputStream、BufferedReader、BufferedWriter---增加缓冲功能，避免频繁读写硬盘。
- 转换流：InputStreamReader、OutputStreamReader---实现字节流和字符流之间的转换。
- 数据流：DataInputStream、DataOutputStream -提供读写Java基础数据类型功能
- 对象流：ObjectInputStream、ObjectOutputStream--提供直接读写Java对象功能

### 5 大顶级抽象父类们

|        | 输入流                | 输出流                 |
| ------ | --------------------- | ---------------------- |
| 字节流 | 字节输入流InputStream | 字节输出流OutputStream |
| 字符流 | 字符输入流Reader      | 字符输出流Writer       |

### 6 字节输出流【OutputStream】

`java.io.OutputStream`抽象类是表示字节输出流的所有类的超类，将指定的字节信息写出到目的地。它定义了字节输出流的基本共性功能方法。

- `public void write(int b)` ：将指定的字节输出流。虽然参数为int类型四个字节，但是只会保留一个字节的信息写出。
- `public void write(byte[] b)`：将 b.length字节从指定的字节数组写入此输出流。
- `public void write(byte[] b, int off, int len)` ：从指定的字节数组写入 len字节，从偏移量 off开始输出到此输出流。
- `public void flush()` ：刷新此输出流并强制任何缓冲的输出字节被写出。
- `public void close()` ：关闭此输出流并释放与此流相关联的任何系统资源。

> 小贴士：close方法，当完成流的操作时，必须调用此方法，释放系统资源。

### 7 FileOutputStream类

`OutputStream`有很多子类，我们从最简单的一个子类开始。`java.io.FileOutputStream`类是文件输出流，用于将数据写出到文件。

- `public FileOutputStream(File file)`：创建文件输出流以写入由指定的 File对象表示的文件。
- `public FileOutputStream(String name)`： 创建文件输出流以指定的名称写入文件。

当你创建一个流对象时，必须传入一个文件路径。如果该文件不存在，会创建该文件。如果有这个文件，会清空这个文件的数据。如果传入的是一个目录，则会报IOException异常。

### 8 字节输入流【InputStream】

`java.io.InputStream`抽象类是表示字节输入流的所有类的超类，可以读取字节信息到内存中。它定义了字节输入流的基本共性功能方法。

- `public int read()`： 从输入流读取一个字节。返回读取的字节值。虽然读取了一个字节，但是会自动提升为int类型。如果已经到达流末尾，没有数据可读，则返回-1。
- `public int read(byte[] b)`： 从输入流中读取一些字节数，并将它们存储到字节数组 b中 。每次最多读取b.length个字节。返回实际读取的字节个数。如果已经到达流末尾，没有数据可读，则返回-1。
- `public int read(byte[] b,int off,int len)`：从输入流中读取一些字节数，并将它们存储到字节数组 b中，从b[off]开始存储，每次最多读取len个字节 。返回实际读取的字节个数。如果已经到达流末尾，没有数据可读，则返回-1。
- `public void close()` ：关闭此输入流并释放与此流相关联的任何系统资源。

> 小贴士：close方法，当完成流的操作时，必须调用此方法，释放系统资源。

### 9 FileInputStream类

`java.io.FileInputStream`类是文件输入流，从文件中读取字节。

- `FileInputStream(File file)`： 通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的 File对象 file命名。
- `FileInputStream(String name)`： 通过打开与实际文件的连接来创建一个 FileInputStream ，该文件由文件系统中的路径名 name命名。

当你创建一个流对象时，必须传入一个文件路径。如果文件不存在，会抛出`FileNotFoundException` 。如果传入的是一个目录，则会报IOException异常。

### 10 复制文件

复制图片文件，代码使用演示：

```java
package com.atguigu.fileio;

import java.io.FileInputStream;
import java.io.FileOutputStream;
import java.io.IOException;

public class FileCopy {
    public static void main(String[] args) throws IOException {
        // 1.创建流对象
        // 1.1 指定数据源
        FileInputStream fis = new FileInputStream("D:\\test.jpg");
        // 1.2 指定目的地
        FileOutputStream fos = new FileOutputStream("test_copy.jpg");

        // 2.读写数据
        // 2.1 定义数组
        byte[] b = new byte[1024];
        // 2.2 定义长度
        int len;
        // 2.3 循环读取
        while ((len = fis.read(b))!=-1) {
            // 2.4 写出数据
            fos.write(b, 0 , len);
        }

        // 3.关闭资源
        fos.close();
        fis.close();
    }
}
```

### 11 字符输入流【Reader】

`java.io.Reader`抽象类是表示用于读取字符流的所有类的超类，可以读取字符信息到内存中。它定义了字符输入流的基本共性功能方法。

- `public int read()`： 从输入流读取一个字符。 虽然读取了一个字符，但是会自动提升为int类型。返回该字符的Unicode编码值。如果已经到达流末尾了，则返回-1。
- `public int read(char[] cbuf)`： 从输入流中读取一些字符，并将它们存储到字符数组 cbuf中 。每次最多读取cbuf.length个字符。返回实际读取的字符个数。如果已经到达流末尾，没有数据可读，则返回-1。
- `public int read(char[] cbuf,int off,int len)`：从输入流中读取一些字符，并将它们存储到字符数组 cbuf中，从cbuf[off]开始的位置存储。每次最多读取len个字符。返回实际读取的字符个数。如果已经到达流末尾，没有数据可读，则返回-1。
- `public void close()` ：关闭此流并释放与此流相关联的任何系统资源。

> 小贴士：close方法，当完成流的操作时，必须调用此方法，释放系统资源。

### 12 FileReader类

`java.io.FileReader`类是读取字符文件的便利类。构造时使用系统默认的字符编码和默认字节缓冲区。

- `FileReader(File file)`： 创建一个新的 FileReader ，给定要读取的File对象。
- `FileReader(String fileName)`： 创建一个新的 FileReader ，给定要读取的文件的名称。

当你创建一个流对象时，必须传入一个文件路径。类似于FileInputStream 。如果该文件不存在，则报FileNotFoundException。如果传入的是一个目录，则会报IOException异常。

### 13 字符输出流【Writer】

`java.io.Writer`抽象类是表示用于写出字符流的所有类的超类，将指定的字符信息写出到目的地。它定义了字节输出流的基本共性功能方法。

- `public void write(int c)` 写入单个字符。
- `public void write(char[] cbuf)`写入字符数组。
- `public void write(char[] cbuf, int off, int len)`写入字符数组的某一部分,off数组的开始索引,len写的字符个数。
- `public void write(String str)`写入字符串。
- `public void write(String str, int off, int len)` 写入字符串的某一部分,off字符串的开始索引,len写的字符个数。
- `public void flush()`刷新该流的缓冲。
- `public void close()` 关闭此流，但要先刷新它。

### 14 FileWriter类

`java.io.FileWriter`类是写出字符到文件的便利类。构造时使用系统默认的字符编码和默认字节缓冲区。

- `FileWriter(File file)`： 创建一个新的 FileWriter，给定要读取的File对象。
- `FileWriter(String fileName)`： 创建一个新的 FileWriter，给定要读取的文件的名称。

当你创建一个流对象时，必须传入一个文件路径，类似于FileOutputStream。如果文件不存在，则会自动创建。如果文件已经存在，则会清空文件内容，写入新的内容。

### 1、续写

- `public FileWriter(File file,boolean append)`： 创建文件输出流以写入由指定的 File对象表示的文件。
- `public FileWriter(String fileName,boolean append)`： 创建文件输出流以指定的名称写入文件。

这两个构造方法，参数中都需要传入一个boolean类型的值，`true` 表示追加数据，`false` 表示清空原有数据。这样创建的输出流对象，就可以指定是否追加续写了，代码使用演示：

操作类似于FileOutputStream。

```java
package com.atguigu.fileio;

import org.junit.Test;

import java.io.FileWriter;
import java.io.IOException;

public class FWWriteAppend {
    @Test
    public void test01()throws IOException {
        // 使用文件名称创建流对象，可以续写数据
        FileWriter fw = new FileWriter("fw.txt",true);
        // 写出字符串
        fw.write("尚硅谷真棒");
        // 关闭资源
        fw.close();
    }
}
```

### 2、换行

```java
package com.atguigu.fileio;

import java.io.FileWriter;
import java.io.IOException;

public class FWWriteNewLine {
    public static void main(String[] args) throws IOException {
        // 使用文件名称创建流对象，可以续写数据
        FileWriter fw = new FileWriter("fw.txt");
        // 写出字符串
        fw.write("尚");
        // 写出换行
        fw.write("\r\n");
        // 写出字符串
        fw.write("硅谷");
        // 关闭资源
        fw.close();
    }
}
```

### 3、关闭和刷新

【注意】FileWriter与FileOutputStream不同。因为内置缓冲区的原因，如果不关闭输出流，无法写出字符到文件中。但是关闭的流对象，是无法继续写出数据的。如果我们既想写出数据，又想继续使用流，就需要`flush` 方法了。

- `flush` ：刷新缓冲区，流对象可以继续使用。
- `close`:先刷新缓冲区，然后通知系统释放资源。流对象不可以再被使用了。

代码使用演示：

```java
package com.atguigu.fileio;

import java.io.FileWriter;
import java.io.IOException;

public class FWWriteFlush {
    public static void main(String[] args)throws IOException {
        // 使用文件名称创建流对象
        FileWriter fw = new FileWriter("fw.txt");
        // 写出数据，通过flush
        fw.write('刷'); // 写出第1个字符
        fw.flush();
        fw.write('新'); // 继续写出第2个字符，写出成功
        fw.flush();

        // 写出数据，通过close
        fw.write('关'); // 写出第1个字符
        fw.close();
        fw.write('闭'); // 继续写出第2个字符,【报错】java.io.IOException: Stream closed
        fw.close();
    }
}
```

> 小贴士：即便是flush方法写出了数据，操作的最后还是要调用close方法，释放系统资源。

### 15 缓冲流

缓冲流,也叫高效流，按照数据类型分类：

- **字节缓冲流**：`BufferedInputStream`，`BufferedOutputStream`
- **字符缓冲流**：`BufferedReader`，`BufferedWriter`

缓冲流的基本原理，是在创建流对象时，会创建一个内置的默认大小的缓冲区数组，通过缓冲区读写，减少系统IO次数，从而提高读写的效率。

### 1 构造方法

- `public BufferedInputStream(InputStream in)` ：创建一个 新的缓冲输入流。
- `public BufferedOutputStream(OutputStream out)`： 创建一个新的缓冲输出流。

构造举例，代码如下：

```java
// 创建字节缓冲输入流
BufferedInputStream bis = new BufferedInputStream(new FileInputStream("bis.txt"));
// 创建字节缓冲输出流
BufferedOutputStream bos = new BufferedOutputStream(new FileOutputStream("bos.txt"));
```

- `public BufferedReader(Reader in)` ：创建一个 新的缓冲输入流。
- `public BufferedWriter(Writer out)`： 创建一个新的缓冲输出流。

构造举例，代码如下：

```java
// 创建字符缓冲输入流
BufferedReader br = new BufferedReader(new FileReader("br.txt"));
// 创建字符缓冲输出流
BufferedWriter bw = new BufferedWriter(new FileWriter("bw.txt"));
```

### 16 InputStreamReader类

转换流`java.io.InputStreamReader`，是Reader的子类，是从字节流到字符流的桥梁。它读取字节，并使用指定的字符集将其解码为字符。它的字符集可以由名称指定，也可以接受平台的默认字符集。

- `InputStreamReader(InputStream in)`: 创建一个使用默认字符集的字符流。
- `InputStreamReader(InputStream in, String charsetName)`: 创建一个指定字符集的字符流。

构造举例，代码如下：

```java
InputStreamReader isr = new InputStreamReader(new FileInputStream("in.txt"));
InputStreamReader isr2 = new InputStreamReader(new FileInputStream("in.txt") , "GBK");
```

示例代码：

```java
package com.atguigu.transfer;

import java.io.FileInputStream;
import java.io.IOException;
import java.io.InputStreamReader;

public class InputStreamReaderDemo {
    public static void main(String[] args) throws IOException {
        // 定义文件路径,文件为gbk编码
        String fileName = "E:\\file_gbk.txt";
        // 创建流对象,默认UTF8编码
        InputStreamReader isr1 = new InputStreamReader(new FileInputStream(fileName));
        // 定义变量,保存字符
        int charData;
        // 使用默认编码字符流读取,乱码
        while ((charData = isr1.read()) != -1) {
            System.out.print((char)charData); // ��Һ�
        }
        isr1.close();

        // 创建流对象,指定GBK编码
        InputStreamReader isr2 = new InputStreamReader(new FileInputStream(fileName) , "GBK");
        // 使用指定编码字符流读取,正常解析
        while ((charData = isr2.read()) != -1) {
            System.out.print((char)charData);// 大家好
        }
        isr2.close();
    }
}
```

### 17 OutputStreamWriter类

转换流`java.io.OutputStreamWriter` ，是Writer的子类，是从字符流到字节流的桥梁。使用指定的字符集将字符编码为字节。它的字符集可以由名称指定，也可以接受平台的默认字符集。

- `OutputStreamWriter(OutputStream in)`: 创建一个使用默认字符集的字符流。
- `OutputStreamWriter(OutputStream in, String charsetName)`: 创建一个指定字符集的字符流。

```java
package com.atguigu.transfer;

import java.io.FileOutputStream;
import java.io.IOException;
import java.io.OutputStreamWriter;

public class OutputStreamWriterDemo {
    public static void main(String[] args) throws IOException {
        // 定义文件路径
        String FileName = "E:\\out_utf8.txt";
        // 创建流对象,默认UTF8编码
        OutputStreamWriter osw = new OutputStreamWriter(new FileOutputStream(FileName));
        // 写出数据
        osw.write("你好"); // 保存为6个字节
        osw.close();

        // 定义文件路径
        String FileName2 = "E:\\out_gbk.txt";
        // 创建流对象,指定GBK编码
        OutputStreamWriter osw2 = new OutputStreamWriter(new FileOutputStream(FileName2),"GBK");
        // 写出数据
        osw2.write("你好");// 保存为4个字节
        osw2.close();
    }
}
```

### 18 数据流与对象流

前面学习的IO流，在程序代码中，要么将数据直接按照字节处理，要么按照字符处理。那么，如果读写Java其他数据类型的数据，怎么办呢？

```java
String name = “巫师”;
int age = 300;
char gender = ‘男’;
int energy = 5000;
double price = 75.5;
boolean relive = true;

Student stu = new Student("张三",23,89);
```

Java提供了数据流和对象流来处理这些类型的数据：

- DataOutputStream：数据输出流允许应用程序以适当方式将基本 Java 数据类型写入输出流中。然后，应用程序可以使用数据输入流（DataInputStream）将数据读入。
- DataInputStream：数据输入流允许应用程序以与机器无关方式从底层输入流中读取基本 Java 数据类型。
- ObjectOutputStream：将 Java 基本数据类型和对象写入字节输出流中。稍后可以使用 ObjectInputStream 将数据读入。通过在流中使用文件可以实现Java各种基本数据类型的数据以及对象的持久存储。如果流是网络套接字流，则可以在另一台主机上或另一个进程中接收这些数据或重构对象。
- ObjectInputStream：ObjectInputStream 对以前使用 ObjectOutputStream 写入的基本数据和对象进行反序列化。

因为DataOutputStream和DataInputStream只支持Java基本数据类型和字符串的读写，而不支持Java对象的对象。而ObjectOutputStream和ObjectInputStream既支持Java基本数据类型的数据读写，又支持Java对象的读写，所以下面直接介绍对象流ObjectOutputStream和ObjectInputStream即可。

- `public ObjectOutputStream(OutputStream out)`： 创建一个指定OutputStream的ObjectOutputStream。
- `public ObjectInputStream(InputStream in)`： 创建一个指定InputStream的ObjectInputStream。

构造举例，代码如下：

```java
FileOutputStream fos = new FileOutputStream("game.dat");
ObjectOutputStream oos = new ObjectOutputStream(fos);
FileInputStream fis = new FileInputStream("game.dat");
ObjectInputStream ois = new ObjectInputStream(fis);
```

### 19 使用对象流读写各种类型的数据

ObjectOutpuStream也从OutputStream父类中继承基本方法：

- `public void write(int b)` ：将指定的字节输出流。虽然参数为int类型四个字节，但是只会保留一个字节的信息写出。
- `public void write(byte[] b)`：将 b.length字节从指定的字节数组写入此输出流。
- `public void write(byte[] b, int off, int len)` ：从指定的字节数组写入 len字节，从偏移量 off开始输出到此输出流。
- `public void flush()` ：刷新此输出流并强制任何缓冲的输出字节被写出。
- `public void close()` ：关闭此输出流并释放与此流相关联的任何系统资源。

还支持将各种Java数据类型的数据写入输出流中：

- public void writeBoolean(boolean val)：写入一个 boolean 值。
- public void writeByte(int val)：写入一个8位字节。
- public void writeShort(int val)：写入一个16位的 short 值。
- public void writeChar(int val)：写入一个16位的 char 值。
- public void writeInt(int val)：写入一个32位的 int 值。
- public void writeLong(long val)：写入一个64位的 long 值。
- public void writeFloat(float val)：写入一个32位的 float 值。
- public void writeDouble(double val)：写入一个64位的 double 值。
- public void writeUTF(String str)：将表示长度信息的两个字节写入输出流，后跟字符串 s 中每个字符的 UTF-8 修改版表示形式。如果 s 为 null，则抛出 NullPointerException。根据字符的值，将字符串 s 中每个字符转换成一个字节、两个字节或三个字节的字节组。注意，将 String 作为基本数据写入流中与将它作为 Object 写入流中明显不同。

ObjectInputStream除了从InputStream父类中继承基本方法之外，

- `public int read()`： 从输入流读取一个字节。返回读取的字节值。虽然读取了一个字节，但是会自动提升为int类型。如果已经到达流末尾，没有数据可读，则返回-1。
- `public int read(byte[] b)`： 从输入流中读取一些字节数，并将它们存储到字节数组 b中 。每次最多读取b.length个字节。返回实际读取的字节个数。如果已经到达流末尾，没有数据可读，则返回-1。
- `public int read(byte[] b,int off,int len)`：从输入流中读取一些字节数，并将它们存储到字节数组 b中，从b[off]开始存储，每次最多读取len个字节 。返回实际读取的字节个数。如果已经到达流末尾，没有数据可读，则返回-1。
- `public void close()` ：关闭此输入流并释放与此流相关联的任何系统资源。

还支持从输入流中读取各种Java数据类型的数据：

- public boolean readBoolean()：读取一个 boolean 值。
- public byte readByte()：读取一个 8 位的字节。
- public short readShort()：读取一个 16 位的 short 值。
- public char readChar()：读取一个 16 位的 char 值。
- public int readInt()：读取一个 32 位的 int 值。
- public long readLong()：读取一个 64 位的 long 值。
- public float readFloat()：读取一个 32 位的 float 值。
- public double readDouble()：读取一个 64 位的 double 值。
- public String readUTF()：读取 UTF-8 修改版格式的 String。

> 注意：读的顺序和方法与写的顺序和方法要一一对应。

示例代码：

```java
package com.atguigu.object;

import org.junit.Test;

import java.io.*;

public class ReadWriteDataOfAnyType {
    @Test
    public void save() throws IOException {
        String name = "巫师";
        int age = 300;
        char gender = '男';
        int energy = 5000;
        double price = 75.5;
        boolean relive = true;

        ObjectOutputStream oos = new ObjectOutputStream(new FileOutputStream("game.dat"));
        oos.writeUTF(name);
        oos.writeInt(age);
        oos.writeChar(gender);
        oos.writeInt(energy);
        oos.writeDouble(price);
        oos.writeBoolean(relive);
        oos.close();
    }
    @Test
    public void reload()throws IOException{
        ObjectInputStream ois = new ObjectInputStream(new FileInputStream("game.dat"));
        String name = ois.readUTF();
        int age = ois.readInt();
        char gender = ois.readChar();
        int energy = ois.readInt();
        double price = ois.readDouble();
        boolean relive = ois.readBoolean();

        System.out.println(name+"," + age + "," + gender + "," + energy + "," + price + "," + relive);

        ois.close();
    }
}
```

### 20 序列化与反序列化概念

Java 提供了一种对象**序列化**的机制。用一个字节序列可以表示一个对象，该字节序列包含该`对象的类型`和`对象中存储的属性`等信息。字节序列写出到文件之后，相当于文件中**持久保存**了一个对象的信息。

反之，该字节序列还可以从文件中读取回来，重构对象，对它进行**反序列化**。`对象的数据`、`对象的类型`和`对象中存储的数据`信息，都可以用来在内存中创建对象。看图理解序列化：

ObjectOutputStream流中支持序列化的方法是：

- `public final void writeObject (Object obj)` : 将指定的对象写出。

ObjectInputStream流中支持反序列化的方法是：

- `public final Object readObject ()` : 读取一个对象。

### 21 Serializable序列化接口与transient关键字

某个类的对象需要序列化输出时，该类必须实现`java.io.Serializable` 接口，`Serializable` 是一个标记接口，不实现此接口的类将不会使任何状态序列化或反序列化，会抛出`NotSerializableException` 。

- 如果对象的某个属性也是引用数据类型，那么如果该属性也要序列化的话，也要实现`Serializable` 接口
- 该类的所有属性必须是可序列化的。如果有一个属性不需要可序列化的，则该属性必须注明是瞬态的，使用`transient` 关键字修饰。
- 静态变量的值不会序列化。因为静态变量的值不属于某个对象。