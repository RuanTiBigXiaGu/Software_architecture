#简单说明

### Hadoop任务分配

#### 整个系统的结构层次

****

1. org.apache.hadoop.fs包总述
2. Hadoop文件系统概述(类层次结构, 输入输出流 )
3. FileSystem深入分析(fs中的接口, FileSystem, FilterFileSystem, ChecksumFileSystem, LocalFileSystem)
4. 输入输出流分析(FSInputStream抽象类, 输出流, FSInputChecker, FSOutputSummer, FSDataInputStream, FSDataOutputStream)
5. AbstractFileSystem分析(AbstractFileSystem抽象类, FilterFs抽象类, ChecksumFs抽象类, LocalFs类, DelegateToFileSystem抽象类, FileContext类)
6. 其它类(Hadoop Shell命令, 杂类)

#### 目前的新的任务分配

- org.apache.hadoop.fs包总述     **我**
- Hadoop文件系统概述             **我**
- FileSystem深入分析             **刘培新 陈广欣**
- 输入输出流分析                 **李冬冬**
- AbstractFileSystem分析         **郭庆 任雅丽**
- 其它类                         **黄逸斌 陈鹏**（同时最后画出UML）

**该划分是按照随代码给的PDF得出的，所以可以根据PDF的内容进行分析，具体每个部分需要看哪些文件就自己去找了**


****

第一阶段的时间，完成初期的代码分析阶段，不要求特备细致，但至少需要看完PDF上的内容的量
注意，PDF只是**前期分析报告**，不能直接照搬，可以再阅读PDF的同时需要查阅源代码的详细资料
可以在[HDFS官方文档] (http://hadoop.apache.org/docs/r2.2.0/api/org/apache/hadoop/fs/package-summary.html) 上查询，或者直接[Google] (https://www.google.co.uk)

# **5月17日晚（周六）** 


### 第二阶段的内容

1. 理解设计思想，并给出软件架构图。
2. 在软件架构图的基础上，给出源代码的详细类图和核心过程的顺序图，说明源代码的设计如何与设计思想呼应。
3. 在明确详细设计的基础上，以源代码中的相关代码为例证明自己的判断。
 
第二阶段的时间截止到
# **5月20日（下周二）**

### 第三阶段的内容
通过阅读源代码，深刻体会 Hadoop 云计算平台与文件系统之间的关系，修正提供的分析报告，给出完整的分析过程与修订后的报告。
时间为
## **1天**

# **请务必准时完成！！！！！！！**
