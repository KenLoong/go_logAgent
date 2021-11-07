## 项目流程

**log_agent_demo流程**

1.初始化kafka,连接kafka服务器，作为kafka producer

2.初始化etcd，连接etcd服务器

2.1从etcd中获取配置信息（通过key来获取配置信息，etcd是一个k-v服务器）

3.初始化taillog，为每一个配置信息都创建一个日志任务（创建后就会开启新协程去监听文件，一行一行地往kafka的通道里发送日志信息），用tailLogMgr去管理这些任务，run方法就是监听通道，等待新配置

4.etcd监听key信息，使用watch功能去监听配置信息的key，一旦key对应的value有变化，就往taillog的通道发送新配置



**log_transfer_demo流程**

1.加载配置文件

2.初始化es(连接es)

3.初始化etcd(连接etcd，获取要从kafka监听的topic)

4.初始化kafka（连接kafka,从kafka获取数据，然后发往es）



## kafka

基于sarama第三方库开发的kafka client
启动kafka:（需要以管理员权限开启命令窗口）
cd E:\dev_apps\kafka_2.12-2.3.0

先启动zookeeper

bin\windows\zookeeper-server-start.bat config\zookeeper.properties

再启动ES

bin\windows\kafka-server-start.bat config\server.properties

终端消费kafak:
bin\windows\kafka-console-consumer.bat --bootstrap-server=127.0.0.1:9092 --topic=redis_log --from-beginning


## kibana
修改config下的配置文件kibana.yml，修改两处地方

- elasticsearch.url: "http://localhost:9200"
- i18n.defaultLocale: "zh-CN"

启动文件：kibana.bat

就可在localhost中用UI去检测ES的数据

## etcd
下载etcd后直接运行exe文件即可

## elasticSearch
运行bat文件即可