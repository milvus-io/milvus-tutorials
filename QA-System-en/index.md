summary: Milvus High Availability (HA) Solution
id: en-how-to-do-reverse-QA-System-with-milvus
categories: milvus
tags: demo
status: Published
authors: Brother Long
Feedback Link: https://milvus.io

# Intelligent question answering system based on Milvus and BERT

## Overview

Duration: 1

This project combines the model provided by Milvus and Bert to realize a Chinese question and answer system.This paper aims to provide a solution to achieve semantic similarity matching with Milvus combined with various AI models.

Project source code reference: https://github.com/milvus-io/bootcamp/tree/1.0/EN_solutions/QA_System

## Environment

Duration: 2

The following tables show recommended configurations for the QA-System:

| Component | Recommended Configuration                     |
| --------- | --------------------------------------------- |
| CPU       | Intel CPU Sandy Bridge or higher.             |
| Memory    | 8GB                                           |
| OS        | Ubuntu 18.04 or higher / Centos 7.5 or higher |
| Software  | Milvus 1.0<br />Docker 19.03 or higher        |

## Data description

Duration: 3

The question-and-answer data set needed for this project includes two texts, a question set and a one-to-one answer set corresponding to the question set, which exist in the data directory.

The data set in the data directory is a sample data.

## config description

Duration: 4

QA/config.py：The script is a configuration file and needs to be modified for the specific environment.

| Parameter        | Description                  | Default setting |
| ---------------- | ---------------------------- | --------------- |
| MILVUS_HOST      | milvus service ip            | 127.0.0.1       |
| MILVUS_PORT      | milvus service port          | 19530           |
| PG_HOST          | postgresql service ip        | 127.0.0.1       |
| PG_PORT          | postgresql service port      | 5432            |
| PG_USER          | postgresql user name         | postgres        |
| PG_PASSWORD      | postgresql password          | postgres        |
| PG_DATABASE      | postgresql datebase name     | postgres        |
| DEFAULT_TABLE    | default table name           | milvus_qa       |
| BERT_HOST        | Bert service ip              | 127.0.0.1       |
| BERT_PORT        | Bert service port            | 5555            |
| collection_param | The parameters of collection |                 |
| search_param     | The parameters of search     | {'nprobe': 32}  |
| top_k            | The number of question       | 5               |

## Steps to build a project

Duration: 15

1.Install [Milvus 1.0.0](https://milvus.io/docs/v1.0.0/milvus_docker-cpu.md)

2.Install PostgreSQL

3.Install the Python packages you need

```
$ pip install -r requriment.txt
```

4.Start the Bert services (more [Bert](https://github.com/hanxiao/bert-as-service#building-a-qa-semantic-search-engine-in-3-minutes) related)

```shell
#Download model
$ cd model
$ wget https://storage.googleapis.com/bert_models/2018_11_03/english_L-12_H-768_A-12.zip
$ unzip english_L-12_H-768_A-12.zip
#start service
$ bert-serving-start -model_dir/tmp/english_L-12_H-768_A-12/ -num_worker =2 -max_seq_len=40
```

5. Start the query service

```shell
$ uvicorn main:app --host 127.0.0.1 --port 8000
```

> Enter 127.0.0.1:8000/docs in the web page to view the interface provided by this project.