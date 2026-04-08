---
title: WSL-Ubuntu-conda 环境中找不到 lib.so 动态库
description: debug 了一个早上和一个中午
tags: 
  - 机器人
---
# WSL-Ubuntu-conda 环境中找不到 `lib.so` 动态库


参考我在 issue 中的[回答](https://github.com/pytorch/pytorch/issues/104259#issuecomment-2513625520)

如果使用 conda 来管理 python 环境，在 `.bashrc` 文件中修改 `LD_LIBRARY_PATH` 、或者直接在终端中 `export LD_LIBRARY_PATH` 的任何改变并不会影响 conda 环境中的效果

所以为了在 conda 环境中真正地添加动态库路径，首先需要找到缺失的动态库（一般在 `/usr/lib/wsl/lib` 下面）或者使用
```shell
sudo find / -name 'libxxx.so'
```

然后激活 conda 环境，并且使用下面命令进行修改
```shell
conda env config vars set LD_LIBRARY_PATH=$LD_LIBRARY_PATH:<your/path/to/missing/lib>
```

然后再重新激活环境，就发现 conda 环境的 python 可以识别到缺失的动态库了

一般常见缺失的解决方法：
```shell
conda env config vars set LD_LIBRARY_PATH=$LD_LIBRARY_PATH:/usr/lib/wsl/lib/
```