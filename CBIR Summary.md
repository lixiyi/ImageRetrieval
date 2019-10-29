# CBIR 综述

## 最常引述的方法
* 表示方法（局部特征、全局特征）
  * 手工特征：SIFT、GIST
  * 深度学习特征：RMAC、GEM
* 存储方法（稀疏化）
  * 倒排索引
    * 视觉字典、短语：Bow、VLAD
    * K-Means、ANN、K-D Tree、PQ
  * Hash
    * LSH

## 各类别的优点、缺点、创意、诉求

## CBIR中的重要问题、重要的优点和特性

## CBIR应用领域

* 



# CBIR 项目调研

* [`CBIR Engine List`](https://en.wikipedia.org/wiki/List_of_CBIR_engines) 

## [LIRE](http://www.lire-project.net/)

* Java Lib 基于颜色和纹理特征进行图像检索
* 用全局和局部的特征生成Lucene的索引

## [TinEye](https://tineye.com/)

* matchEngine: 利用像素，而`非图片内容`生成和比较图像的数字指纹
* 用于发现重复的图片，无法发现同一物体不同角度的图

## [Visenze](https://www.visenze.com/)
* 购物推荐：款式类似的服饰
* 深度学习图片识别

## [SnapFashion](https://www.snapfashion.com)
* 购物：相似的颜色和形状