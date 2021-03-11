summary: Audio retrieval system with Milvus
id: en-how-to-setup-audio-search-with-milvus
categories: milvus
tags: demo
status: Published
authors: Shiyu Chen
Feedback Link: https://milvus.io

# Audio retrieval system with Milvus

## Overview

Duration: 1

This tutorial will show you how to use [PANNs](https://github.com/qiuqiangkong/audioset_tagging_cnn) and [Milvus](https://milvus.io)  to search the similarity audio items.

## Local Deployment

Duration: 10

### Requirements

- [Milvus 0.10.5](https://milvus.io/docs/v0.10.5/milvus_docker-cpu.md) (please note the Milvus version)
- [MySQL](https://hub.docker.com/r/mysql/mysql-server)
- [Python3](https://www.python.org/downloads/)

### Run Server

1. **Install python requirements**

   ```
   $ git clone https://github.com/zilliz-bootcamp/audio_search.git
   $ cd bootcamp/solutions/audio_search/webserver/
   $ pip install -r audio_requirements.txt
   ```

2. **Modify configuration parameters**

   Before running the script, please modify the parameters in **webserver/audio/common/config.py**:

   | Parameter    | Description               | Default setting |
   | ------------ | ------------------------- | --------------- |
   | MILVUS_HOST  | milvus service ip address | 127.0.0.1       |
   | MILVUS_PORT  | milvus service port       | 19530           |
   | MYSQL_HOST   | mysql service ip          | 127.0.0.1       |
   | MYSQL_PORT   | mysql service port        | 3306            |
   | MYSQL_USER   | mysql user name           | root            |
   | MYSQL_PWD    | mysql password            | 123456          |
   | MYSQL_DB     | mysql datebase name       | mysql           |
   | MILVUS_TABLE | default table name        | milvus_audio    |

3. **Star server**

   ```
   $ cd webserver
   $ python main.py
   ```

## System Usage

Duration: 5

You can type `127.0.0.1:8002/docs` into your browser to see all the APIs.

![img](./pic/all_API.png)

- Insert data.

  You can download the sample [game_sound.zip](https://github.com/shiyu22/bootcamp/blob/0.11.0/solutions/audio_search/data/game_sound.zip?raw=true) and insert it into the system.

  > The sound data in the zip archive is required to be in wav format.

  ![img](./pic/insert.png)

- Search audio.

  You can pass in [test.wav](https://github.com/shiyu22/bootcamp/blob/0.11.0/solutions/audio_search/data/test.wav) to find the result that is most similar to that sound.

  ![img](./pic/search.png)

> If you need a front-end interface, please refer to https://zilliz.com/demos/.