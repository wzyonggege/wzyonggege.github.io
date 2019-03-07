---
layout:     post
title:      "基于RNN自动生成古诗"
subtitle:   "char-rnn是一个多层的RNN网络，本次基于经过Tensorflow改写的sherjilozair/char-rnn-tensorflow，输入大量的古诗，让机器学会自己创作。"
date:       2018-02-13 12:00:00
author:     "Jam"
header-img: "img/about-bg.jpg"
tags:
    - Code
    - Python
    - RNN
    - Tensorflow
---

## 0. char-rnn

关于RNN，LSTM，GRU的介绍已经有很多了，大家可以搜索相关的深度学习主题文章先去了解。

而char-rnn是一个多层的RNN网络，本次基于经过Tensorflow改写的sherjilozair/char-rnn-tensorflow，输入大量的古诗，让机器学会自己创作。

## 1. 使用

<p>
    代码：wzyonggege/RNN_poetry_generator | 
    <iframe
        style="margin-left: 2px; margin-bottom:-5px;"
        frameborder="0" scrolling="0" width="100px" height="20px"
        src="https://ghbtns.com/github-btn.html?user=wzyonggege&repo=RNN_poetry_generator&type=star&count=true" >
    </iframe>
</p>

#### 环境
- Python 3.6
- Tensorflow 1.2.0

#### 使用

<pre>
帮助
python poetry_gen.py --help
命令行显示：

usage: poetry_gen.py [-h] [--mode MODE] [--head HEAD]

optional arguments:
  -h, --help   show this help message and exit
  --mode MODE  usage: train or sample, sample is default
  --head HEAD  生成藏头诗
</pre>

- 训练样本数据
<pre>python poetry_gen.py --mode train</pre>

- 生成古诗

上面的训练可能会花点时间，当然你也可以减少数据量去训练（GPU可以无视）。

训练完成之后：

<pre>python poetry_gen.py --mode sample</pre>

即可生成古诗：

（可以选择选取多少个高频的汉字，若模型生成的不在选取的字典中，用‘*’代替）

> 南晓弦门络丹墀，晚来兰槛酒盘弯。
> 故人无岁江水长，两泪任身泪满缨。
>（先不管内容了～～）

生成藏头诗

<pre>python poetry_gen.py --mode sample --head 明月别枝惊鹊</pre>

> 明年襟宠任，月出画床帘。别有平州伯性悔，枝边折得李桑迷。惊腰每异年三杰，鹊出交钟玉笛频。

再来一首

> 明排东西落，月浣绮罗纷。别月鲜方淡，枝枝胜鸟争。惊传元羽节，鹊堞吹桑衫。


## 2. 最后

<p>
    代码代码：wzyonggege/RNN_poetry_generator | 
    <iframe
        style="margin-left: 2px; margin-bottom:-5px;"
        frameborder="0" scrolling="0" width="100px" height="20px"
        src="https://ghbtns.com/github-btn.html?user=wzyonggege&repo=RNN_poetry_generator&type=star&count=true" >
    </iframe>
</p>