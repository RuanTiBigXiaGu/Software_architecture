# Hadoop概述

标签（空格分隔）： hadoop

---
> Hadoop文件系统可以访问多个不同的具体的文件系统，如HDFS、KFS和S3文件系统。

![HDFS](http://h.hiphotos.bdimg.com/album/s%3D1100%3Bq%3D90/sign=83b10645f9edab64707249c1c70694b2/caef76094b36acaf0963ee727ed98d1000e99cb8.jpg)
不同的文件系统之间其实也是可以进行通信，Hadoop提供许多接口，它的文件系统一般采用URI方案，以选择正确的文件系统实例与沟通。虽然可以再不同的文件系统之间进行交互，但是一般并不那样做，如在进行MapReduce的时候，一般只采取一种文件系统如HDFS一边进行数据的局部优化。
> Hadoop抽象文件系统类似Linux中的VFS虚拟文件系统，它从不同的文件系统中抽取了共同的操作，这些操作是一般的文件系统都具有的操作，如打开文件，创建文件，删除文件，复制文件，获取文件的信息等。这些共同的基本操作组合在一起就形成了FileSystem抽象类和AbstractFileSystem抽象类。然后从基类派生，以实现各不同的具体的文件系统。
这两类层次结构十分相似，很多类的实现几乎完全一样。
除FilterFileSystem外，FileSystem的直接子类都是具体的文件系统。包括以下文件系统：

- KosmosFileSystem：cloudstore(其前身是Kosmos文件系统)是相似于HDFS或是Google的GFS的文件系统，用C++编写。
- HftpFileSystem：一个在HTTP上提供对HDFS只读访问的文件系统(虽然其名称为HFTP，但它与FTP无关)。通常与distcp结合使用，在运行不同版本HDFS的集群间复制数据。
- RawLocalFileSystem：代表**本地**文件系统。
- FTPFileSystem：由FTP服务器支持的文件系统。
- S3FileSystem：由AmazonS3支持的文件系统，以块格式存储文件(与HDFS很相似)来解决S3的5 GB文件大小限制。
- NativeS3FileSystem：由Amazon S3支持的文件系统。

官方文档对RawLocalFileSystem的解释是
> extends FileSystem
Implement the FileSystem API for the raw local filesystem.

根据文档可以判断，所谓的RawLocalFileSystem应该指的就是本地的原生文件系统

至于NativeS3FileSystem实现的是AmazonS3文件系统
> AmazonS3文件系统是是一种 Internet 上的存储服务。该服务是为降低开发人员进行网络规模级计算的难度而设计的。这种实现以原始的形式在S3上存储文件，以便它们可以通过其他的S3的工具进行工作。

> Hadoop的使用者可以分为两类，应用程序编写者和文件系统实现者。在Hadoop 0.21版本之前，FileSystem类作为一般（抽象）文件系统的基类，一方面为**应用程序编写者**提供了使用Hadoop文件系统的接口，另一方面，为**文件系统实现者**提供了实现一个文件系统的接口（如hdfs，本地文件系统，kfs等等）。但在Hadoop0.21版本中，出现了FileContext类和AbstractFileSystem类，通过这两个API，可以将原来集中于FileSystem一个类中的功能分开，让使用者更加方便的在应用程序中使用多个文件系统。FileContext这个API还没有在hadoop中被大量的使用，因为还没有被合并到mapreduce计算中，但是它包含了正常的FileSystem 接口没有的新功能，如支持hdfs层面的软链接等。FileContext类是用来取代FileSystem类，向应用程序编写者提供使用Hadoop文件系统的接口，而原来的FileSystem则仅由文件系统实现者使用。估计AbstractFileSystem类将来会取代FileSystem类。

总结一下，FileSystem是面向文件系统实现者，而FileContext类和AbstractFileSystem类面向文件系统实现者。

## 输入输出流
> 可将Java 库的IO 类分割为输入与输出两个部分，这一点在用Web 浏览器阅读联机Java类文档时便可知道。通过继承，从InputStream（输入流）衍生的所有类都拥有名为read()的基本方法，用于读取单个字节或者字节数组。类似地，从OutputStream衍生的所有类都拥有基本方法write()，用于写入单个字节或者字节数组。然而，我们通常不会用到这些方法；它们之所以存在，是因为更复杂的类可以利用它们，以便提供一个更有用的接口。因此，我们很少用单个类创建自己的系统对象。一般情况下，我们都是将多个对象重叠在一起，提供自己期望的功能。我们之所以感到Java 的流库（Stream Library）异常复杂，正是由于为了创建单独一个结果流，却需要创建多个对象的缘故。很有必要按照功能对类进行分类。库的设计者首先决定与输入有关的所有类都从InputStream 继承，而与输出有关的所有类都从OutputStream 继承。

Java 的 I/O 操作类在包java.io下，大概有将近80个类，但是这些类大概可以分成四组，分别是：
1. 基于字节操作的 I/O 接口：InputStream 和 OutputStream
2. 基于字符操作的 I/O 接口：Writer 和 Reader
3. 基于磁盘操作的 I/O 接口：File
4. 基于网络操作的 I/O 接口：Socket

InputStream 相关类层次结构
![InputStream](http://a.hiphotos.bdimg.com/album/s%3D1100%3Bq%3D90/sign=0653546baf6eddc422e7b0fa09eb8d8c/5ab5c9ea15ce36d3dd9ef9be38f33a87e850b1a1.jpg)
OutputStream 相关类层次结构
![OutputStream](http://g.hiphotos.bdimg.com/album/s%3D1100%3Bq%3D90/sign=7bb99da50cf41bd5de53ecf561eababa/359b033b5bb5c9ea1369796dd739b6003bf3b3a1.jpg)

### InputStream
> InputStream 的作用是标志那些从不同起源地产生输入的类。这些起源地包括（每个都有一个相关的InputStream 子类）：

1. 字节数组
2. String对象
3. 文件
4. “管道”，它的工作原理与现实生活中的管道类似：将一些东西置入一端，它们在另一端出来一系列其他流，以便我们将其统一收集到单独一个流内
5. 其他起源地，如Internet 连接等

---
- 除此以外，FilterInputStream 也属于InputStream 的一种类型，用它可为“破坏器”（**貌似就是析构函数**）类提供一个基础类，以便将属性或者有用的接口同输入流连接到一起。FilterInputStream 的作用是用来“封装其它的输入流，并为它们提供额外的功能”。它的常用的子类有BufferedInputStream和DataInputStream。
- ByteArrayInputStream：允许内存中的一个缓冲区作为InputStream，使用从中提取字节的缓冲区作为一个数据源使用。通过将其同一个FilterInputStream 对象连接，可提供一个有用的接口。
- StringBufferInputStream：将一个String转换成InputStream一个String（字串）。基础的实施方案实际采用一个StringBuffer（字串缓冲）作为一个数据源使用。通过将其同一个FilterInputStream对象连接，可提供一个有用的接口。
- FileInputStream：用于从文件读取信息代表文件名的一个String，或者一个File FileDescriptor对象作为一个数据源使用。通过将其同一个FilterInputStream 对象连接，可提供一个有用的接口。
- PipedInputStream：产生为相关的PipedOutputStream写的数据。实现了“管道化”的概念。（**管道流类PipedInputStream类和PipedOutputStream类用于在应用程序中创建管道通信。一个PipedInputStream实例对象必须和PipedOutputStream实例对象进行连接而产生一个通信管道，PipedOutputsStream可以向管道中写入数据，PipedInputStream可以从管道中读取PipedOutputStream写入的数据，这两个类主要用来完成线程之间的通信，一个线程的PipedInputStream对象能够从另外一个线程的PipedOutputStream对象中读取数据。**）
- SequenceInputStream：将两个或更多的InputStream对象转换成单个InputStream 使用两个InputStream对象或者一个Enumeration，用于InputStream 对象的一个容器作为一个数据源使用。通过将其同一个FilterInputStream 对象连接，可提供一个有用的接口FilterInputStream对作为破坏器接口使用的类进行抽象；那个破坏器为其他InputStream类提供了有用的功能。（**有些情况下，当我们需要从多个输入流中向程序读入数据。此时，可以使用合并流，将多个输入流合并成一个SequenceInputStream流对象。SequenceInputStream会将与之相连接的流集组合成一个输入流并从第一个输入流开始读取，直到到达文件末尾，接着从第二个输入流读取，依次类推，直到到达包含的后一个输入流的文件末尾为止。合并流的作用是将多个源合并合一个源。**）

---
### OutputStream
> 这一类别包括的类决定了我们的输入往何处去：一个字节数组（但没有String；假定我们可用字节数组创建一个）；一个文件；或者一个“管道”。除此以外，FilterOutputStream为“破坏器”类提供了一个基础类，它将属性或者有用的接口同输出流连接起来

- ByteArrayOutputStream：在内存中创建一个缓冲区。我们发送给流的所有数据都会置入这个缓冲区。可选缓冲区的初始大小用于指出数据的目的地。若将其同FilterOutputStream 对象连接到一起，可提供一个有用的接口。
- FileOutputStream：将信息发给一个文件，用一个String代表文件名，或选用一个File 或FileDescriptor对象用于指出数据的目的地。若将其同FilterOutputStream对象连接到一起，可提供一个有用的接口。
- PipedOutputStream：我们写给它的任何信息都会自动成为相关的PipedInputStream的输出。实现了“管道化”的概念。PipedInputStream为多线程处理指出自己数据的目的地，将其同FilterOutputStream对象连接到一起，便可提供一个有用的接口
- FilterOutputStream对作为破坏器接口使用的类进行抽象处理；那个破坏器为其OutputStream 类提供了有用的功能

###DataInputStream
> DataInputStream从FilterInputStream派生，在FilterInputStream中InputStream in的属性。
####文档的解释是
**DataInputStream is a kind of InputStream to read data directly as primitive data types.**

> 数据输入流允许应用程序以与机器无关方式从底层输入流中读取基本Java 数据类型。应用程序可以使用数据输出流写入稍后由数据输入流读取的数据。DataInputStream对于多线程访问不一定是安全的。线程安全是可选的，它由此类方法的使用者负责。

---
###DataOutputStream
> DataOutputStream数据输出流允许应用程序以适当方式将基本Java数据类型写入输出流中。然后，应用程序可以使用数据输入流将数据读入。

---
##Hadoop的输入输出流
> 在Hadoop中，FSInputStream(**FSInputStream is a generic old InputStream with a little bit of RAF-style seek ability.**)、FSDataInputStream(**Utility that wraps a FSInputStream in a DataInputStream and buffers input through a BufferedInputStream.**)和FSDataOutputStream(**Utility that wraps a OutputStream in a DataOutputStream.**)的作用与InputStream、DataInputStream和DataOutputStream在Java IO中的作用类似。用FileSystem的create方法创建一个输出流时的返回值类型为FSDataOutputStream，而open方法则返回一个FSDataInputStream实例。Hadoop中并没有FSOutputStream，当有两个从OutputStream派生的类：KFSOutputStream和S3OutputStream。这两个类也就相当于OutputStream。很多文件系统都会从FSInputStream派生，实现自己特定的输入输出流，如S3InputStream等等。
