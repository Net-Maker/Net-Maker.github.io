---
title: "2年来3D视觉科研总结&科研经验分享"
date: 2024-04-15T15:06:23+08:00
---
# Motivation
本文章用来总结本人在3D视觉研究中遇到的问题，同时给出一些经验，主要与Python，NeRF，3DGS相关。

本人才疏学浅，只希望大家能够在浏览文章后能够互相学习借鉴。

适用人员：想要入门科研的本科生、未接触过科研的研一、有着科研热情的所有新手！
# 写在前面
本文章没有对具体问题的具体解决方法，更多的是自己的一套方法论，希望可以帮助大家获得一定解决问题的通用能力。
# 配置环境常见问题
## 基本网络问题
一定要梯子！一定要梯子！一定要梯子！

梯子可以解决99%的pip install和conda install遇到的问题
当然，考虑到一些公司环境，这里还是提供一些github镜像和pip镜像（清华源）以及conda镜像（清华源）

清华源tuna是一个非常好的项目，偶尔会封禁一些下载流量过大的ip，这个时候可以发邮件向他们申请，回复及时且很友善。


- 清华pip源配置命令
```
参考https://mirrors.tuna.tsinghua.edu.cn/help/pypi/
```
临时使用：
```
pip install -i https://pypi.tuna.tsinghua.edu.cn/simple 你的包
```
默认使用（pip版本>=10.0.0）
```
python -m pip install --upgrade pip
pip config set global.index-url https://pypi.tuna.tsinghua.edu.cn/simple
```

- 清华conda源配置命令
```
参考https://mirrors.tuna.tsinghua.edu.cn/help/anaconda/
```
修改你的`/home/用户名/.condarc`文件，替换为
```
channels:
  - defaults
show_channel_urls: true
default_channels:
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/main
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/r
  - https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/msys2
custom_channels:
  conda-forge: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  msys2: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  bioconda: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  menpo: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  pytorch-lts: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  simpleitk: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud
  deepmodeling: https://mirrors.tuna.tsinghua.edu.cn/anaconda/cloud/
```
如果没有这个文件，则直接创建，无法创建的windows用户可使用`conda config --set show_channel_urls yes`

## Torch版本老是对不上或者没有CUDA

很多人都被这个问题折磨过，不过这里给出一个比较通用的解决方案
- 首先，明确你需要的torch版本，torchvision版本和你自己的cuda版本`nvcc -V`
- 接下来，有多种选择
  - 使用conda安装：
    ```
    conda install pytorch=XXX torchvision=XXX cudatoolkit=XXX -c pytorch`
    ```
  - 使用conda的好处是简单、高效，且可以自由选择cuda的版本，不过大多数人的网络可能不支持这么丝滑的下载
  - 使用pytorch官方网站的下载连接，手动下载正确版本：`pip install torch torchvision --index-url https://download.pytorch.org/whl/cu118`

## 个别较难安装的库，如pytorch3D
这里仅以pytorch3D举例，其实没有太多技巧，主要是进入到这个库的官方github，clone到本地，然后python setup.py install即可，非常方便

## 难以编译的库，如OPENPOSE
本人有一段在3090上编译openpose的痛苦经历，虽然openpose这个项目的贡献非常大，但是不得不说，这个库已经过时了，在CUDA11以上的版本非常难编译成功。
这里并不细化到如何编译openpose，而是给出一个通用的方法论：
- 准备好你需要的工具：
  - Google，Github的issues，阅读英文论坛的耐心
  - 绝大多数的问题都可以在google搜索后使用github的issue功能自行解决，这是很简单但是很多人无法完成的，不要一味地指望CSDN。
- 解决所有的warning：
  - 当你编译时碰到了莫名其妙的bug，多看看输出中是否有warning的出现吧！ 输出中会包含解决问题的信息的。
- 具体问题无法搜索到时，减少搜索的词数，从类似的问题中寻找问题的特性
  - 举个例子，当我编译openpose的时候，报错显示有一个lib并未被定义，由于搜不到任何结果，于是我搜索'cmake，xxx未被定义'，顺利地解决了这个问题。

## To be continue
有问题欢迎探讨！

# 科研常见问题
## 简述
这一节主要讲述我自己的科研经历以及碰到的各种问题，包括其解决方案。我个人是CV偏CG方向，3D视觉，仅供参考

## 信息搜集&工具汇总
在这里希望大家能够熟练地利用好已有的工具，不要知难而退，很多工具是能非常大地提升自己的效率的
至于工具推荐，我这里不作过多介绍，可以参考以下博客，也是程序员学习基础技能必备：
- 大名鼎鼎的CS自学指南：https://csdiy.wiki/
- 国内视觉青年学者论坛：VALSE：https://space.bilibili.com/562085182?spm_id_from=333.337.search-card.all.click
- 浙大CAD实验室入门手册&大牛科研经验分享：https://github.com/pengsida/learning_research
- Latex写作教程：https://github.com/guanyingc/latex_paper_writing_tips
- 各类github总结库：awesome-xxx，比如awesome-nerf，awesome-3dgs
- AI热度排行榜：https://paperswithcode.com/
- 文献调研神器：https://www.connectedpapers.com/
- 写作神器：https://www.grammarly.com/
- AI会议倒计时：https://aideadlin.es/
- 文献管理：https://readpaper.com/

上面提到的大多数教程都是github上的，这里也强烈推荐使用学好git，玩起github，能接触到更多适合你的资源。

还有更多优质的网站欢迎分享

## 代码改了没效果，idea不work？
相信大多数人刚接触科研的时候和我一样，想到了一个自我感觉良好的idea，然后开始投入代码之中，在失败后频繁地摆弄这一团已经改得乱七八糟的代码，在一遍遍的自我怀疑中丧失科研的信心。

或许有少数天赋异禀的同学能够避免这个过程，直接发表不错的作品，但是我想说，陷入这个问题的同学也不用气馁，很多人都是这样过来的。
这里，我给出自己的一些建议，能够帮助大家避免陷入这个自我怀疑的过程。
- ### 做好版本控制和实验数据的记录
  - 每天工作完，或是改完一版较为完成的代码，都要git commit一下， 未来的你会感谢每一个commit，特别是赶实验的时候。
- ### 做实验不要局限于一个样本
  - 可能是计算机专业（或是NeRF方向）的专属问题，很多人喜欢只看一个样本（scene）的指标，来断定自己的模型好不好，但是这是一个非常严重的错误。即使实验时间非常久，也要做好完成的实验，观察各个指标的变化，通过指标的含义去分析你的实验结果。
  - 拿NeRF的实验举例：你的模型可能没有对PSNR进行提升，但是可能对SSIM这个指标有很大提升，这也是完全有可能的。
- ### 选baseline的时候，要懂得知难而退
  - 如果你的领域突然出现了一个很牛逼的作品，而你准备在这个作品的基础上进行改进时，先评估它的代码复杂度、idea需要开发的时间、以及实验的时间和金钱成本。
  - 技术发展是非常快的，控制好时间成本也是科研的硬能力，如果这个作品的实现非常复杂，要么换一个idea，要么等其他开源的简单版本复现。（针对科研新手，大佬可以忽略）
  - 本人曾经便陷入过这个陷阱之中，在Instant-NGP的代码基础上开发一个idea，在代码的海洋中好不容易实现了功能，结果渲染部分无法对齐，debug？ 渲染部分上万行CUDA代码，编译一次需要十几分钟。 最后只能不了了之。
- ### idea不work还是你的代码不work？
  - 科研写代码可以写得很杂乱，但是细节部分是一定要仔细检查核对的，有时候并不是你的idea不work，而是在某个细节部分你和baseline不一样，从而导致了巨大的差异。
  - 例如：在三维重建任务中，使用softplus激活函数的方法的指标会比使用relu的高。 
  - 又比如：它的作品用了float64，而你的是float16，你的部分梯度直接消失了。
  - 这里同样凸显了版本控制的必要性！
- ### 实在做不出来，不如出去走走
  - 可能有不少人在科研路上不断和自己较劲，不达到自己的预期绝不停手。
  - 但是，科研的路还长，一时的得失并不能代表什么，没有成果产出的时候不如出去走走，运动，享受美食或是游戏。
  - 地球缺了这一篇文章并不会停止转动，计算机学科更不会缺我这一份工作，我个人更愿意将生活排在科研前面，享受生活也享受科研。
- ### To be continue
  - 后续的想起来再加，我要去洗澡了！


## 写作对我来说太难了！
没事，对我来说也一样难，那不也得写吗？不让你指望它自己写好？用好我提供的工具，基本的叙述应该是没问题的。

对我来说，写作的部分可以讲的经验不多，但是上面提供的资料中有大量的写作技巧和教程，大家共勉。

后续有更多tips，再分享在这。