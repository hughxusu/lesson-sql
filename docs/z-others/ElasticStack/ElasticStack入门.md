# ElasticStack入门

ElasticStack包括：

* Elasitcsearch数据存储查询分析
* Kibana数据探索与可视化分解
* Beats、Logstash数据收集与处理
  * Beats轻量级数据传送者
    * Filebeat日志文件
    * Metricbeat度量数据
    * Packetbeat网络数据
    * Winlogbeat Windows数据
    * Heartbeat健康检查
  * Logstash数据传送

## 安装与配置

```shell
docker pull elasticsearch:6.8.13 # 下载image
docker network create elk # 创建网络

# 启动容器
docker run -d --name elasticsearch --net elk -p 9200:9200 -p 9300:9300 -e "discovery.type=single-node" elasticsearch:6.8.13
docker run -d --name kibana --net elk -p 5601:5601 kibana:6.8.13
```

浏览器中打开`http://localhost:9200/`查看elasticsearch是否启动成功

浏览器中打开`http://localhost:5601/`查看kibana是否启动成功

### elasticsearch配置

#### 目录结构

```shell
.
├── bin													# 工具和脚本
├── config											# 配置文件
│   ├── elasticsearch.yml				# es相关配置
│   ├── jvm.options							# jvm相关参数
│   └── log4j2.properties				# 日志相关配置
├── data			 
├── lib													# 底层依赖的jar包
├── logs
├── modules											# 相关的功能模块
└── plugins											# plugins插件包
```

#### 配置文件(config文件夹)

##### jvm.options

```shell
-Xms1g # 1g内存
-Xmx1g
```

最小和最大内存，分配同样大小，系统的1/2~1/3。

##### elasticsearch.yml

```yaml
cluster.name: my-application # 集群名称，同一个网段自动加入
node.name: node-1 # 节点名称
http.port: 9200 # http端口
network.host : 0.0.0.0 # 将端口设置为0.0.0.0后，外网也能访问。

# 配置主节点
node.master : true 

# 配置从节点
http.port : 9201 # 同一台机器上端口应该不一样
discovery.zen.ping.unicast.hosts : ["127.0.0.1"] # 找到master节点

# 修改集群名称
cluster.name: my-application # elasticsearch.yml文件中
```

#### 插件安装

##### head安装

1. Github下载elasticsearch-head
2. 在elasticsearch-head目录下执行：

   1. `npm install`
   2. `npm run start`
3. 修改elasticsearch配置文件增加，允许第三方服务访问。

```yaml
http.cors.enabled: true
http.cors.allow-origin: "*"
# http.cors.allow-methods: OPTIONS, HEAD, GET, POST, PUT, DELETE
# http.cors.allow-headers: "X-Requested-With", Content-Type, Content-Length, X-User
```

4. 启动elasticsearch并登陆`http://localhost:9100`

##### ik安装

1. GitHub下载ik分词器released版本并解压（不是原项目）。
2. 在your-es-root/plugins/下创建ik文件夹，将解压文件考入
3. 测试分词器，控制台输入

```shell
curl -XGET -H 'Content-Type: application/json' 'http://localhost:9200/_analyze?pretty' -d '{ "analyzer" : "ik_max_word", "text": "中华人民共和国国歌" }'
```

### Kibana配置

#### 目录结构

```shell
.
├── bin													# 工具和脚本
├── config											# 配置文件
│   └── kibana.yml							# kibana相关配置
├── data			 
├── lib													# 底层依赖的jar包
├── logs
└── plugins											# plugins插件包
```

#### 配置文件(config文件夹)

##### kibana.yaml

```yaml
server.host: "0" # kibana访问端口
elasticsearch.hosts: [ "http://elasticsearch:9200" ] # elasticsearch地址
```

#### kibana常用功能

* Discover 数据搜索查看
* Visualize 图表制作
* Dasboard 仪表盘制作
* Timelion 时序数据的高级可视化分析
* DevTools 开发者工具
* Management 配置管理

