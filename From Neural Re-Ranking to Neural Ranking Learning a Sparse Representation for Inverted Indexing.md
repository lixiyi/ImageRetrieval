# 学习倒排索引的稀疏表示

## Abstract

* 目前的NRM都是多阶段排序
  * 只排序第一阶段返回的top k个文档
  * 生成的是`稠密向量`，需要和文档一一比对，不适用于整个数据集
* 采用第一阶段的排序器导致了两个问题
  * 两阶段交互和联合的影响不好说
  * 第一阶段相当于`gate-keeper`,导致第二阶段根本无法接触到最匹配的文档
* 本文采用`单阶段模型`，引入`稀疏`属性学习Document（D）和Query（Q）的表示
  * 包含了足够匹配Q和D的`语义信息`
  * `稀疏`到足以为整个数据集构建倒排索引
  * 采用了pseudo-relevance feedback伪相关度反馈（PRF）
## Introduction

* 多阶段排序缺陷
  * 阶段间存在引入和传递误差的问题
  * 生成的稠密向量不适合用倒排索引
* 本文的模型特点：为Q和D都生成了`高维`、`稀疏`的语义表示，便于`倒排索引`，是`单阶段`排序，效率和BM25、TF-IDF，QL模型一样高
* Q和D向量表示的`两个目标`
  * 引入稀疏属性
  * 捕获语义信息
* 流程概述
  * 先对Q和D的每个n-gram，先map到低维稠密空间（压缩信息），再map到高维空间得到稀表示
  * 希望学习到的高维稀疏表示
    * 稀疏性和输入文本的长度相关：`Q的稀疏性应该要比D更大`
    * `稀疏程度和词项的稀疏程度差不多`
  * 建立倒排索引
    * 每个维度作为一个词项
  * 查询时，先得到Q的高维稀疏表示，把非0位置的倒排列表取出合并，Q和D一一点积计算Score
  * 查询时用PRF扩展Q
  * 用弱监督方法训练：QL模型打弱标签

## [PPT](pdf/PPT-SNRM.pdf)

## Reference
[From Neural Re-Ranking to Neural Ranking Learning a Sparse Representation for Inverted Indexing](http://delivery.acm.org/10.1145/3280000/3271800/p497-zamani.pdf?ip=159.226.40.77&id=3271800&acc=ACTIVE%20SERVICE&key=33E289E220520BFB%2ED25FD1BB8C28ADF7%2E4D4702B0C3E38B35%2E4D4702B0C3E38B35&__acm__=1572256137_4578584be48ee316f2dafa9f17757c45)