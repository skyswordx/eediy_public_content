---
title: 生成式物理引擎 Genesis 配置踩坑记录
date: 2024-12-19
description: 暂时完结，感谢 github 的各位大佬
tags: 
    - 机器人
---

# 生成式物理引擎 Genesis 配置踩坑记录

## 参考链接

官方文档和仓库
-  [Genesis-Embodied-AI/Genesis](https://github.com/Genesis-Embodied-AI/Genesis)

个人配置时解决问题的过程参考
- [自己开的 issue：Win10-WSL2-PROBLEMS](https://github.com/Genesis-Embodied-AI/Genesis/issues/43)
- [Github issue: GPU 驱动和 mesa 版本的共同问题](https://github.com/microsoft/WSL/issues/9753)
-  [WSL2无痛将OpenGL升级成4.4以上版本（上述的 Github issue 总结](https://www.bilibili.com/opus/986947226512130056)
-  [Github issue: pyglet.gl.ContextException: Could not create GL context](https://github.com/pyglet/pyglet/issues/1047)


这是今天才刚刚发布的引擎，在配置的过程中确实搭大家都出现了好多 bug
在这里记录一下的目的主要也是祭奠从下午到晚上 debug 爬 issue 以及在我的电脑上运行各种奇奇怪怪的命令然后不知道拉了多少托💩的时间

## 环境配置

目前由于官方项目组使用的渲染 GUI 的组件是 OpenGL，**而 OpenGL 在 windows 平台本体本身没有配置**，**顶多在显卡的驱动上面可以找到相关的 dll 动态库**，所以除非等待官方后续的支持（有在 issue 中提到使用 matplot 库代替 OpenGL）

所以如果要在 windwos 平台跑这个项目
1. 找到显卡驱动自带的 OpenGL 动态库然后链接过去，我也不知道怎么做
2. 使用 WSL 环境
3. 使用双系统，但是因为我不喜欢老是重启电脑，所以这个选项不考虑

## 使用 WSL 配置遇到的问题

### SOLVED: pyglet.gl.ContextException: Could not create GL context

这个错误主要是由于**如果 WSL2 要使用 OpenGL 来调用 GPU 硬件进行渲染 GUI 窗口的话**，主机将 NVIDIA 驱动映射到 WSL2 的操作目前**就限制了 OpenGL 的版本只能为 3.1**

而 `pyglet` 这个库自从 v2.0 以后就需要 `OpenGL render string` 是 3.3+ 版本

而由于 OpenGL 的特性，使用硬件渲染和软件渲染的驱动器 `render string` 是不一样的，在 WSL 下如果使用软件渲染的话，OpenGL 的版本可以被提升

因此解决方案就是有两个思路
1. ❌ 不推荐，升级 WSL2的 OpenGL 版本到 3.3 然后使用软件渲染 OpenGL 的 GUI 窗口，并且在运行 py 脚本之前设置环境变量 `LIBGL_ALWAYS_SOFTWARE=1` 之后才 `python xxx.py`
2. ✔✔推荐，升级 WSL2 的 OpenGL 版本到 4.5 然后使用硬件渲染 OpenGL 的 GUI 窗口


无论怎么样都需要升级 WSL2 的 OpenGL 版本，可以先添加下面的 mesa 软件源以升级 OpenGL 版本

升级到 OpenGL 4.5 
```shell
sudo add-apt-repository ppa:kisak/kisak-mesa
sudo apt update && sudo apt upgrade -y
```

升级到 OpenGL 3.3
```shell
sudo add-apt-repository ppa:oibaf/graphics-drivers
sudo apt update && sudo apt upgrade -y
```
>但是这个源的 mesa 版本不高，我自己一套下来 OpenGL 版本并没有升级
>所以只能使用 mesa 的软件渲染进行 OpenGL 的 GUI 窗口渲染
>但是这样的话，虽然可以运行 pyglet 的 GUI 窗口，但是性能会很差，在渲染出场景前要等待很久

然后如果要使用硬件渲染的话，需要在 WSL2 `.bashrc` 的配置文件中添加下面的配置
```shell
# <<< OpenGL 4.5 使用配置 <<<
# 启动直接渲染
export LIBGL_ALWAYS_INDIRECT=0
# 使用主机 GPU 进行渲染（启用 d3d12 桥接）
export GALLIUM_DRIVER=d3d12
export MESA_D3D12_DEFAULT_ADAPTER_NAME=NVIDIA  # 如果使用的是 NVIDIA 显卡
# >>> OpenGL 4.5 使用配置 >>>
```


### SOLVED: OpenGL.error.Error: Attempt to retrieve context when no valid context


只要在每一个导入 `genesis` 的地方之前添加下面的代码就可以解决这个问题
```python
import os
os.environ['PYOPENGL_PLATFORM'] = 'glx'

import genesis
```
