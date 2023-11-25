[TOC]

# 1. 介绍

Kibana是用来管理和查询Elasticsearch的软件。

## 1.1. 安装

> https://www.elastic.co/downloads/kibana

1）压缩包安装

下载并解压压缩包，即可使用。

2）Docker安装

TODO

## 1.2. 运行

1）进入解压后的文件夹，即`$KIBANA_HOME`，在终端执行

```Bash
./bin/kibana # Linux or Mac
```

即可得到类似下面的输出结果：

![img](https://raw.githubusercontent.com/shengchaohua/my-images/main/images/202311251725061.png)

2）在浏览器打开http://127.0.0.1:5601，即可进入Kibana界面。

Kibana 默认从 `$KIBANA_HOME/config/kibana.yml`读取配置。

如果要使用Kibana访问远程的Elasticsearch服务器，可以复制一份配置文件，比如`kibana-remote.yml`，并修改对应的配置，然后在终端以指定配置执行Kibana，

```Bash
./bin/kibana --config=./config/kibana-remote.yml
```