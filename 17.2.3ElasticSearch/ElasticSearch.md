﻿﻿# Elastic Stack 简介

如果你没有听说过 Elastic Stack，那你一定听说过 ELK，实际上 ELK 是三款软件的简称，分别是 Elasticsearch、
Logstash、Kibana 组成，在发展的过程中，又有新成员 Beats 的加入，所以就形成了 Elastic Stack。所以说，ELK 是旧的称呼，Elastic Stack 是新的名字。

![image-20200922092403279](media/image-20200922092403279.png)

全系的 Elastic Stack 技术栈包括：

![image-20200922092505011](media/image-20200922092505011.png)

### Elasticsearch

Elasticsearch 基于 java，是个开源分布式搜索引擎，它的特点有：分布式，零配置，自动发现，索引自动分片，索引副本机制，restful 风格接口，多数据源，自动搜索负载等。

### Logstash

Logstash 基于 java，是一个开源的用于收集,分析和存储日志的工具。

### Kibana

Kibana 基于 nodejs，也是一个开源和免费的工具，Kibana 可以为 Logstash 和 ElasticSearch 提供的日志分析友好的 Web 界面，可以汇总、分析和搜索重要数据日志。

### Beats

Beats 是 elastic 公司开源的一款采集系统监控数据的代理 agent，是在被监控服务器上以客户端形式运行的数据收集器的统称，可以直接把数据发送给 Elasticsearch 或者通过 Logstash 发送给 Elasticsearch，然后进行后续的数据分析活动。Beats 由如下组成:

- Packetbeat：是一个网络数据包分析器，用于监控、收集网络流量信息，Packetbeat 嗅探服务器之间的流量，解析应用层协议，并关联到消息的处理，其支 持 ICMP (v4 and v6)、DNS、HTTP、Mysql、PostgreSQL、Redis、MongoDB、Memcache 等协议；
- Filebeat：用于监控、收集服务器日志文件，其已取代 logstash forwarder；
- Metricbeat：可定期获取外部系统的监控指标信息，其可以监控、收集 Apache、HAProxy、MongoDB
  MySQL、Nginx、PostgreSQL、Redis、System、Zookeeper 等服务；

> Beats 和 Logstash 其实都可以进行数据的采集，但是目前主流的是使用 Beats 进行数据采集，然后使用 Logstash 进行数据的分割处理等，早期没有 Beats 的时候，使用的就是 Logstash 进行数据的采集。

ES7 需要 Java11

# ElasticSearch

## 简介

官网：https://www.elastic.co/

ElasticSearch 是一个基于 Lucene 的搜索服务器。它提供了一个分布式多用户能力的全文搜索引擎，基于 RESTful web 接口。Elasticsearch 是用 Java 开发的，并作为 Apache 许可条款下的开放源码发布，是当前流行的企业级搜索引擎。设计用于云计算中，能够达到实时搜索，稳定，可靠，快速，安装使用方便。

我们建立一个网站或应用程序，并要添加搜索功能，但是想要完成搜索工作的创建是非常困难的。我们希望搜索解决方案要运行速度快，我们希望能有一个零配置和一个完全免费的搜索模式，我们希望能够简单地使用 JSON 通过 HTTP 来索引数据，我们希望我们的搜索服务器始终可用，我们希望能够从一台开始并扩展到数百台，我们要实时搜索，我们要简单的多租户，我们希望建立一个云的解决方案。因此我们利用 Elasticsearch 来解决所有这些问题及可能出现的更多其它问题。

ElasticSearch 是 Elastic Stack 的核心，同时 Elasticsearch 是一个分布式、RESTful 风格的搜索和数据分析引擎，能够解决不断涌现出的各种用例。作为 Elastic Stack 的核心，它集中存储您的数据，帮助您发现意料之中以及意料之外的情况。

应用场景：

程序——ES（Lucene/Solr——索引库

程序——Mybatis——数据库

与 Solr 对比：

Solr 是 ZK 进行分布式管理，Solr 支持多格式的数据，JSON，xml，CVS，而 ES 只致辞 JSON，ES 太吃内存。

ES 与关系型数据库

数据库 DataBase——表 TABLE——ROW 行——列

索引库 Index ——类型 TYPE——文档 DOCUMENT——字段 Field

分词建立索引，底层使用倒排索引

特点：分布式，高可用，多类型，多API，面向文档；

近实时，集群，

### 索引

- 索引（index）是 Elasticsearch 对逻辑数据的逻辑存储，所以它可以分为更小的部分。
- 可以把索引看成关系型数据库的表，索引的结构是为快速有效的全文索引准备的，特别是它不存储原始值。
- Elasticsearch 可以把索引存放在一台机器或者分散在多台服务器上，每个索引有一或多个分片（shard），每个分片可以有多个副本（replica）。

### 文档

- 存储在 Elasticsearch 中的主要实体叫文档（document）。用关系型数据库来类比的话，一个文档相当于数据库表中的一行记录。
- Elasticsearch 和 MongoDB 中的文档类似，都可以有不同的结构，但 Elasticsearch 的文档中，相同字段必须有相同类型。
- 文档由多个字段组成，每个字段可能多次出现在一个文档里，这样的字段叫多值字段（multivalued）。
  每个字段的类型，可以是文本、数值、日期等。字段类型也可以是复杂类型，一个字段包含其他子文档或者数
  组。

### 映射

所有文档写进索引之前都会先进行分析，如何将输入的文本分割为词条、哪些词条又会被过滤，这种行为叫做
映射（mapping）。一般由用户自己定义规则。

### 文档类型

- 在 Elasticsearch 中，一个索引对象可以存储很多不同用途的对象。例如，一个博客应用程序可以保存文章和评
  论。
- 每个文档可以有不同的结构。
- 不同的文档类型不能为相同的属性设置不同的类型。例如，在同一索引中的所有文档类型中，一个叫 title 的字段必须具有相同的类型。

## 安装

下载

到官网下载：https://www.elastic.co/cn/downloads/

Window 安装：

解压：D:\ES\elasticsearch-7.11.1\

运行：D:\ES\elasticsearch-7.11.1\bin\elasticsearch.bat

访问：http://localhost:9200/

Linux 安装：

选择对应版本的数据，这里我使用的是 Linux 来进行安装，所以就先下载好 ElasticSearch 的 Linux 安装包

拉取 Docker 容器

因为我们需要部署在 Linux 下，为了以后迁移 ElasticStack 环境方便，我们就使用 Docker 来进行部署，首先我们拉取一个带有 ssh 的 centos docker 镜像

```bash
# 拉取镜像
docker pull moxi/centos_ssh
# 制作容器
docker run --privileged -d -it -h ElasticStack --name ElasticStack -p 11122:22 -p 9200:9200 -p 5601:5601 -p 9300:9300 -v /etc/localtime:/etc/localtime:ro  moxi/centos_ssh /usr/sbin/init
```

然后直接远程连接 11122 端口即可

单机版安装

因为 ElasticSearch 不支持 Root 用户直接操作，因此我们需要创建一个 elsearch 用户

```bash
# 添加新用户
useradd elsearch

# 创建一个soft目录，存放下载的软件
mkdir /soft

# 进入，然后通过xftp工具，将刚刚下载的文件拖动到该目录下
cd /soft

# 解压缩
tar -zxvf elasticsearch-7.9.1-linux-x86_64.tar.gz

#重命名
mv elasticsearch-7.9.1/ elsearch
```

因为刚刚我们是使用 root 用户操作的，所以我们还需要更改一下/soft 文件夹的所属，改为 elsearch 用户

```bash
chown elsearch:elsearch /soft/ -R
```

然后在切换成 elsearch 用户进行操作

```bash
# 切换用户
su - elsearch
```

然后我们就可以对我们的配置文件进行修改了

```bash
# 进入到 elsearch下的config目录
cd /soft/elsearch/config
```

然后找到下面的配置

```yaml
#打开配置文件
vim elasticsearch.yml

#设置ip地址，任意网络均可访问
network.host: 0.0.0.0
```

在 Elasticsearch 中如果，network.host 不是 localhost 或者 127.0.0.1 的话，就会认为是生产环境，会对环境的要求比较高，我们的测试环境不一定能够满足，一般情况下需要修改 2 处配置，如下：

```bash
# 修改jvm启动参数
vim conf/jvm.options

#根据自己机器情况修改
-Xms128m
-Xmx128m
```

然后在修改第二处的配置，这个配置要求我们到宿主机器上来进行配置

```bash
# 到宿主机上打开文件
vim /etc/sysctl.conf
# 增加这样一条配置，一个进程在VMAs(虚拟内存区域)创建内存映射最大数量
vm.max_map_count=655360
# 让配置生效
sysctl -p
```

启动 ElasticSearch

首先我们需要切换到 elsearch 用户，root 用户无法启动 ES，启动就报错，必须新建用户

```bash
su - elsearch
```

然后在到 bin 目录下，执行下面

```bash
# 进入bin目录
cd /soft/elsearch/bin
# 后台启动
./elasticsearch -d
```

启动成功后，访问下面的 URL

```bash
http://202.193.56.222:9200/
```

如果出现了下面的信息，就表示已经成功启动了

![image-20200922150758205](media/image-20200922150758205.png)

如果你在启动的时候，遇到过问题，那么请参考下面的错误分析~

错误分析

错误情况 1

如果出现下面的错误信息

```
java.lang.RuntimeException: can not run elasticsearch as root
	at org.elasticsearch.bootstrap.Bootstrap.initializeNatives(Bootstrap.java:111)
	at org.elasticsearch.bootstrap.Bootstrap.setup(Bootstrap.java:178)
	at org.elasticsearch.bootstrap.Bootstrap.init(Bootstrap.java:393)
	at org.elasticsearch.bootstrap.Elasticsearch.init(Elasticsearch.java:170)
	at org.elasticsearch.bootstrap.Elasticsearch.execute(Elasticsearch.java:161)
	at org.elasticsearch.cli.EnvironmentAwareCommand.execute(EnvironmentAwareCommand.java:86)
	at org.elasticsearch.cli.Command.mainWithoutErrorHandling(Command.java:127)
	at org.elasticsearch.cli.Command.main(Command.java:90)
	at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:126)
	at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:92)
For complete error details, refer to the log at /soft/elsearch/logs/elasticsearch.log
[root@e588039bc613 bin]# 2020-09-22 02:59:39,537121 UTC [536] ERROR CLogger.cc@310 Cannot log to named pipe /tmp/elasticsearch-5834501324803693929/controller_log_381 as it could not be opened for writing
2020-09-22 02:59:39,537263 UTC [536] INFO  Main.cc@103 Parent process died - ML controller exiting
```

就说明你没有切换成 elsearch 用户，因为不能使用 root 操作 es

```bash
su - elsearch
```

错误情况 2

```bash
[1]:max file descriptors [4096] for elasticsearch process is too low, increase to at least[65536]
```

解决方法：切换到 root 用户，编辑 limits.conf 添加如下内容

```bash
vi /etc/security/limits.conf

# ElasticSearch添加如下内容:
* soft nofile 65536
* hard nofile 131072
* soft nproc 2048
* hard nproc 4096
```

错误情况 3

```bash
[2]: max number of threads [1024] for user [elsearch] is too low, increase to at least
[4096]
```

也就是最大线程数设置的太低了，需要改成 4096

```bash
#解决：切换到root用户，进入limits.d目录下修改配置文件。
vi /etc/security/limits.d/90-nproc.conf
#修改如下内容：
* soft nproc 1024
#修改为
* soft nproc 4096
```

错误情况 4

```bash
[3]: system call filters failed to install; check the logs and fix your configuration
or disable system call filters at your own risk
```

解决：Centos6 不支持 SecComp，而 ES5.2.0 默认 bootstrap.system_call_filter 为 true

```bash
vim config/elasticsearch.yml
# 添加
bootstrap.system_call_filter: false
bootstrap.memory_lock: false
```

错误情况 5

```bash
[elsearch@e588039bc613 bin]$ Exception in thread "main" org.elasticsearch.bootstrap.BootstrapException: java.nio.file.AccessDeniedException: /soft/elsearch/config/elasticsearch.keystore
Likely root cause: java.nio.file.AccessDeniedException: /soft/elsearch/config/elasticsearch.keystore
	at java.base/sun.nio.fs.UnixException.translateToIOException(UnixException.java:90)
	at java.base/sun.nio.fs.UnixException.rethrowAsIOException(UnixException.java:111)
	at java.base/sun.nio.fs.UnixException.rethrowAsIOException(UnixException.java:116)
	at java.base/sun.nio.fs.UnixFileSystemProvider.newByteChannel(UnixFileSystemProvider.java:219)
	at java.base/java.nio.file.Files.newByteChannel(Files.java:375)
	at java.base/java.nio.file.Files.newByteChannel(Files.java:426)
	at org.apache.lucene.store.SimpleFSDirectory.openInput(SimpleFSDirectory.java:79)
	at org.elasticsearch.common.settings.KeyStoreWrapper.load(KeyStoreWrapper.java:220)
	at org.elasticsearch.bootstrap.Bootstrap.loadSecureSettings(Bootstrap.java:240)
	at org.elasticsearch.bootstrap.Bootstrap.init(Bootstrap.java:349)
	at org.elasticsearch.bootstrap.Elasticsearch.init(Elasticsearch.java:170)
	at org.elasticsearch.bootstrap.Elasticsearch.execute(Elasticsearch.java:161)
	at org.elasticsearch.cli.EnvironmentAwareCommand.execute(EnvironmentAwareCommand.java:86)
	at org.elasticsearch.cli.Command.mainWithoutErrorHandling(Command.java:127)
	at org.elasticsearch.cli.Command.main(Command.java:90)
	at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:126)
	at org.elasticsearch.bootstrap.Elasticsearch.main(Elasticsearch.java:92)

```

我们通过排查，发现是因为 /soft/elsearch/config/elasticsearch.keystore 存在问题

![image-20200922111823740](media/image-20200922111823740.png)

也就是说该文件还是所属于 root 用户，而我们使用 elsearch 用户无法操作，所以需要把它变成 elsearch

```bash
chown elsearch:elsearch elasticsearch.keystore
```

错误情况 6

```bash
[1]: the default discovery settings are unsuitable for production use; at least one of [discovery.seed_hosts, discovery.seed_providers, cluster.initial_master_nodes] must be configured
ERROR: Elasticsearch did not exit normally - check the logs at /soft/elsearch/logs/elasticsearch.log
```

继续修改配置 elasticsearch.yaml

```bash
# 取消注释，并保留一个节点
node.name: node-1
cluster.initial_master_nodes: ["node-1"]
```

## 集成

添加依赖：



修改配置

Application.properties

```properties
# ES 服务地址
spring.data.elasticsearch.cluster-nodes=localhost:9300
# 设置连接超时时间
spring.data.elasticsearch.properties.transport.tcp.connect_time=120s
```





## Head 可视化工具

由于 ES 官方没有给 ES 提供可视化管理工具，仅仅是提供了后台的服务，elasticsearch-head 是一个为 ES 开发的一个页面客户端工具，其源码托管于 Github，地址为 [传送门](https://github.com/mobz/elasticsearch-head)

head 提供了以下安装方式

- 源码安装，通过 npm run start 启动（不推荐）
- 通过 docker 安装（推荐）
- 通过 chrome 插件安装（推荐）
- 通过 ES 的 plugin 方式安装（不推荐）

通过 Docker 方式安装

```bash
#拉取镜像
docker pull mobz/elasticsearch-head:5
#创建容器
docker create --name elasticsearch-head -p 9100:9100 mobz/elasticsearch-head:5
#启动容器
docker start elasticsearch-head
```

通过浏览器进行访问：

![image-20200922151529269](media/image-20200922151529269.png)

注意：由于前后端分离开发，所以会存在跨域问题，需要在服务端做 CORS 的配置，如下：

```bash
vim elasticsearch.yml

http.cors.enabled: true http.cors.allow-origin: "*"
```

通过 chrome 插件的方式安装不存在该问题

通过 Chrome 插件安装

打开 chrome 的应用商店，即可安装 https://chrome.google.com/webstore/detail/elasticsearch-head/ffmkiejjmecolpfloofpjologoblkegm

![image-20200922152428838](media/image-20200922152428838.png)

我们也可以新建索引

![image-20200922152534471](media/image-20200922152534471.png)

建议：推荐使用 chrome 插件的方式安装，如果网络环境不允许，就采用其它方式安装。

## RESTful API

在 Elasticsearch 中，提供了功能丰富的 RESTful API 的操作，包括基本的 CRUD、创建索引、删除索引等操作。

### 创建非结构化索引

在 Lucene 中，创建索引是需要定义字段名称以及字段的类型的，在 Elasticsearch 中提供了非结构化的索引，就是不需要创建索引结构，即可写入数据到索引中，实际上在 Elasticsearch 底层会进行结构化操作，此操作对用户是透明的。

### 创建空索引

```bash
PUT /haoke
{
    "settings": {
        "index": {
        "number_of_shards": "2", #分片数
        "number_of_replicas": "0" #副本数
        }
    }
}
```

### 删除索引

```bash
#删除索引
DELETE /haoke
{
	"acknowledged": true
}
```

### 插入数据

> URL 规则：
> POST /{索引}/{类型}/{id}

```bash
POST /haoke/user/1001
#数据
{
"id":1001,
"name":"张三",
"age":20,
"sex":"男"
}
```

使用 postman 操作成功后

![image-20200922155642306](media/image-20200922155642306.png)

我们通过 ElasticSearchHead 进行数据预览就能够看到我们刚刚插入的数据了

![image-20200922155843314](media/image-20200922155843314.png)

说明：非结构化的索引，不需要事先创建，直接插入数据默认创建索引。不指定 id 插入数据：

![image-20200922155935366](media/image-20200922155935366.png)

### 更新数据

在 Elasticsearch 中，文档数据是不为修改的，但是可以通过覆盖的方式进行更新。

```bash
PUT /haoke/user/1001
{
"id":1001,
"name":"张三",
"age":21,
"sex":"女"
}
```

更新结果如下：

![image-20200922160154599](media/image-20200922160154599.png)

![image-20200922160201130](media/image-20200922160201130.png)

可以看到数据已经被覆盖了。问题来了，可以局部更新吗？ -- 可以的。前面不是说，文档数据不能更新吗？ 其实是这样的：在内部，依然会查询到这个文档数据，然后进行覆盖操作，步骤如下：

1. 从旧文档中检索 JSON
2. 修改它
3. 删除旧文档
4. 索引新文档

```bash
#注意：这里多了_update标识
POST /haoke/user/1001/_update
{
"doc":{
"age":23
}
}
```

![image-20200922160709463](media/image-20200922160709463.png)

![image-20200922160717001](media/image-20200922160717001.png)

可以看到，数据已经是局部更新了

### 删除索引

在 Elasticsearch 中，删除文档数据，只需要发起 DELETE 请求即可，不用额外的参数

```bash
DELETE 1 /haoke/user/1001
```

![image-20200922160752862](media/image-20200922160752862.png)

需要注意的是，result 表示已经删除，version 也增加了。

如果删除一条不存在的数据，会响应 404

![image-20200922161627716](media/image-20200922161627716.png)

> 删除一个文档也不会立即从磁盘上移除，它只是被标记成已删除。Elasticsearch 将会在你之后添加更多索引的时候才会在后台进行删除内容的清理。【相当于批量操作】

### 搜索数据

#### 根据 id 搜索数据

```bash
GET /haoke/user/BbPe_WcB9cFOnF3uebvr
#返回的数据如下
{
    "_index": "haoke",
    "_type": "user",
    "_id": "BbPe_WcB9cFOnF3uebvr",
    "_version": 8,
    "found": true,
    "_source": { #原始数据在这里
        "id": 1002,
        "name": "李四",
        "age": 40,
        "sex": "男"
        }
}
```

#### 搜索全部数据

```bash
GET 1 /haoke/user/_search
```

注意，使用查询全部数据的时候，默认只会返回 10 条

![image-20200922162228822](media/image-20200922162228822.png)

#### 关键字搜索数据

```bash
#查询年龄等于20的用户
GET /haoke/user/_search?q=age:20
```

结果如下：

![image-20200922162309797](media/image-20200922162309797.png)

### DSL 搜索

Elasticsearch 提供丰富且灵活的查询语言叫做 DSL 查询(Query DSL),它允许你构建更加复杂、强大的查询。
DSL(Domain Specific Language 特定领域语言)以 JSON 请求体的形式出现。

```bash
POST /haoke/user/_search
#请求体
{
    "query" : {
        "match" : { #match只是查询的一种
        	"age" : 20
        }
    }
}
```

实现：查询年龄大于 30 岁的男性用户。

![image-20200922162943539](media/image-20200922162943539.png)

```bash
POST /haoke/user/_search
#请求数据
{
    "query": {
        "bool": {
            "filter": {
                    "range": {
                        "age": {
                        "gt": 30
                    }
                }
            },
            "must": {
                "match": {
                	"sex": "男"
                }
            }
        }
    }
}
```

查询出来的结果

![image-20200922163109515](media/image-20200922163109515.png)

#### 全文搜索

```bash
POST /haoke/user/_search
#请求数据
{
    "query": {
        "match": {
        	"name": "张三 李四"
        }
    }
}
```

![image-20200922163315285](media/image-20200922163315285.png)

高亮显示，只需要在添加一个 highlight 即可

```bash
POST /haoke/user/_search
#请求数据
{
    "query": {
        "match": {
        	"name": "张三 李四"
        }
    }
    "highlight": {
        "fields": {
        	"name": {}
        }
    }
}
```

![image-20200922163432853](media/image-20200922163432853.png)

#### 聚合

在 Elasticsearch 中，支持聚合操作，类似 SQL 中的 group by 操作。

```bash
POST /haoke/user/_search
{
    "aggs": {
        "all_interests": {
            "terms": {
                "field": "age"
            }
        }
    }
}
```

结果如下，我们通过年龄进行聚合

![image-20200922163614708](media/image-20200922163614708.png)

从结果可以看出，年龄 30 的有 2 条数据，20 的有一条，40 的一条。

## ElasticSearch 核心详解

### 文档

在 Elasticsearch 中，文档以 JSON 格式进行存储，可以是复杂的结构，如：

```bash
{
    "_index": "haoke",
    "_type": "user",
    "_id": "1005",
    "_version": 1,
    "_score": 1,
    "_source": {
        "id": 1005,
        "name": "孙七",
        "age": 37,
        "sex": "女",
        "card": {
            "card_number": "123456789"
         }
    }
}
```

其中，card 是一个复杂对象，嵌套的 Card 对象

#### 元数据（metadata）

一个文档不只有数据。它还包含了元数据(metadata)——关于文档的信息。三个必须的元数据节点是：

![image-20200922165956176](media/image-20200922165956176.png)

#### index

索引(index)类似于关系型数据库里的“数据库”——它是我们存储和索引关联数据的地方。

> 提示：事实上，我们的数据被存储和索引在分片(shards)中，索引只是一个把一个或多个分片分组在一起的逻辑空间。然而，这只是一些内部细节——我们的程序完全不用关心分片。对于我们的程序而言，文档存储在索引(index)中。剩下的细节由 Elasticsearch 关心既可。

#### \_type

在应用中，我们使用对象表示一些“事物”，例如一个用户、一篇博客、一个评论，或者一封邮件。每个对象都属于一个类(class)，这个类定义了属性或与对象关联的数据。user 类的对象可能包含姓名、性别、年龄和 Email 地址。
在关系型数据库中，我们经常将相同类的对象存储在一个表里，因为它们有着相同的结构。同理，在 Elasticsearch
中，我们使用相同类型(type)的文档表示相同的“事物”，因为他们的数据结构也是相同的。

每个类型(type)都有自己的映射(mapping)或者结构定义，就像传统数据库表中的列一样。所有类型下的文档被存储在同一个索引下，但是类型的映射(mapping)会告诉 Elasticsearch 不同的文档如何被索引。

\_type 的名字可以是大写或小写，不能包含下划线或逗号。我们将使用 blog 做为类型名。

#### \_id

id 仅仅是一个字符串，它与\_index 和\_type 组合时，就可以在 Elasticsearch 中唯一标识一个文档。当创建一个文
档，你可以自定义\_id ，也可以让 Elasticsearch 帮你自动生成（32 位长度）

### 查询响应

#### pretty

可以在查询 url 后面添加 pretty 参数，使得返回的 json 更易查看。

![image-20200923101056932](media/image-20200923101056932.png)

#### 指定响应字段

在响应的数据中，如果我们不需要全部的字段，可以指定某些需要的字段进行返回。通过添加 \_source

```bash
GET /haoke/user/1005?_source=id,name
#响应
{
    "_index": "haoke",
    "_type": "user",
    "_id": "1005",
    "_version": 1,
    "found": true,
    "_source": {
        "name": "孙七",
        "id": 1005
     }
}
```

如不需要返回元数据，仅仅返回原始数据，可以这样：

```bash
GET /haoke/1 user/1005/_source
```

![image-20200923101239226](media/image-20200923101239226.png)

还可以这样：

```bash
GET /haoke/user/1005/_source?_1 source=id,name
```

![image-20200923101319728](media/image-20200923101319728.png)

#### 判断文档是否存在

如果我们只需要判断文档是否存在，而不是查询文档内容，那么可以这样：

```bash
HEAD /haoke/user/1005
```

通过发送一个 head 请求，来判断数据是否存在

![image-20200923101354992](media/image-20200923101354992.png)

```bash
HEAD 1 /haoke/user/1006
```

![image-20200923101433608](media/image-20200923101433608.png)

> 当然，这只表示你在查询的那一刻文档不存在，但并不表示几毫秒后依旧不存在。另一个进程在这期间可能创建新文档。

### 批量操作

有些情况下可以通过批量操作以减少网络请求。如：批量查询、批量插入数据。

#### 批量查询

```bash
POST /haoke/user/_mget
{
	"ids" : [ "1001", "1003" ]
}
```

结果：

![image-20200923101540616](media/image-20200923101540616.png)

如果，某一条数据不存在，不影响整体响应，需要通过 found 的值进行判断是否查询到数据。

```bash
POST /haoke/user/_mget
{
	"ids" : [ "1001", "1006" ]
}
```

结果：

![image-20200923101721344](media/image-20200923101721344.png)

> 也就是说，一个数据的存在不会影响其它数据的返回

#### \_bulk 操作

在 Elasticsearch 中，支持批量的插入、修改、删除操作，都是通过\_bulk 的 api 完成的。

请求格式如下：（请求格式不同寻常）

```
{ action: { metadata }}
{ request body }
{ action: { metadata }}
{ request body }
...
```

批量插入数据：

```bash
{"create":{"_index":"haoke","_type":"user","_id":2001}}
{"id":2001,"name":"name1","age": 20,"sex": "男"}
{"create":{"_index":"haoke","_type":"user","_id":2002}}
{"id":2002,"name":"name2","age": 20,"sex": "男"}
{"create":{"_index":"haoke","_type":"user","_id":2003}}
{"id":2003,"name":"name3","age": 20,"sex": "男"}
```

注意最后一行的回车。

![image-20200923101946147](media/image-20200923101946147.png)

批量删除：

```bash
{"delete":{"_index":"haoke","_type":"user","_id":2001}}
{"delete":{"_index":"haoke","_type":"user","_id":2002}}
{"delete":{"_index":"haoke","_type":"user","_id":2003}}
```

由于 delete 没有请求体，所以，action 的下一行直接就是下一个 action。

![image-20200923102044231](media/image-20200923102044231.png)

其他操作就类似了。一次请求多少性能最高？

- 整个批量请求需要被加载到接受我们请求节点的内存里，所以请求越大，给其它请求可用的内存就越小。有一
  个最佳的 bulk 请求大小。超过这个大小，性能不再提升而且可能降低。
- 最佳大小，当然并不是一个固定的数字。它完全取决于你的硬件、你文档的大小和复杂度以及索引和搜索的负
  载。
- 幸运的是，这个最佳点(sweetspot)还是容易找到的：试着批量索引标准的文档，随着大小的增长，当性能开始
  降低，说明你每个批次的大小太大了。开始的数量可以在 1000~5000 个文档之间，如果你的文档非常大，可以使用较小的批次。
- 通常着眼于你请求批次的物理大小是非常有用的。一千个 1kB 的文档和一千个 1MB 的文档大不相同。一个好的
  批次最好保持在 5-15MB 大小间。

### 分页

和 SQL 使用 LIMIT 关键字返回只有一页的结果一样，Elasticsearch 接受 from 和 size 参数：

- size: 结果数，默认 10
- from: 跳过开始的结果数，默认 0

如果你想每页显示 5 个结果，页码从 1 到 3，那请求如下：

```bash
GET /_search?size=5
GET /_search?size=5&from=5
GET /_search?size=5&from=10
```

应该当心分页太深或者一次请求太多的结果。结果在返回前会被排序。但是记住一个搜索请求常常涉及多个分
片。每个分片生成自己排好序的结果，它们接着需要集中起来排序以确保整体排序正确。

```bash
GET /haoke/user/_1 search?size=1&from=2
```

![image-20200923102611567](media/image-20200923102611567.png)

#### 在集群系统中深度分页

为了理解为什么深度分页是有问题的，让我们假设在一个有 5 个主分片的索引中搜索。当我们请求结果的第一
页（结果 1 到 10）时，每个分片产生自己最顶端 10 个结果然后返回它们给请求节点(requesting node)，它再
排序这所有的 50 个结果以选出顶端的 10 个结果。

现在假设我们请求第 1000 页——结果 10001 到 10010。工作方式都相同，不同的是每个分片都必须产生顶端的
10010 个结果。然后请求节点排序这 50050 个结果并丢弃 50040 个！

你可以看到在分布式系统中，排序结果的花费随着分页的深入而成倍增长。这也是为什么网络搜索引擎中任何
语句不能返回多于 1000 个结果的原因。

### 映射

前面我们创建的索引以及插入数据，都是由 Elasticsearch 进行自动判断类型，有些时候我们是需要进行明确字段类型的，否则，自动判断的类型和实际需求是不相符的。

自动判断的规则如下：

![image-20200923103848097](media/image-20200923103848097.png)

Elasticsearch 中支持的类型如下：

![image-20200923103917807](media/image-20200923103917807.png)

- string 类型在 ElasticSearch 旧版本中使用较多，从 ElasticSearch 5.x 开始不再支持 string，由 text 和
  keyword 类型替代。
- text 类型，当一个字段是要被全文搜索的，比如 Email 内容、产品描述，应该使用 text 类型。设置 text 类型
  以后，字段内容会被分析，在生成倒排索引以前，字符串会被分析器分成一个一个词项。text 类型的字段
  不用于排序，很少用于聚合。
- keyword 类型适用于索引结构化的字段，比如 email 地址、主机名、状态码和标签。如果字段需要进行过
  滤(比如查找已发布博客中 status 属性为 published 的文章)、排序、聚合。keyword 类型的字段只能通过精
  确值搜索到。

#### 创建明确类型的索引：

> 如果你要像之前旧版版本一样兼容自定义 type ,需要将 \*\*i\*\*nclude_type_name=true 携带

```bash
put http://202.193.56.222:9200/itcast?include_type_name=true
{
    "settings":{
        "index":{
            "number_of_shards":"2",
            "number_of_replicas":"0"
        }
    },
    "mappings":{
        "person":{
            "properties":{
                "name":{
                    "type":"text"
                },
                "age":{
                    "type":"integer"
                },
                "mail":{
                    "type":"keyword"
                },
                "hobby":{
                    "type":"text"
                }
            }
        }
    }
}
```

查看映射

```bash
GET /itcast/_mapping
```

![image-20200923104201613](media/image-20200923104201613.png)

插入数据

```bash
POST /itcast/_bulk
{"index":{"_index":"itcast","_type":"person"}}
{"name":"张三","age": 20,"mail": "111@qq.com","hobby":"羽毛球、乒乓球、足球"}
{"index":{"_index":"itcast","_type":"person"}}
{"name":"李四","age": 21,"mail": "222@qq.com","hobby":"羽毛球、乒乓球、足球、篮球"}
{"index":{"_index":"itcast","_type":"person"}}
{"name":"王五","age": 22,"mail": "333@qq.com","hobby":"羽毛球、篮球、游泳、听音乐"}
{"index":{"_index":"itcast","_type":"person"}}
{"name":"赵六","age": 23,"mail": "444@qq.com","hobby":"跑步、游泳"}
{"index":{"_index":"itcast","_type":"person"}}
{"name":"孙七","age": 24,"mail": "555@qq.com","hobby":"听音乐、看电影"}
```

![image-20200923104551405](media/image-20200923104551405.png)

#### 测试搜索

```bash
POST /itcast/person/_search
{
    "query":{
        "match":{
            "hobby":"音乐"
        }
    }
}
```

![image-20200923104653427](media/image-20200923104653427.png)

### 结构化查询

#### term 查询

term 主要用于精确匹配哪些值，比如数字，日期，布尔值或 not_analyzed 的字符串(未经分析的文本数据类型)：

```bash
{ "term": { "age": 26 }}
{ "term": { "date": "2014-09-01" }}
{ "term": { "public": true }}
{ "term": { "tag": "full_text" }}
```

示例

```bash
POST /itcast/person/_search
{
    "query":{
        "term":{
            "age":20
        }
    }
}
```

![image-20200923104851159](media/image-20200923104851159.png)

#### terms 查询

terms 跟 term 有点类似，但 terms 允许指定多个匹配条件。 如果某个字段指定了多个值，那么文档需要一起去
做匹配：

```bash
{
    "terms":{
        "tag":[
            "search",
            "full_text",
            "nosql"
        ]
    }
}
```

示例：

```bash
POST /itcast/person/_search
{
    "query":{
        "terms":{
            "age":[
                20,
                21
            ]
        }
    }
}
```

![image-20200923105030182](media/image-20200923105030182.png)

#### range 查询

range 过滤允许我们按照指定范围查找一批数据：

```bash
{
    "range":{
        "age":{
            "gte":20,
            "lt":30
        }
    }
}
```

范围操作符包含：

- gt : 大于
- gte:: 大于等于
- lt : 小于
- lte: 小于等于

示例：

```bash
POST /itcast/person/_search
{
    "query":{
        "range":{
            "age":{
                "gte":20,
                "lte":22
            }
        }
    }
}
```

#### exists 查询

exists 查询可以用于查找文档中是否包含指定字段或没有某个字段，类似于 SQL 语句中的 IS_NULL 条件

```bash
{
    "exists": {
    	"field": "title"
    }
}
```

这两个查询只是针对已经查出一批数据来，但是想区分出某个字段是否存在的时候使用。示例：

```bash
POST /haoke/user/_search
{
    "query": {
        "exists": { #必须包含
        	"field": "card"
        }
    }
}
```

![image-20200923105416339](media/image-20200923105416339.png)

#### match 查询

match 查询是一个标准查询，不管你需要全文本查询还是精确查询基本上都要用到它。

如果你使用 match 查询一个全文本字段，它会在真正查询之前用分析器先分析 match 一下查询字符：

```bash
{
    "match": {
    	"tweet": "About Search"
    }
}
```

如果用 match 下指定了一个确切值，在遇到数字，日期，布尔值或者 not_analyzed 的字符串时，它将为你搜索你
给定的值：

```bash
{ "match": { "age": 26 }}
{ "match": { "date": "2014-09-01" }}
{ "match": { "public": true }}
{ "match": { "tag": "full_text" }}
```

#### bool 查询

- bool 查询可以用来合并多个条件查询结果的布尔逻辑，它包含一下操作符：
- must :: 多个查询条件的完全匹配,相当于 and 。
- must_not :: 多个查询条件的相反匹配，相当于 not 。
- should :: 至少有一个查询条件匹配, 相当于 or 。

这些参数可以分别继承一个查询条件或者一个查询条件的数组：

```bash
{
    "bool":{
        "must":{
            "term":{
                "folder":"inbox"
            }
        },
        "must_not":{
            "term":{
                "tag":"spam"
            }
        },
        "should":[
            {
                "term":{
                    "starred":true
                }
            },
            {
                "term":{
                    "unread":true
                }
            }
        ]
    }
}
```

### 过滤查询

前面讲过结构化查询，Elasticsearch 也支持过滤查询，如 term、range、match 等。

示例：查询年龄为 20 岁的用户。

```bash
POST /itcast/person/_search
{
    "query":{
        "bool":{
            "filter":{
                "term":{
                    "age":20
                }
            }
        }
    }
}
```

#### 查询和过滤的对比

- 一条过滤语句会询问每个文档的字段值是否包含着特定值。
- 查询语句会询问每个文档的字段值与特定值的匹配程度如何。
- 一条查询语句会计算每个文档与查询语句的相关性，会给出一个相关性评分 \_score，并且 按照相关性对匹
  配到的文档进行排序。 这种评分方式非常适用于一个没有完全配置结果的全文本搜索。
- 一个简单的文档列表，快速匹配运算并存入内存是十分方便的， 每个文档仅需要 1 个字节。这些缓存的过滤结果集与后续请求的结合使用是非常高效的。
- 查询语句不仅要查找相匹配的文档，还需要计算每个文档的相关性，所以一般来说查询语句要比 过滤语句更耗时，并且查询结果也不可缓存。

#### 建议：

做精确匹配搜索时，最好用过滤语句，因为过滤语句可以缓存数据。

## 3：中文分词

分词就是指将一个文本转化成一系列单词的过程，也叫文本分析，在 Elasticsearch 中称之为 Analysis。

举例：我是中国人 --> 我/是/中国人

### 分词 api

指定分词器进行分词

```bash
POST /_analyze
{
    "analyzer":"standard",
    "text":"hello world"
}
```

结果如下

![image-20200923110814505](media/image-20200923110814505.png)

在结果中不仅可以看出分词的结果，还返回了该词在文本中的位置。

> 指定索引分词

```bash
POST /itcast/_analyze
{
    "analyzer": "standard",
    "field": "hobby",
    "text": "听音乐"
}
```

![image-20200923110905001](media/image-20200923110905001.png)

### 中文分词难点

中文分词的难点在于，在汉语中没有明显的词汇分界点，如在英语中，空格可以作为分隔符，如果分隔不正确就会造成歧义。如：

- 我/爱/炒肉丝
- 我/爱/炒/肉丝

常用中文分词器，IK、jieba、THULAC 等，推荐使用 IK 分词器。

IK Analyzer 是一个开源的，基于 java 语言开发的轻量级的中文分词工具包。从 2006 年 12 月推出 1.0 版开始，IKAnalyzer 已经推出了 3 个大版本。最初，它是以开源项目 Luence 为应用主体的，结合词典分词和文法分析算法的中文分词组件。新版本的 IK Analyzer 3.0 则发展为面向 Java 的公用分词组件，独立于 Lucene 项目，同时提供了对 Lucene 的默认优化实现。

采用了特有的“正向迭代最细粒度切分算法“，具有 80 万字/秒的高速处理能力 采用了多子处理器分析模式，支持：英文字母（IP 地址、Email、URL）、数字（日期，常用中文数量词，罗马数字，科学计数法），中文词汇（姓名、地名处理）等分词处理。 优化的词典存储，更小的内存占用。

IK 分词器 Elasticsearch 插件地址：https://github.com/medcl/elasticsearch-analysis-ik

### 安装分词器

首先下载到最新的 ik 分词器：[下载地址](https://github.com/medcl/elasticsearch-analysis-ik/releases/tag/v7.9.1)

下载完成后，使用 xftp 工具，拷贝到服务器上

```bash
#安装方法：将下载到的 es/plugins/ik 目录下
mkdir es/plugins/ik

#解压
unzip elasticsearch-analysis-ik-7.9.1.zip

#重启
./bin/elasticsearch
```

我们通过日志，发现它已经成功加载了 ik 分词器插件

![image-20200923113200826](media/image-20200923113200826.png)

### 测试

```bash
POST /_analyze
{
    "analyzer": "ik_max_word",
    "text": "我是中国人"
}
```

我们发现 ik 分词器已经成功分词完成

![image-20200923113222937](media/image-20200923113222937.png)

### 中文分词器原理

分词器的原理本质上是词典分词。在现有内存中初始化一个词典，然后在分词过程中挨个读取字符和字典中的字符相匹配，把文档中所有词语拆分出来的过程。

字典树，Trie 树，是一种树形结构，是一种哈希树的变种。典型应用是用于统计，排序和保存大量的字符串（但不仅限于字符串），所以经常被搜索引擎系统用于文本词频统计。它的优点是：利用字符串的公共前缀来减少查询时间，最大限度地减少无谓的字符串比较，查询效率比哈希树高。

下面一个存放了[大学、大学生、学习、学习机、学生、生气、生活、活着]这个词典的 trie 树：

![img](media/b663b55102c3454bbeb6060db9a0b796.png)

它可以看作是用每个词第 n 个字做第 n 到第 n+1 层节点间路径哈希值的哈希树，每个节点是实际要存放的词。

现在用这个树来进行“大学生活”的匹配。依然从“大”字开始匹配，如下图所示：从根节点开始，沿最左边的路径匹配到了大字，沿着“大”节点可以匹配到“大学”,继续匹配则可以匹配到“大学生”，之后字典中再没有以“大”字开头的词，至此已经匹配到了[大学、大学生]第一轮匹配结束

![img](media/a83382b0cf4e49d5bbcc842b65a504d4.png)

继续匹配“学”字开头的词，方法同上步，可匹配出[学生]

![img](media/4fb1bfba7def46daa2a6b5c667613387.png)

继续匹配“生”和“活”字开头的词，这样“大学生活”在词典中的词全部被查出来。

可以看到，以匹配“大”字开头的词为例，第一种匹配方式需要在词典中查询是否包含“大”、“大学”、“大学”、“大学生活”，共 4 次查询，而使用 trie 树查询时当找到“大学生”这个词之后就停止了该轮匹配，减少了匹配的次数，当要匹配的句子越长，这种性能优势就越明显。

失败指针

再来看一下上面的匹配过程，在匹配“大学生”这个词之后，由于词典中不存在其它以“大”字开头的词，本轮结束，将继续匹配以“学”字开头的词，这时，需要再回到根节点继续匹配，如果这个时候“大学生”节点有个指针可以直指向“学生”节点，就可以减少一次查询，类似地，当匹配完“学生”之后如果“学生”节点有个指针可以指向“生活”节点，就又可以减少一次查询。这种当下一层节点无法匹配需要进行跳转的指针就是失败指针，创建好失败指针的树看起来如下图：

![img](media/76e23ae9c1f54f78b9480298e21ce603.png)

图上红色的线就是失败指针，指向的是当下层节点无法匹配时应该跳转到哪个节点继续进行匹配

失败指针的创建过程通常为：

- 创建好 trie 树。

- BFS 每一个节点(不能使用 DFS，因为每一层节点的失败指针在创建时要确保上一层节点的失败指针全部创建完成)。

- 根节点的子节点的失败指针指向根节点。

- 其它节点查找其父节点的失败指针指向的节点的子节点是否有和该节点字相同的节点，如果有则失败指针指向该节点，如果没有则重复刚才的过程直至找到字相同的节点或根节点。

查询过程如下：

![img](media/2f92164301e54a0b8ed7263419cb58e2.png)

## 全文搜索

全文搜索两个最重要的方面是：

- 相关性（Relevance） 它是评价查询与其结果间的相关程度，并根据这种相关程度对结果排名的一种能力，这
  种计算方式可以是 TF/IDF 方法、地理位置邻近、模糊相似，或其他的某些算法。
- 分词（Analysis） 它是将文本块转换为有区别的、规范化的 token 的一个过程，目的是为了创建倒排索引以及查询倒排索引。

### 构造数据

> ES 7.4 默认不在支持指定索引类型，默认索引类型是\_doc

```bash
http://202.193.56.222:9200/itcast?include_type_name=true
{
    "settings":{
        "index":{
            "number_of_shards":"1",
            "number_of_replicas":"0"
        }
    },
    "mappings":{
        "person":{
            "properties":{
                "name":{
                    "type":"text"
                },
                "age":{
                    "type":"integer"
                },
                "mail":{
                    "type":"keyword"
                },
                "hobby":{
                    "type":"text",
                    "analyzer":"ik_max_word"
                }
            }
        }
    }
}
```

然后插入数据

```bash
POST http://202.193.56.222:9200/itcast/_bulk
{"index":{"_index":"itcast","_type":"person"}}
{"name":"张三","age": 20,"mail": "111@qq.com","hobby":"羽毛球、乒乓球、足球"}
{"index":{"_index":"itcast","_type":"person"}}
{"name":"李四","age": 21,"mail": "222@qq.com","hobby":"羽毛球、乒乓球、足球、篮球"}
{"index":{"_index":"itcast","_type":"person"}}
{"name":"王五","age": 22,"mail": "333@qq.com","hobby":"羽毛球、篮球、游泳、听音乐"}
{"index":{"_index":"itcast","_type":"person"}}
{"name":"赵六","age": 23,"mail": "444@qq.com","hobby":"跑步、游泳、篮球"}
{"index":{"_index":"itcast","_type":"person"}}
{"name":"孙七","age": 24,"mail": "555@qq.com","hobby":"听音乐、看电影、羽毛球"}
```

![image-20200923141912153](media/image-20200923141912153.png)

### 单词搜索

```bash
POST /itcast/person/_search
{
    "query":{
        "match":{
            "hobby":"音乐"
        }
    },
    "highlight":{
        "fields":{
            "hobby":{

            }
        }
    }
}
```

查询出来的结果如下，并且还带有高亮

![image-20200923142131426](media/image-20200923142131426.png)

过程说明：

- 检查字段类型
  - 爱好 hobby 字段是一个 text 类型（ 指定了 IK 分词器），这意味着查询字符串本身也应该被分词。
- 分析查询字符串 。
  - 将查询的字符串 “音乐” 传入 IK 分词器中，输出的结果是单个项 音乐。因为只有一个单词项，所以 match 查询执行的是单个底层 term 查询。
- 查找匹配文档 。
  - 用 term 查询在倒排索引中查找 “音乐” 然后获取一组包含该项的文档，本例的结果是文档：3 、5 。
- 为每个文档评分 。
  - 用 term 查询计算每个文档相关度评分 \_score ，这是种将 词频（term frequency，即词 “音乐” 在相关文档的 hobby 字段中出现的频率）和 反向文档频率（inverse document frequency，即词 “音乐” 在所有文档的 hobby 字段中出现的频率），以及字段的长度（即字段越短相关度越高）相结合的计算方式。

### 多词搜索

```bash
POST /itcast/person/_search
{
    "query":{
        "match":{
            "hobby":"音乐 篮球"
        }
    },
    "highlight":{
        "fields":{
            "hobby":{

            }
        }
    }
}
```

可以看到，包含了“音乐”、“篮球”的数据都已经被搜索到了。可是，搜索的结果并不符合我们的预期，因为我们想搜索的是既包含“音乐”又包含“篮球”的用户，显然结果返回的“或”的关系。在 Elasticsearch 中，可以指定词之间的逻辑关系，如下：

```bash
POST /itcast/person/_search
{
    "query":{
        "match":{
            "hobby":"音乐 篮球"
            "operator":"and"
        }
    },
    "highlight":{
        "fields":{
            "hobby":{

            }
        }
    }
}
```

通过这样的话，就会让两个关键字之间存在 and 关系了

![image-20200923142538105](media/image-20200923142538105.png)

可以看到结果符合预期。

前面我们测试了“OR” 和 “AND”搜索，这是两个极端，其实在实际场景中，并不会选取这 2 个极端，更有可能是选取这种，或者说，只需要符合一定的相似度就可以查询到数据，在 Elasticsearch 中也支持这样的查询，通过
minimum_should_match 来指定匹配度，如：70%；

示例：

```bash
{
    "query":{
        "match":{
        "hobby":{
            "query":"游泳 羽毛球",
            "minimum_should_match":"80%"
            }
            }
            },
        "highlight": {
        "fields": {
        "hobby": {}
        }
    }
}
#结果：省略显示
"hits": {
"total": 4, #相似度为80%的情况下，查询到4条数据
"max_score": 1.621458,
"hits": [

}
#设置40%进行测试：
{
    "query":{
        "match":{
            "hobby":{
            "query":"游泳 羽毛球",
            "minimum_should_match":"40%"
            }
            }
            },
            "highlight": {
            "fields": {
            "hobby": {}
        }
    }
}

#结果：
"hits": {
"total": 5, #相似度为40%的情况下，查询到5条数据
"max_score": 1.621458,
"hits": [

}
```

相似度应该多少合适，需要在实际的需求中进行反复测试，才可得到合理的值。

### 组合搜索

在搜索时，也可以使用过滤器中讲过的 bool 组合查询，示例：

```bash
POST /itcast/person/_search
{
    "query":{
        "bool":{
            "must":{
                "match":{
                    "hobby":"篮球"
                }
            },
            "must_not":{
                "match":{
                    "hobby":"音乐"
                }
            },
            "should":[
                {
                    "match":{
                        "hobby":"游泳"
                    }
                }
            ]
        }
    },
    "highlight":{
        "fields":{
            "hobby":{

            }
        }
    }
}
```

> 上面搜索的意思是：
> 搜索结果中必须包含篮球，不能包含音乐，如果包含了游泳，那么它的相似度更高。

结果：

![image-20200923145310698](media/image-20200923145310698.png)

> 评分的计算规则
>
> bool 查询会为每个文档计算相关度评分 \_score ， 再将所有匹配的 must 和 should 语句的分数 \_score 求和，最后除以 must 和 should 语句的总数。
>
> must_not 语句不会影响评分； 它的作用只是将不相关的文档排除。

默认情况下，should 中的内容不是必须匹配的，如果查询语句中没有 must，那么就会至少匹配其中一个。当然了，也可以通过 minimum_should_match 参数进行控制，该值可以是数字也可以的百分比。

示例：

```bash
POST /itcast/person/_search
{
    "query":{
        "bool":{
            "should":[
                {
                    "match":{
                        "hobby":"游泳"
                    }
                },
                {
                    "match":{
                        "hobby":"篮球"
                    }
                },
                {
                    "match":{
                        "hobby":"音乐"
                    }
                }
            ],
            "minimum_should_match":2
        }
    },
    "highlight":{
        "fields":{
            "hobby":{

            }
        }
    }
}
```

minimum_should_match 为 2，意思是 should 中的三个词，至少要满足 2 个。

### 权重

有些时候，我们可能需要对某些词增加权重来影响该条数据的得分。如下：

搜索关键字为“游泳篮球”，如果结果中包含了“音乐”权重为 10，包含了“跑步”权重为 2。

```bash
POST /itcast/person/_search
{
    "query":{
        "bool":{
            "must":{
                "match":{
                    "hobby":{
                        "query":"游泳篮球",
                        "operator":"and"
                    }
                }
            },
            "should":[
                {
                    "match":{
                        "hobby":{
                            "query":"音乐",
                            "boost":10
                        }
                    }
                },
                {
                    "match":{
                        "hobby":{
                            "query":"跑步",
                            "boost":2
                        }
                    }
                }
            ]
        }
    },
    "highlight":{
        "fields":{
            "hobby":{

            }
        }
    }
}
```

## ElasticSearch 集群

### 集群节点

ELasticsearch 的集群是由多个节点组成的，通过 cluster.name 设置集群名称，并且用于区分其它的集群，每个节点通过 node.name 指定节点的名称。

在 Elasticsearch 中，节点的类型主要有 4 种：

- master 节点
  - 配置文件中 node.master 属性为 true(默认为 true)，就有资格被选为 master 节点。master 节点用于控制整个集群的操作。比如创建或删除索引，管理其它非 master 节点等。
- data 节点
  - 配置文件中 node.data 属性为 true(默认为 true)，就有资格被设置成 data 节点。data 节点主要用于执行数据相关的操作。比如文档的 CRUD。
- 客户端节点
  - 配置文件中 node.master 属性和 node.data 属性均为 false。
  - 该节点不能作为 master 节点，也不能作为 data 节点。
  - 可以作为客户端节点，用于响应用户的请求，把请求转发到其他节点
- 部落节点
  - 当一个节点配置 tribe.\*的时候，它是一个特殊的客户端，它可以连接多个集群，在所有连接的集群上执行
    搜索和其他操作。

### 搭建集群

```bash
#启动3个虚拟机，分别在3台虚拟机上部署安装Elasticsearch
mkdir /itcast/es-cluster

#分发到其它机器
scp -r es-cluster elsearch@192.168.40.134:/itcast

#node01的配置：
cluster.name: es-itcast-cluster
node.name: node01
node.master: true
node.data: true
network.host: 0.0.0.0
http.port: 9200
discovery.zen.ping.unicast.hosts: ["192.168.40.133","192.168.40.134","192.168.40.135"]
# 最小节点数
discovery.zen.minimum_master_nodes: 2
# 跨域专用
http.cors.enabled: true
http.cors.allow-origin: "*"

#node02的配置：
cluster.name: es-itcast-cluster
node.name: node02
node.master: true
node.data: true
network.host: 0.0.0.0
http.port: 9200
discovery.zen.ping.unicast.hosts: ["192.168.40.133","192.168.40.134","192.168.40.135"]
discovery.zen.minimum_master_nodes: 2
http.cors.enabled: true
http.cors.allow-origin: "*"

#node03的配置：
cluster.name: es-itcast-cluster
node.name: node02
node.master: true
node.data: true
network.host: 0.0.0.0
http.port: 9200
discovery.zen.ping.unicast.hosts: ["192.168.40.133","192.168.40.134","192.168.40.135"]
discovery.zen.minimum_master_nodes: 2
http.cors.enabled: true
http.cors.allow-origin: "*"

#分别启动3个节点
./elasticsearch
```

查看集群

![image-20200923151823672](media/image-20200923151823672.png)

创建索引：

![image-20200923151851785](media/image-20200923151851785.png)

![image-20200923151935283](media/image-20200923151935283.png)

查询集群状态：/\_cluster/health
响应：

![image-20200923151953227](media/image-20200923151953227.png)

集群中有三种颜色

![image-20200923152005930](media/image-20200923152005930.png)

### 分片和副本

为了将数据添加到 Elasticsearch，我们需要索引(index)——一个存储关联数据的地方。实际上，索引只是一个用来指向一个或多个分片(shards)的“逻辑命名空间(logical namespace)”.

- 一个分片(shard)是一个最小级别“工作单元(worker unit)”,它只是保存了索引中所有数据的一部分。
- 我们需要知道是分片就是一个 Lucene 实例，并且它本身就是一个完整的搜索引擎。应用程序不会和它直接通
  信。
- 分片可以是主分片(primary shard)或者是复制分片(replica shard)。
- 索引中的每个文档属于一个单独的主分片，所以主分片的数量决定了索引最多能存储多少数据。
- 复制分片只是主分片的一个副本，它可以防止硬件故障导致的数据丢失，同时可以提供读请求，比如搜索或者从别的 shard 取回文档。
- 当索引创建完成的时候，主分片的数量就固定了，但是复制分片的数量可以随时调整。

### 故障转移

#### 将 data 节点停止

这里选择将 node02 停止：

![image-20200923152229908](media/image-20200923152229908.png)

当前集群状态为黄色，表示主节点可用，副本节点不完全可用，过一段时间观察，发现节点列表中看不到 node02，副本节点分配到了 node01 和 node03，集群状态恢复到绿色。

![image-20200923152248547](media/image-20200923152248547.png)

将 node02 恢复： ./node02/1 bin/elasticsearch

![image-20200923152328458](media/image-20200923152328458.png)

可以看到，node02 恢复后，重新加入了集群，并且重新分配了节点信息。

#### 将 master 节点停止

接下来，测试将 node01 停止，也就是将主节点停止。

![image-20200923152415890](media/image-20200923152415890.png)

从结果中可以看出，集群对 master 进行了重新选举，选择 node03 为 master。并且集群状态变成黄色。
等待一段时间后，集群状态从黄色变为了绿色：

![image-20200923153343555](media/image-20200923153343555.png)

恢复 node01 节点：

```bash
./node01/1 bin/elasticsearch
```

重启之后，发现 node01 可以正常加入到集群中，集群状态依然为绿色：

![image-20200923153415117](media/image-20200923153415117.png)

特别说明：

如果在配置文件中 discovery.zen.minimum_master_nodes 设置的不是 N/2+1 时，会出现脑裂问题，之前宕机
的主节点恢复后不会加入到集群。

![image-20200923153441693](media/image-20200923153441693.png)

### 选举

Elasticsearch 的选主是 ZenDiscovery 模块负责的，主要包含 Ping（节点之间通过这个 RPC 来发现彼此）和 Unicast（单播模块包含一个主机列表以控制哪些节点需要 ping 通）这两部分；
　　对所有可以成为 master 的节点（node.master: true）根据 nodeId 字典排序，每次选举每个节点都把自己所知道节点排一次序，然后选出第一个（第 0 位）节点，暂且认为它是 master 节点。
　　如果对某个节点的投票数达到一定的值（可以成为 master 节点数 n/2+1）并且该节点自己也选举自己，那这个节点就是 master。否则重新选举一直到满足上述条件。

### 脑裂

第一种情况：当选举的两个都是 n/2 时怎么办？

第二种情况：选举资格的节点时有机会透两票的，比如有三备选个节点的集群 ABC，应该选 a 为 master,开始选举 b 先投了 a，然后 a 也投了自己，符合大多数原则，按理应该 a 返回消息，告诉大家我当选，结束选举，但是 b 投票后，网络通讯变慢 b 迟迟没收到 a 的回复导致超时，b 又投了 c 一票，此时 c 也与 a 断开，也投了自己，c 又当选，这时又出现了脑裂

解决办法：ES 规定超过 n/2+1 的才能称为主节点，

### 分布式文档

#### 路由

首先，来看个问题：

![image-20200923153556720](media/image-20200923153556720.png)

如图所示：当我们想一个集群保存文档时，文档该存储到哪个节点呢？ 是随机吗？ 是轮询吗？实际上，在 ELasticsearch 中，会采用计算的方式来确定存储到哪个节点，计算公式如下：

```bash
shard = hash(routing) % number_1 of_primary_shards
```

其中：

- routing 值是一个任意字符串，它默认是\_id 但也可以自定义。
- 这个 routing 字符串通过哈希函数生成一个数字，然后除以主切片的数量得到一个余数(remainder)，余数
  的范围永远是 0 到 number_of_primary_shards - 1，这个数字就是特定文档所在的分片

这就是为什么创建了主分片后，不能修改的原因。

#### 文档的写操作

新建、索引和删除请求都是写（write）操作，它们必须在主分片上成功完成才能复制分片上

![image-20200923155314424](media/image-20200923155314424.png)

下面我们罗列在主分片和复制分片上成功新建、索引或删除一个文档必要的顺序步骤：

1. 客户端给 Node 1 发送新建、索引或删除请求。
2. 节点使用文档的\_id 确定文档属于分片 0 。它转发请求到 Node 3 ，分片 0 位于这个节点上。
3. Node 3 在主分片上执行请求，如果成功，它转发请求到相应的位于 Node 1 和 Node 2 的复制节点上。当所有
   的复制节点报告成功， Node 3 报告成功到请求的节点，请求的节点再报告给客户端。

客户端接收到成功响应的时候，文档的修改已经被应用于主分片和所有的复制分片。你的修改生效了。

### 搜索文档

文档能够从主分片或任意一个复制分片被检索。

![image-20200923160046962](media/image-20200923160046962.png)

下面我们罗列在主分片或复制分片上检索一个文档必要的顺序步骤：

1. 客户端给 Node 1 发送 get 请求。
2. 节点使用文档的\_id 确定文档属于分片 0 。分片 0 对应的复制分片在三个节点上都有。此时，它转发请求到 Node 2 。
3. Node 2 返回文档(document)给 Node 1 然后返回给客户端。对于读请求，为了平衡负载，请求节点会为每个请求选择不同的分片——它会循环所有分片副本。可能的情况是，一个被索引的文档已经存在于主分片上却还没来得及同步到复制分片上。这时复制分片会报告文档未找到，主分片会成功返回文档。一旦索引请求成功返回给用户，文档则在主分片和复制分片都是可用的。

### 全文搜索

对于全文搜索而言，文档可能分散在各个节点上，那么在分布式的情况下，如何搜索文档呢？

搜索，分为 2 个阶段，

- 搜索（query）
- 取回（fetch）

#### 搜索（query）

![image-20200923161323235](media/image-20200923161323235.png)

查询阶段包含以下三步：

1. 客户端发送一个 search（搜索） 请求给 Node 3 , Node 3 创建了一个长度为 from+size 的空优先级队
2. Node 3 转发这个搜索请求到索引中每个分片的原本或副本。每个分片在本地执行这个查询并且结果将结果到
   一个大小为 from+size 的有序本地优先队列里去。
3. 每个分片返回 document 的 ID 和它优先队列里的所有 document 的排序值给协调节点 Node 3 。Node 3 把这些
   值合并到自己的优先队列里产生全局排序结果。

#### 取回 fetch

![image-20200923161447618](media/image-20200923161447618.png)

分发阶段由以下步骤构成：

1. 协调节点辨别出哪个 document 需要取回，并且向相关分片发出 GET 请求。
2. 每个分片加载 document 并且根据需要丰富（enrich）它们，然后再将 document 返回协调节点。
3. 一旦所有的 document 都被取回，协调节点会将结果返回给客户端。

## Java 客户端

在 Elasticsearch 中，为 java 提供了 2 种客户端，一种是 REST 风格的客户端，另一种是 Java API 的客户端

### REST 客户端

Elasticsearch 提供了 2 种 REST 客户端，一种是低级客户端，一种是高级客户端。

- Java Low Level REST Client：官方提供的低级客户端。该客户端通过 http 来连接 Elasticsearch 集群。用户在使
  用该客户端时需要将请求数据手动拼接成 Elasticsearch 所需 JSON 格式进行发送，收到响应时同样也需要将返回的 JSON 数据手动封装成对象。虽然麻烦，不过该客户端兼容所有的 Elasticsearch 版本。
- Java High Level REST Client：官方提供的高级客户端。该客户端基于低级客户端实现，它提供了很多便捷的
  API 来解决低级客户端需要手动转换数据格式的问题。

### 构造数据

```bash
POST /haoke/house/_bulk

{"index":{"_index":"haoke","_type":"house"}}
{"id":"1001","title":"整租 · 南丹大楼 1居室 7500","price":"7500"}
{"index":{"_index":"haoke","_type":"house"}}
{"id":"1002","title":"陆家嘴板块，精装设计一室一厅，可拎包入住诚意租。","price":"8500"}
{"index":{"_index":"haoke","_type":"house"}}
{"id":"1003","title":"整租 · 健安坊 1居室 4050","price":"7500"}
{"index":{"_index":"haoke","_type":"house"}}
{"id":"1004","title":"整租 · 中凯城市之光+视野开阔+景色秀丽+拎包入住","price":"6500"}
{"index":{"_index":"haoke","_type":"house"}}
{"id":"1005","title":"整租 · 南京西路品质小区 21213三轨交汇 配套齐* 拎包入住","price":"6000"}
{"index":{"_index":"haoke","_type":"house"}}
{"id":"1006","title":"祥康里 简约风格 *南户型 拎包入住 看房随时","price":"7000"}
```

![image-20200923162419395](media/image-20200923162419395.png)

### REST 低级客户端

创建项目，加入依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>

    <groupId>org.example</groupId>
    <artifactId>Study_ElasticSearch_Code</artifactId>
    <version>1.0-SNAPSHOT</version>
    <build>
        <plugins>
            <plugin>
                <groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-compiler-plugin</artifactId>
                <configuration>
                    <source>7</source>
                    <target>7</target>
                </configuration>
            </plugin>
        </plugins>
    </build>

    <dependencies>
        <dependency>
            <groupId>org.elasticsearch.client</groupId>
            <artifactId>elasticsearch-rest-client</artifactId>
            <version>6.8.5</version>
        </dependency>
        <dependency>
            <groupId>junit</groupId>
            <artifactId>junit</artifactId>
            <version>4.12</version>
            <scope>test</scope>
        </dependency>
        <dependency>
            <groupId>com.fasterxml.jackson.core</groupId>
            <artifactId>jackson-databind</artifactId>
            <version>2.11.1</version>
        </dependency>
    </dependencies>
</project>
```

编写测试类

```java
import com.fasterxml.jackson.core.JsonProcessingException;
import com.fasterxml.jackson.databind.ObjectMapper;
import org.apache.http.HttpHost;
import org.apache.http.util.EntityUtils;
import org.elasticsearch.client.Request;
import org.elasticsearch.client.Response;
import org.elasticsearch.client.RestClient;
import org.elasticsearch.client.RestClientBuilder;

import java.io.IOException;
import java.util.HashMap;
import java.util.Map;

/**
 * 使用低级客户端 访问
 *
 * @author: 陌溪
 * @create: 2020-09-23-16:33
 */
public class ESApi {
    private RestClient restClient;
    private static final ObjectMapper MAPPER = new ObjectMapper();

    /**
     * 初始化
     */
    public void init() {
        RestClientBuilder restClientBuilder = RestClient.builder(new HttpHost("202.193.56.222", 9200, "http"));
        this.restClient = restClientBuilder.build();
    }

    /**
     * 查询集群状态
     */
    public void testGetInfo() throws IOException {
        Request request = new Request("GET", "/_cluster/state");
        request.addParameter("pretty", "true");
        Response response = this.restClient.performRequest(request);
        System.out.println(response.getStatusLine());
        System.out.println(EntityUtils.toString(response.getEntity()));
    }

    /**
     * 根据ID查询数据
     * @throws IOException
     */
    public void testGetHouseInfo() throws IOException {
        Request request = new Request("GET", "/haoke/house/Z3CduXQBYpWein3CRFug");
        request.addParameter("pretty", "true");
        Response response = this.restClient.performRequest(request);
        System.out.println(response.getStatusLine());
        System.out.println(EntityUtils.toString(response.getEntity()));
    }

    public void testCreateData() throws IOException {
        Request request = new Request("POST", "/haoke/house");
        Map<String, Object> data = new HashMap<>();
        data.put("id", "2001");
        data.put("title", "张江高科");
        data.put("price", "3500");
        // 写成JSON
        request.setJsonEntity(MAPPER.writeValueAsString(data));
        Response response = this.restClient.performRequest(request);
        System.out.println(response.getStatusLine());
        System.out.println(EntityUtils.toString(response.getEntity()));

    }

    // 搜索数据
    public void testSearchData() throws IOException {
        Request request = new Request("POST", "/haoke/house/_search");
        String searchJson = "{\"query\": {\"match\": {\"title\": \"拎包入住\"}}}";
        request.setJsonEntity(searchJson);
        request.addParameter("pretty","true");
        Response response = this.restClient.performRequest(request);
        System.out.println(response.getStatusLine());
        System.out.println(EntityUtils.toString(response.getEntity()));
    }

    public static void main(String[] args) throws IOException {
        ESApi esApi = new ESApi();
        esApi.init();
//        esApi.testGetInfo();
//        esApi.testGetHouseInfo();
        esApi.testCreateData();
    }
}
```

### REST 高级客户端

创建项目，引入依赖

```xml
<dependency>
    <groupId>org.elasticsearch.client</groupId>
    <artifactId>elasticsearch-rest-high-level-client</artifactId>
    <version>6.8.5</version>
</dependency>
```

编写测试用例

```java
import com.fasterxml.jackson.databind.ObjectMapper;
import org.apache.http.HttpHost;
import org.apache.http.util.EntityUtils;
import org.elasticsearch.action.ActionListener;
import org.elasticsearch.action.delete.DeleteRequest;
import org.elasticsearch.action.delete.DeleteResponse;
import org.elasticsearch.action.get.GetRequest;
import org.elasticsearch.action.get.GetResponse;
import org.elasticsearch.action.index.IndexRequest;
import org.elasticsearch.action.index.IndexResponse;
import org.elasticsearch.action.search.SearchRequest;
import org.elasticsearch.action.search.SearchResponse;
import org.elasticsearch.action.update.UpdateRequest;
import org.elasticsearch.action.update.UpdateResponse;
import org.elasticsearch.client.*;
import org.elasticsearch.common.Strings;
import org.elasticsearch.common.unit.TimeValue;
import org.elasticsearch.index.query.QueryBuilders;
import org.elasticsearch.search.SearchHit;
import org.elasticsearch.search.SearchHits;
import org.elasticsearch.search.builder.SearchSourceBuilder;
import org.elasticsearch.search.fetch.subphase.FetchSourceContext;

import java.io.IOException;
import java.util.HashMap;
import java.util.Map;
import java.util.concurrent.TimeUnit;

/**
 * ES高级客户端
 *
 * @author: 陌溪
 * @create: 2020-09-23-16:56
 */
public class ESHightApi {
    private RestHighLevelClient client;

    public void init() {
        RestClientBuilder restClientBuilder = RestClient.builder(
                new HttpHost("202.193.56.222", 9200, "http"));
        this.client = new RestHighLevelClient(restClientBuilder);
    }

    public void after() throws Exception {
        this.client.close();
    }

    /**
     * 新增文档，同步操作
     *
     * @throws Exception
     */
    public void testCreate() throws Exception {
        Map<String, Object> data = new HashMap<>();
        data.put("id", "2002");
        data.put("title", "南京西路 拎包入住 一室一厅");
        data.put("price", "4500");
        IndexRequest indexRequest = new IndexRequest("haoke", "house")
                .source(data);
        IndexResponse indexResponse = this.client.index(indexRequest,
                RequestOptions.DEFAULT);
        System.out.println("id->" + indexResponse.getId());
        System.out.println("index->" + indexResponse.getIndex());
        System.out.println("type->" + indexResponse.getType());
        System.out.println("version->" + indexResponse.getVersion());
        System.out.println("result->" + indexResponse.getResult());
        System.out.println("shardInfo->" + indexResponse.getShardInfo());
    }

    /**
     * 异步创建文档
     * @throws Exception
     */
    public void testCreateAsync() throws Exception {
        Map<String, Object> data = new HashMap<>();
        data.put("id", "2003");
        data.put("title", "南京东路 最新房源 二室一厅");
        data.put("price", "5500");
        IndexRequest indexRequest = new IndexRequest("haoke", "house")
                .source(data);
        this.client.indexAsync(indexRequest, RequestOptions.DEFAULT, new
                ActionListener<IndexResponse>() {
                    @Override
                    public void onResponse(IndexResponse indexResponse) {
                        System.out.println("id->" + indexResponse.getId());
                        System.out.println("index->" + indexResponse.getIndex());
                        System.out.println("type->" + indexResponse.getType());
                        System.out.println("version->" + indexResponse.getVersion());
                        System.out.println("result->" + indexResponse.getResult());
                        System.out.println("shardInfo->" + indexResponse.getShardInfo());
                    }
                    @Override
                    public void onFailure(Exception e) {
                        System.out.println(e);
                    }
                });
        System.out.println("ok");
        Thread.sleep(20000);
    }

    /**
     * 查询
     * @throws Exception
     */
    public void testQuery() throws Exception {
        GetRequest getRequest = new GetRequest("haoke", "house",
                "GkpdE2gBCKv8opxuOj12");
        // 指定返回的字段
        String[] includes = new String[]{"title", "id"};
        String[] excludes = Strings.EMPTY_ARRAY;
        FetchSourceContext fetchSourceContext =
                new FetchSourceContext(true, includes, excludes);
        getRequest.fetchSourceContext(fetchSourceContext);
        GetResponse response = this.client.get(getRequest, RequestOptions.DEFAULT);
        System.out.println("数据 -> " + response.getSource());
    }

    /**
     * 判断是否存在
     *
     * @throws Exception
     */
    public void testExists() throws Exception {
        GetRequest getRequest = new GetRequest("haoke", "house",
                "GkpdE2gBCKv8opxuOj12");
// 不返回的字段
        getRequest.fetchSourceContext(new FetchSourceContext(false));
        boolean exists = this.client.exists(getRequest, RequestOptions.DEFAULT);
        System.out.println("exists -> " + exists);
    }
    /**
     * 删除数据
     *
     * @throws Exception
     */
    public void testDelete() throws Exception {
        DeleteRequest deleteRequest = new DeleteRequest("haoke", "house",
                "GkpdE2gBCKv8opxuOj12");
        DeleteResponse response = this.client.delete(deleteRequest,
                RequestOptions.DEFAULT);
        System.out.println(response.status());// OK or NOT_FOUND
    }
    /**
     * 更新数据
     *
     * @throws Exception
     */
    public void testUpdate() throws Exception {
        UpdateRequest updateRequest = new UpdateRequest("haoke", "house",
                "G0pfE2gBCKv8opxuRz1y");
        Map<String, Object> data = new HashMap<>();
        data.put("title", "张江高科2");
        data.put("price", "5000");
        updateRequest.doc(data);
        UpdateResponse response = this.client.update(updateRequest,
                RequestOptions.DEFAULT);
        System.out.println("version -> " + response.getVersion());
    }
    /**
     * 测试搜索
     *
     * @throws Exception
     */
    public void testSearch() throws Exception {
        SearchRequest searchRequest = new SearchRequest("haoke");
        searchRequest.types("house");
        SearchSourceBuilder sourceBuilder = new SearchSourceBuilder();
        sourceBuilder.query(QueryBuilders.matchQuery("title", "拎包入住"));
        sourceBuilder.from(0);
        sourceBuilder.size(5);
        sourceBuilder.timeout(new TimeValue(60, TimeUnit.SECONDS));
        searchRequest.source(sourceBuilder);
        SearchResponse search = this.client.search(searchRequest,
                RequestOptions.DEFAULT);
        System.out.println("搜索到 " + search.getHits().totalHits + " 条数据.");
        SearchHits hits = search.getHits();
        for (SearchHit hit : hits) {
            System.out.println(hit.getSourceAsString());
        }
    }

    public static void main(String[] args) throws Exception {
        ESHightApi esHightApi = new ESHightApi();
        esHightApi.init();
        esHightApi.testCreate();
    }
}
```

# 第九章：ES 的基本原理

## 分布式架构原理

elasticsearch 设计的理念就是分布式搜索引擎，底层其实还是基于 lucene 的。核心思想就是在多台机器上启动多个 es 进程实例，组成了一个 es 集群。

es 中存储数据的基本单位是索引

es 集群多个节点，会自动选举一个节点为 master 节点，这个 master 节点其实就是干一些管理的工作的，比如维护索引元数据拉，负责切换 primary shard 和 replica shard 身份拉，之类的。

要是 master 节点宕机了，那么会重新选举一个节点为 master 节点。

如果是非 master 节点宕机了，那么会由 master 节点，让那个宕机节点上的 primary shard 的身份转移到其他机器上的 replica shard。急着你要是修复了那个宕机机器，重启了之后，master 节点会控制将缺失的 replica shard 分配过去，同步后续修改的数据之类的，让集群恢复正常。

## ES 写数据过程

- 客户端选择一个 node 发送请求过去，这个 node 就是 `coordinating node`（协调节点）。

- `coordinating node` 对 document 进行**路由** ，将请求转发给对应的 node（有 primary shard）。

- 实际的 node 上的 `primary shard` 处理请求，然后将数据同步到 `replica node`。

- ```
  coordinating node
  ```

  如果发现

  ```
  primary node
  ```

  和所有

  ```
  replica node
  ```

  都搞定之后，就返回响应结果给客户端。

## es 读数据过程

查询，GET 某一条数据，写入了某个 document，这个 document 会自动给你分配一个全局唯一的 id，可以通过 `doc id` 来查询，会根据 `doc id` 进行 hash，判断出来当时把 `doc id` 分配到了哪个 shard 上面去，从那个 shard 去查询。

- 客户端发送请求到**任意** 一个 node，成为 `coordinate node`。
- `coordinate node` 对 `doc id` 进行哈希路由，将请求转发到对应的 node，此时会使用 `round-robin` **随机轮询算法** ，在 `primary shard` 以及其所有 replica 中随机选择一个，让读请求负载均衡。

当分片所在的节点接收到来自协调节点的请求后，会将请求写入到 Memory Buffer，然后定时（默认是每隔 1 秒）写入到 Filesystem Cache，这个从 Momery Buffer 到 Filesystem 　　 Cache 的过程就叫做 refresh；

当然在某些情况下，存在 Momery Buffer 和 Filesystem Cache 的数据可能会丢失，ES 是通过 translog 的机制来保证数据的可靠性的。其实现机制是接收到请求后，同时也会写入到 translog 中，当 Filesystem cache 中的数据写入到磁盘中时，才会清除掉，这个过程叫做 flush；
　　在 flush 过程中，内存中的缓冲将被清除，内容被写入一个新段，段的 fsync 将创建一个新的提交点，并将内容刷新到磁盘，旧的 translog 将被删除并开始一个新的 translog。

- 接收请求的 node 返回 document 给 `coordinate node`。
- `coordinate node` 返回 document 给客户端。

## es 搜索数据过程

es 最强大的是做全文检索，就是比如你有三条数据：

```
java真好玩儿啊
java好难学啊
j2ee特别牛
```

你根据 `java` 关键词来搜索，将包含 `java`的 `document` 给搜索出来。es 就会给你返回：java 真好玩儿啊，java 好难学啊

搜索分成两个阶段：Query（搜索） 和 Fetch（取回）

- 客户端发送请求到一个 `coordinate node`。
- 协调节点将搜索请求转发到**所有** 的 shard 分片 对应的 `primary shard`主分片 或 `replica shard`副分片，都可以。每个分片在本地执行搜索并构建一个匹配文档的大小为 from + size 的优先队列。PS：在搜索的时候是会查询 Filesystem Cache 的，但是有部分数据还在 Memory Buffer，所以搜索是近实时的。
- query phase：每个 shard 分片 将自己优先队列中搜索结果（所有文档的 `doc id`）返回给协调节点，由协调节点进行数据的合并到自己的优先队列、排序、分页等操作，产出最终结果。
- fetch phase：接着由协调节点根据 `doc id` 去各个节点上**拉取实际** 的 `document` 数据，最终返回给客户端。

## 更新删除文档

删除和更新也都是写操作，但是 Elasticsearch 中的文档是不可变的，因此不能被删除或者改动以展示其变更；
　　磁盘上的每个段都有一个相应的.del 文件。当删除请求发送后，文档并没有真的被删除，而是在.del 文件中被标记为删除。该文档依然能匹配查询，但是会在结果中被过滤掉。当段合并时，在.del 文件中被标记为删除的文档将不会被写入新段。
　　在新的文档被创建时，Elasticsearch 会为该文档指定一个版本号，当执行更新时，旧版本的文档在.del 文件中被标记为删除，新版本的文档被索引到一个新段。旧版本的文档依然能匹配查询，但是会在结果中被过滤掉。

## 保证读写一致

通过版本号使用乐观锁。

对于写操作，一致性级别支持 quorum/one/all，默认为 quorum，即只有当大多数分片可用时才允许写操作。但即使大多数可用，也可能存在因为网络等原因导致写入副本失败，这样该副本被认为故障，分片将会在一个不同的节点上重建。

对于读操作，可以设置 replication 为 sync(默认)，这使得操作在主分片和副本分片都完成后才会返回；如果设置 replication 为 async 时，也可以通过设置搜索请求参数\_preference 为 primary 来查询主分片，确保文档是最新版本。

## 倒排索引

在搜索引擎中，每个文档都有一个对应的文档 ID，文档内容被表示为一系列关键词的集合。例如，文档 1 经过分词，提取了 20 个关键词，每个关键词都会记录它在文档中出现的次数和出现位置。

那么，倒排索引就是**关键词到文档** ID 的映射，每个关键词都对应着一系列的文件，这些文件中都出现了关键词。

有以下文档：

| DocId | Doc                                            |
| :---- | :--------------------------------------------- |
| 1     | 谷歌地图之父跳槽 Facebook                      |
| 2     | 谷歌地图之父加盟 Facebook                      |
| 3     | 谷歌地图创始人拉斯离开谷歌加盟 Facebook        |
| 4     | 谷歌地图之父跳槽 Facebook 与 Wave 项目取消有关 |
| 5     | 谷歌地图之父拉斯加盟社交网站 Facebook          |

对文档进行分词之后，去重，排序，得到以下**倒排索引** 。

| WordId | Word     | DocIds    |
| :----- | :------- | :-------- |
| 1      | 谷歌     | 1,2,3,4,5 |
| 2      | 地图     | 1,2,3,4,5 |
| 3      | 之父     | 1,2,4,5   |
| 4      | 跳槽     | 1,4       |
| 5      | Facebook | 1,2,3,4,5 |
| 6      | 加盟     | 2,3,5     |
| 7      | 创始人   | 3         |
| 8      | 拉斯     | 3,5       |
| 9      | 离开     | 3         |
| 10     | 与       | 4         |
| ..     | ..       | ..        |

另外，实用的倒排索引还可以记录更多的信息，比如文档频率信息，表示在文档集合中有多少个文档包含某个单词。

那么，有了倒排索引，搜索引擎可以很方便地响应用户的查询。比如用户输入查询 `Facebook`，搜索系统查找倒排索引，从中读出包含这个单词的文档，这些文档就是提供给用户的搜索结果。

## 底层 lucene

简单来说，lucene 就是一个 jar 包，里面包含了封装好的各种建立倒排索引的算法代码。我们用 Java 开发的时候，引入 lucene jar，然后基于 lucene 的 api 去开发就可以了。

通过 lucene，我们可以将已有的数据建立索引，lucene 会在本地磁盘上面，给我们组织索引的数据结构

# Kibana
