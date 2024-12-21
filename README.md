# mysql 数据同步到 postgres 方案

## dbsyncer

开源项目地址： https://github.com/86dbs/dbsyncer

是一款开源的数据同步中间件，提供MySQL、Oracle、SqlServer、PostgreSQL、Elasticsearch(ES)、Kafka、File、SQL等同步场景

### 使用方式
通过网页配置mysql 和 postgre 的连接，再配置同步驱动，然后点击同步按钮即可

### 备注
需要每个表配置单独映射

### 缺点
1. 不能作为进程方式，后台进行同步，需要手动或API点击同步按钮，才触发一次数据同步

## debezium

开源项目文档地址： https://debezium.io/documentation/reference/3.0/tutorial.html

Debezium 是一个强大的开源 CDC 工具，可从 MySQL 捕获变更日志并将其同步到 PostgreSQL。

### 特点
* 实时性：基于 binlog 的变更捕获。
* 可靠性：通过 Kafka 分发数据，支持断点续传。
* 灵活性：支持将数据写入多个目标数据库（PostgreSQL、Elasticsearch 等）。

### 使用方式
需要为每个表创建connector生产者和消费者，并匹配topic

### 备注
* 依赖zookeeper，kafka，connect, 组件多
* 表结构变更demo未能成功

## pg_chameleon

pg_chameleon 是一个专门用于 MySQL 到 PostgreSQL 数据同步的开源工具。(python 实现)

官网文档地址： https://pgchameleon.org/documents/usage.html#command-line-reference

### 特点
* 支持实时同步。
* 支持 schema 转换和数据类型映射。
* 易于配置。

### 使用方式

* 通过配置文件配置 mysql 和 postgre 的连接，
* create_replica_schema: 初始化 pg_chameleon 的 schema；
* add_source: 添加一个源，并配置相应的 schema
* init_replica: 初始化 pg_chameleon 的 同步状态
* start_replica： 开始同步

### 备注
* 配置文件必须定义好表映射关系，如需要新同步表，需要在配置文件中添加，并重启同步命令
* 已配置完成的表，支持表结构变更
