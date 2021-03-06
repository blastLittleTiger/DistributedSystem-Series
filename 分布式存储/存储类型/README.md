# 分布式存储

分布式存储系统，广义上来讲，将文件存储抽象化，之前提到的块存储和对象存储都建立在这个系统之上。从某些角度来讲，存储系统相当于中间件，建立在底层的 SATA 或者 SSD 磁盘之上，而服务于上层的块存储。

存储的方式大致可以分为三类，对象存储（Object Storage）、块存储（Block Storage）和文件存储（File Storage）。

![](https://i.postimg.cc/ZRCQqF7b/image.png)

# 存储类型

不同的存储类型有各自的特点和优点，在选择的时候，我们需要根据具体情况来决定最合适的存储类型。

| 比较项目             | 块存储                           | 对象存储                 | 文件存储       |
| :------------------- | :------------------------------- | :----------------------- | :------------- |
| 基础单位             | fixed-size block                 | object                   | file           |
| 支持 in-place update | 支持                             | 否                       | 支持           |
| protocol             | SCSI, Fibre Channel, SATA        | REST and SOAP over HTTP  | CIFS and NFS   |
| 性能                 | 高                               | 中等                     | 高             |
| 元数据               | 少量                             | 大量                     | 中等           |
| 适用场景             | 交易型数据（经常需要变动的数据） | 相对静止（static）的数据 | 共享的文件     |
| 最大优点             | 高性能                           | 可扩展性强、分布式使用   | 相对简化地管理 |

![](https://i.postimg.cc/HjDzqrVt/image.png)

块存储在低延时高 IOPS 上有明显优势，特别适合数据库等高性能 IO 场景；对象存储在海量和吞吐上有明显优势，特别适合互联网图片、音视频处理场景；文件存储在吞吐和共享上有明显优势，特别适合高性能计算、以及数据共享场景。

## 块存储

块存储，简单来说就是使用块设备为系统提供存储服务。块存储分多种类型，有单机块存储，网络存储（如 NAS，SAN 等），分布式块存储（目前主流的如 AWS 的 EBS，青云的云硬盘，阿里云的云磁盘，网易云硬盘等）。通常块存储的表现形式就是一块设备，用户看到的就是类似于 sda，sdb 这样的逻辑设备。

块存储，不论具体存储内容的大小，都会将它存储到一个固定大小的容器中。块存储的主要特点是可以抽象成块设备，也就是大家常见的云盘。可以理解块存储为将个人用品收纳在一个固定大小的盒子内。如果我们需要寻找某件衣服，我们需要将整个盒子打开，然后一件一件地寻找。这里的盒子就相当于一个 block，这个盒子内部的空间是连续的，而且怎样摆放衣物也是不固定的（相反地，如果使用抽屉，我们就需要按照抽屉的内部构造来摆放衣物，因为抽屉会有棱棱角角，也可能会有内部的多层结构）

Block 就是一段连续的、没有硬性结构（unstructured）的比特（bytes）。通常情况下，考虑到数据存储的安全性和抗灾性，存储内容通常会被拆分成多个部分然后存储到不同的 Block 中，这样的设计同时也方便了更新过程。用户只需要去访问存储在某个 Block 中的数据信息，而非整个存储内容。

## 文件存储

文件存储，一般基于操作系统之上，其基本单位变成单个文件。我们最熟悉的个人电脑就将单个文件进行存储。在查找的时候，我们会通过路径来查找某个文件。通常情况下，对象会包含多个文件，我们可以理解为一个对象由多个文件组成，其中每个文件代表了一个小的部件，存储内容的时候我们会将这几个文件打包一并存储。

## 对象存储

对象存储，可以理解为将整个物体或者存储对象存储起来。一般来说，对象存储会同时存储大量元数据（meta data）以方便查找。通常来说，我们会把不同的衣物放进不同的抽屉或者同一抽屉的不同结构中，然后给它们贴上不同的标签。等到需要查找某个物件比如领带的时候，我们可以直接找到某一抽屉的固定地方，这样的设计简化了我们的查找过程。对象存储适合存储大量的小文件。
