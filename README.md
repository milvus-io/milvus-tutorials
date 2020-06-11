# milvus-tutorials

This repo are used to generate milvus tutorial websites. 

Each tutorial can be a markdown file. 

## recommend structure
```
-how-to-install-milvus
  - assets // pictures used in index.md
  - index.md // tutorial
```

## Meta information
Each markdown must contain meta information at the head of the document, so that the web system can recognize it as a tutorial
```
summary: 基于 Milvus 和 VGG 实现以图搜图
id: how-to-do-reverse-image-search
categories: milvus
tags: demo
status: Published
authors: Brother Long
Feedback Link: https://milvus.io
```
## Publish
The website will get update when the master branch changes, so please do not modify content directly on master branch, pull request is welcome.
