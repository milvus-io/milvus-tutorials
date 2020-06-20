summary: 基于 Milvus 和 BERT 实现智能问答系统
id: how-to-do-reverse-QA-System-with-milvus
categories: milvus
tags: demo
status: Published
authors: Brother Long
Feedback Link: https://milvus.io

# 基于 Milvus 和 BERT 实现智能问答系统

## 概述

Duration: 1

本文展示如何利用 Milvus 和 BERT 提供的模型实现一个智能问答系统。旨在提供一个用 Milvus 结合各种 AI 模型实现语义相似度匹配的解决方案。

## 数据说明

Duration: 3

本项目所需要问答数据集包括两个文本，一个问题集，一个与问题集一一对应的答案集，存在本项目的 data 目录下。

data 目录下的数据集是来自于十万个为什么的问答集，有100条问答数据对。

如果想要测试更多的数据集，请下载33万条[银行业务客服问答数据集](https://pan.baidu.com/s/1g-vMh05sDRv1EBZN6X7Qxw)，提取码：hkzn 。下载该数据后解压放在 data 对应的目录下，若使用该数据集，在导入数据集时，需要指定该问答数据文件所在路径。该银行业务相关的数据来源：https://github.com/Bennu-Li/ChineseNlpCorpus。本项目提供的数据取自ChineseNlpCorpus项目下问答系统中的金融数据集，从中提取了约33w对的问答集。

> 说明：您也可以使用其他的问答数据对进行测试。

## 脚本说明

Duration: 4

在 github 上拉取本项目所需要的脚本。

```shell
git clone https://github.com/milvus-io/bootcamp.git
cd bootcamp/solutions/QA_System
```

本项目下存在一个文件夹 QA-search-server, 存放的是启动本服务所需要的脚本。

app.py: 该脚本为前端界面提供接口

main.py: 该脚本可以在终端直接执行数据导入及查询等操作。

参数说明：

| 参数       | 说明                                       |
| ---------- | ------------------------------------------ |
| --table    | 该参数在执行脚本时指定表名                 |
| --question | 该参数在执行脚本时指定问题数据集所在的路径 |
| --answer   | 该参数在执行脚本时指定答案数据集所在的路径 |
| --load     | 该参数执行数据导入操作                     |
| --sentence | 该参数给出查询时的问句                     |
| --search   | 该参数执行查询操作                         |

config.py：该脚本是配置文件，需要根据具体环境做出相应修改。

| 参数          | 说明                   | 默认设置  |
| ------------- | ---------------------- | --------- |
| MILVUS_HOST   | milvus服务所在ip       | 127.0.0.1 |
| MILVUS_PORT   | milvus服务的端口       | 19530     |
| PG_HOST       | postgresql服务所在ip   | 127.0.0.1 |
| PG_PORT       | postgresql服务的端口   | 5432      |
| PG_USER       | postgresql用户名       | postgres  |
| PG_PASSWORD   | postgresql密码         | postgres  |
| PG_DATABASE   | postgresql的数据库名称 | postgres  |
| DEFAULT_TABLE | 默认表名               | milvus_qa |

## 搭建步骤

Duration: 15

1. 安装[milvus](https://milvus.io/cn/docs/v0.10.0/guides/get_started/install_milvus/cpu_milvus_docker.md)

2. 安装postgresql

3. 安装所需要的python包

```shell
pip install --ignore-installed --upgrade tensorflow==1.10
pip install -r requriment.txt
```

4. 启动bert服务(更多[bert](https://github.com/hanxiao/bert-as-service#building-a-qa-semantic-search-engine-in-3-minutes)相关)

```shell
#下载模型
mkdir model
cd model
wget https://storage.googleapis.com/bert_models/2018_11_03/chinese_L-12_H-768_A-12.zip
#启动服务
bert-serving-start -model_dir chinese_L-12_H-768_A-12/ -num_worker=8 -max_seq_len=40
```

5. 导入数据

```shell
cd QA-search-server
python main.py --collection milvus_qa --question data/question.txt --answer data/answer.txt --load
```

> 注：data/question.txt 是导入的问题集所在的路径
>
> ​        data/answer.txt 是导入的答案集所在的路径

6. 启动查询服务

```shell
python app.py
```

7. 构建并启动查询客户端

```shell
# 拉取前端界面的镜像
docker pull zilliz/milvus-demo-chat-bot:latest
# 启动客户端
docker run --name milvus_qa -d --rm -p 8001:80 -e API_URL=http://127.0.0.1:5000 -e LAN=cn zilliz/milvus-demo-chat-bot:latest
```

> http://127.0.0.1 该处为步骤六启动的服务所在的机器 ip 。

8. 打开网页，输入网址：127.0.0.1:8001，即可体验属于您的智能问答系统。

> 127.0.0.1:8001是启动步骤七服务所在的机器ip
