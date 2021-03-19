summary: 基于 Milvus 和 BERT 实现智能问答系统
id: how-to-do-reverse-QA-System-with-milvus
categories: milvus-cn
tags: demo
status: Published
authors: Brother Long
Feedback Link: https://milvus.io

# 基于 Milvus 和 BERT 实现智能问答系统

## 概述

Duration: 1

本文展示如何利用 Milvus 和 BERT 提供的模型实现一个智能问答系统。旨在提供一个用 Milvus 结合各种 AI 模型实现语义相似度匹配的解决方案。

## 环境

Duration: 2

下表列出了本项目所需环境配置：

| 组件     | 推荐配置                               |
| -------- | -------------------------------------- |
| CPU      | Intel CPU Sandy Bridge 或以上          |
| Memory   | 8GB                                    |
| OS       | Ubuntu 18.04或以上 / CentOS 7.5 或以上 |
| Software | Milvus 1.0.0<br />Docker 19.03 或以上  |

## 数据说明

Duration: 3

本项目所需要问答数据集存在在data目录下，是一个包含了问题和答案的数据集。

data目录下的数据集是来自于十万个为什么的问答集，只有100条数据。

如果想要测试更多的数据集，请下载33万条[银行业务客服问答数据集](https://pan.baidu.com/s/1g-vMh05sDRv1EBZN6X7Qxw)，提取码：hkzn 。下载该数据后按照上面给出的格式来转化数据。

该银行业务相关的数据来源：https://github.com/Bennu-Li/ChineseNlpCorpus。本项目提供的数据取自ChineseNlpCorpus项目下问答系统中的金融数据集，从中提取了约33w对的问答集。

> 说明：您也可以使用其他的问答数据对进行测试。

## 项目结构说明

Duration: 4

data: 该目录中是本次数据的

requirement.txt: 该脚本中是需要安装的python包。

main.py: 该脚本是本项目的启动程序。

QA：该目录下是本项目中的关键代码。

QA下的脚本 config.py 是配置文件，需要根据具体环境做出相应修改。参数说明如下：

| 参数             | 说明                   | 默认设置       |
| ---------------- | ---------------------- | -------------- |
| MILVUS_HOST      | milvus服务所在ip       | 127.0.0.1      |
| MILVUS_PORT      | milvus服务的端口       | 19530          |
| PG_HOST          | postgresql服务所在ip   | 127.0.0.1      |
| PG_PORT          | postgresql服务的端口   | 5432           |
| PG_USER          | postgresql用户名       | postgres       |
| PG_PASSWORD      | postgresql密码         | postgres       |
| PG_DATABASE      | postgresql的数据库名称 | postgres       |
| BERT_HOST        | Bert服务所在的ip       | 127.0.0.1      |
| BERT_PORT        | Bert服务所在的端口     | 5555           |
| DEFAULT_TABLE    | 默认表名               | milvus_qa      |
| collection_param | 创建集合的参数         |                |
| search_param     | 查询时的参数           | {'nprobe': 32} |
| top_k            | 定义给出相似问题的数目 | 5              |

## 搭建步骤

Duration: 15

1. 安装[milvus1.0.0](https://milvus.io/cn/docs/v1.0.0/milvus_docker-cpu.md)

2. 安装postgresql

3. 安装所需要的python包

```shell
$ pip install -r requriment.txt
```

4. 启动bert服务(更多[bert](https://github.com/hanxiao/bert-as-service#building-a-qa-semantic-search-engine-in-3-minutes)相关)

```shell
# 下载模型
$ mkdir model
$ cd model
$ wget https://storage.googleapis.com/bert_models/2018_11_03/chinese_L-12_H-768_A-12.zip
$ unzip chinese_L-12_H-768_A-12.zip
# 启动服务
$ bert-serving-start -model_dir chinese_L-12_H-768_A-12/ -num_worker=2 -max_seq_len=40
```

5. 启动查询服务

```
uvicorn main:app --host 127.0.0.1 --port 8000
```

6. 打开 Fastapi 提供的前端接口

在网页中输入 127.0.0.1:8000/docs 查看本项目提供的接口。