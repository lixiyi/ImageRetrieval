# 2003-2016 CBIR论文分类、评估及研究方向

## 简介

* 传统的CBIR只对图像的元信息建立索引
* 两大挑战：
	* 意图鸿沟（Intention Gap）：用户的意图 --- 表达出的Query 之间的差异
	* 语义鸿沟（Semantic Gap）：高层语义 --- 低层视觉特征 之间的差异
* CBIR的两个重要先驱方法
	* [SIFT](https://www.cs.ubc.ca/~lowe/papers/ijcv04.pdf)
		* 拥有局部不变性的视觉特征：旋转、缩放、光线
	* [BoW](http://www.robots.ox.ac.uk/~vgg/publications/papers/sivic03.pdf)
		* 通过量化局部特征得到图片的稠密表示，能套用倒排索引
* CBIR的三个重要问题，所有的研究都在对这三个问题作出贡献
	* 图像表示
	  * 区分相似与不相似的图片（discriminative）
	  * 充分表示图片内容（descriptive）
	  * 拥有局部不变性
	* 图像存储
	  * 倒排索引
	  * Hash技术
	  * 方法：视觉字典、特征量化、空间上下文编码, etc.
	* 图像相似度衡量
	  * 可看作不同的kernel的匹配方法

## PipeLine

![RA_stage](./img/RA_stage.png)

* 五大模块：
  * [查询形式](##查询形式)
  * [图片表示](##图片表示)
  * [索引](##索引)
  * [相关度打分](##相关度打分)
  * [重排序](##重排序)
* 图像表示模块线上和线下通用

## 查询形式

* 文本：关键词
* 示例图片：最直观
* 手画图：最能比表达用户语义，用户难画，图片都得映射成手画图
* 颜色分布图：相近的颜色，不易处理光线变化
* 文本分布图：场景检索
* 目前都是单图检索，还可以有多图联合检索（视频）

## 图片表示

### 特征抽取

* 手工特征
  * 颜色、形状、纹理、结构
  * [GIST](https://ieeexplore.ieee.org/stamp/stamp.jsp?tp=&arnumber=4042704)
    * `广泛用于ANN算法中`，是一种`全局特征`
    * 但不适用于复杂背景
  * `局部特征`如SIFT
    * 关键点检测
      * 各种检测器：DoG, MSER, Hessian affine, Harris Hessian, FAST
      * 不用检测器，稠密或均匀采样
    * 局部区域解释器
    * SIFT有各式各样的变种
      * SURF, root-SIFT, binary-SIFT etc.
* 学习特征
  * 预测图片的属性特征得分
    * 属性词表、图片的风格
    * 优点：能表达图片的语义
    * 缺点：词表很难设计完整、上千种属性的分类计算量也很大
  * 用主题模型pLSA和LDA获得语义编码
  * 用`深度学习`的方法获得语义编码
    * 全局特征：AlexNet, VGGNet, etc
    * 局部特征
      * 无监督的区域抽取：[selective search](http://huppelen.nl/publications/selectiveSearchDraft.pdf)
      * 在每个区域里抽取特征
        * CNN抽取物体检测Region的特征（图片4方向旋转，取最高分)
        * RMAC([用了RPN网络](https://arxiv.org/abs/1604.01325))
    * 上述的特征全都是从`分类任务`中抽取的，得到的特征不能完整地表示图像，最好能直接从`检索任务`中学习，`为了摆脱分类任务`:
      * [LandMark检索](https://arxiv.org/pdf/1404.1777.pdf)：只专注于地标的检索，分类直接设置为地标（即检索的对象）
      * [无监督patch-level卷积网络特征](https://hal.inria.fr/hal-01207966/file/deep_patches.pdf)
      * [二进制编码](https://pdfs.semanticscholar.org/a71d/d6e9a0b1c2b3aee7f8d39446051b6638fa22.pdf)：分解训练图像的相似矩阵，端到端的方式
      * [三元损失，更短的二进制编码](https://arxiv.org/abs/1504.03410)

### 视觉字典学习

### 空间上下文编码

### 特征量化

### 特征聚合

## 索引

## 相关度打分

## 重排序



























