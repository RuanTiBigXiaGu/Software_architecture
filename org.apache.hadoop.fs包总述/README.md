##将类图或者代码分析传送至# Hadoop概述


---

- Mapreduce是一种模式。
- Hadoop是一种框架。
- Hadoop是一个实现了mapreduce模式的开源的分布式并行编程框架。

---

### Mapreduce模式
MapReduce是一种计算模型，简单的说就是将大批量的工作（数据）分解（MAP）执行，然后再将结果合并成最终结果（REDUCE）。这样做的好处是可以在任务被分解后，可以通过大量机器进行并行计算，减少整个操作的时间。但如果你要我再通俗点介绍，那么，说白了，Mapreduce的原理就是一个归并排序。
![MapReduce](https://github.com/julycoding/The-Art-Of-Programming-By-July/raw/master/ebook/images/8/8.2/8.2.1.gif)
MapReduce致力于解决大规模数据处理的问题，因此在设计之初就考虑了数据的局部性原理，利用局部性原理将整个问题分而治之。MapReduce集群由普通PC机构成，为无共享式架构。在处理之前，将数据集分布至各个节点。处理时，每个节点就近读取本地存储的数据处理（map），将处理后的数据进行合并（combine）、排序（shuffle and sort）后再分发（至reduce节点），避免了大量数据的传输，提高了处理效率。无共享式架构的另一个好处是配合复制（replication）策略，集群可以具有良好的容错性，一部分节点的down机对集群的正常工作不会造成影响。
![Sort](https://github.com/julycoding/The-Art-Of-Programming-By-July/raw/master/ebook/images/8/8.2/8.2.3.gif)

###Hadoop框架
Hadoop 是一个实现了MapReduce 计算模型的开源分布式并行编程框架，程序员可以借助Hadoop编写程序，将所编写的程序运行于计算机机群上，从而实现对海量数据的处理。
此外，Hadoop 还提供一个分布式文件系统(HDFS）及分布式数据库（HBase）用来将数据存储或部署到各个计算节点上。所以，你可以大致认为：Hadoop=HDFS（文件系统，数据存储技术相关）+HBase（数据库）+MapReduce（数据处理）。Hadoop 框架如图2 所示：
![hadoop](https://github.com/julycoding/The-Art-Of-Programming-By-July/raw/master/ebook/images/8/8.2/8.2.4.gif)
借助Hadoop 框架及云计算核心技术MapReduce来实现数据的计算和存储，并且将HDFS 分布式文件系统和HBase分布式数据库很好的融入到云计算框架中，从而实现云计算的分布式、并行计算和存储，并且得以实现很好的处理大规模数据的能力。

---

###Hadoop的组成部分
Hadoop是Google的MapReduce一个Java实现。MapReduce是一种简化的分布式编程模式，让程序自动分布到一个由普通机器组成的超大集群上并发执行。Hadoop主要由HDFS、MapReduce和HBase等组成。具体的hadoop的组成如下图：
![chengfen](https://github.com/julycoding/The-Art-Of-Programming-By-July/raw/master/ebook/images/8/8.2/8.2.5.gif)

---

###Hadoop HDFS(源码分析)
Hadoop HDFS是Google GFS存储系统的开源实现，主要应用场景是作为并行计算环境（MapReduce）的基础组件，同时也是BigTable（如HBase、HyperTable）的底层分布式文件系统。HDFS采用master/slave架构。一个HDFS集群是有由一个Namenode和一定数目的Datanode组成。Namenode是一个中心服务器，负责管理文件系统的namespace和客户端对文件的访问。Datanode在集群中一般是一个节点一个，负责管理节点上它们附带的存储。在内部，一个文件其实分成一个或多个block，这些block存储在Datanode集合里。如下图所示（HDFS体系结构图）：
![HDFS](https://github.com/julycoding/The-Art-Of-Programming-By-July/raw/master/ebook/images/8/8.2/8.2.6.gif)

---

##org.apache.hadoop.fs
> An abstract file system API

org.apache.hadoop.fs包提供了一个抽象文件系统的API。该包下有50多个类，有7个包。
![org](http://d.hiphotos.bdimg.com/album/s%3D1100%3Bq%3D90/sign=2f31d02aa6c27d1ea1263fc52be5961f/242dd42a2834349bf8350c0bcbea15ce36d3be09.jpg)
org.apache.hadoop.fs包中的FileSystem抽象类和AbstractFileSystem抽象类作为抽象文件系统的基类，提供了基本的抽象操作。其中FileSystem类是0.21版本之前唯一的基类，但在0.21版本中，出现了AbstractFileSystem，该类似乎来取代FileSystem类原来的部分功能。在这两个基类的基础上形成了两个类继承的层次结构。
org.apache.hadoop.fs子包ftp、kfs、local、s3和s3native都是实现的具体的文件系统。Permission文件夹实现了有关文件访问许可的功能。Shell文件夹实现了对shell命令的调用。
