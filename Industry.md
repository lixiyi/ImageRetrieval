<!-- TOC -->

- [工业界CBIR论文](#工业界cbir论文)
- [京东](#京东)
        - [挑战](#挑战)
        - [架构设计](#架构设计)
            - [Indexing](#indexing)
            - [Search](#search)
        - [评价指标](#评价指标)
        - [其他](#其他)
- [Ebay](#ebay)
        - [摘要](#摘要)
- [参考](#参考)

<!-- /TOC -->

# 工业界CBIR论文

* 关键检索词： visual search
* 其他系统：
    * Google Similar Images
    * Amazon Flows for Internet search
    * Pinterest Lens
    * Google Lens

# 京东

### 挑战

* 图片体量大且在不断增长（JD有100 billion的图像数据）
* 数据更新快，需要保证`实时性`和`一致性`（JD每天都有1 billion左右的更新图片）

### 架构设计

#### Indexing

* 定时全量Index ：完整性
* 实时增量Index ：时效性（用户体验）
    * 收到产品更新事件时立即触发：add, del, modify
    * `reuse之前抽取过的信息` ：商品分时节下架、商家，抽取图片特征最耗时间，bitmap标记当前商品、图片是否有效

#### Search

* 分布式层次结构，3 level适应大规模数据，每个均有多个实例（并行、容错、负载均衡）
    * Blender ：（1）接收request，通知所有 Broker ;  （5）排序
    * Broker ：（2）通知自己管辖的那部分 Searcher ; （4）合并管辖 Searcher 的 Top K返回给Blender
    * Searcher : （3）检索相似图片，返回 Top K 给 Broker

### 评价指标
* 吞吐量（QPS）
* 查询响应时间（Response Time）

### 其他

* `分布式倒排索引为何是竖向分割而不是横向分割`
    * `竖向分割`：一个结点包含了所有的key,但每个key对应的posting list是按hash的结果分节点存储的
    * 横向分割：一个节点只存了部分key,但每个key的posting list是完整的
    * posting list要比key的数量大得多，横向分割查找起来很慢，网络通信代价也很大


# Ebay

### 摘要

* 有监督的方法：分类预测 + 稠密二进制语义Hash（无损精度），`直接在预测出的分类中检索`
* Aspect-Based 重排序强制语义相似
* 在Ebay应用于 : ShopBot 和 Close5



# 参考
[1] 京东: [The Design and Implementation of a Real Time Visual Search System on JD E-commerce Platform](https://arxiv.org/pdf/1908.07389.pdf)
[2] 京东 vearch Github [https://github.com/vearch/vearch](https://github.com/vearch/vearch)
[3] Ebay: [Visual Search at eBay](https://arxiv.org/pdf/1706.03154.pdf)