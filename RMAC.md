# MAC

# 简介

* 卷积层的每个`通道`最大激活值组成的向量
* $ \chi_i $ 代表每个通道的2D特征图
* MAC得到的向量为 $ f_{\Omega} = [f_{\Omega,1}, ..., f_{\Omega, i}, f_{\Omega, K}]^T $, 其中$ f_{\Omega,i} = max_{p \in \Omega} \chi_i(p) $，即每个通道的最大值
* 向量长度为通道卷积层特征的数量

# RMAC

## 简介

* 卷积层的每个`区域`最大激活值
* RMAC得到的向量为 $ f_{R} = [f_{R,1}, ..., f_{R, i}, f_{R, K}]^T $, 其中$ f_{R,i} = max_{p \in R} \chi_i(p) $，即每个区域的最大值
* 向量长度为通道卷积层特征的数量

## 实例流程

* 读取img，Resize成1024 * 1024
* 抽取Region List
	* 在`32 * 32`的特征图上抽取L种不同大小的Region
	* 相邻的Region重叠度为0.4
	* 宽高比会影响每种Scale Region的数量 $ l * (l + m-1)$
	*  每种Region的大小为$ 2 * min(W, H) / (l + 1) $
	* Region List: $[[x_i, y_i, w_i, h_i], ...]$, 每个Region用4个值表示
* VGG16抽取最后一个卷积层特征[1, 512, 32, 32]
* ```nums_rois = len(Region List)``` , 利用`RoiPooling`将每个Region转化为定长向量
	* 把每个Region都切分成数量相同的小块，每个小块大小不同
	* 取每个小块的最大值拼起来
	* [1, nums_rois, fix_len]
* L2-Norm	: [1, nums_rois, fix_len]
* Region PCA
	* 每个roi向量用PCA降成512维$z_i = W^T x_i$
	* `PCA的权重要预训练`
	* [1, nums_rois, 512]
* L2-Norm: [1, nums_rois, 512]
* Sum: 所有Region的向量求和[1, 1, 512]
* L2-Norm: [1, 1, 512]
	* 512 即是VGG16特征图的通道数

# References

* [RMAC Paper](https://arxiv.org/abs/1511.05879)
* [Github: keras rmac](https://github.com/noagarcia/keras_rmac)




