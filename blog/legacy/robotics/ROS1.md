---
title: ROS1 总结文档
description: 有待整理，化繁为简，断舍离
tags: 
  - 机器人
---

# 参考链接
-  [ROS1官方中文教程文档](https://wiki.ros.org/cn/ROS/Tutorials)
-  [autolabor教程](http://www.autolabor.com.cn/book/ROSTutorials/di-2-zhang-ros-jia-gou-she-ji/22hua-ti-tong-xin/214-hua-ti-tong-xin-zhi-zi-ding-yi-xiao-xi.html)
# 安装 ROS1 环境

跟着ROS的官方文档下载 ROS1 和 ROS2

安装问题记录
-  [rosdep init](https://zhuanlan.zhihu.com/p/77483614)
-  [ssl 等待超时](https://blog.csdn.net/yuteng12138/article/details/130103807)

记得安装完配置ROS的环境喔
```shell
source /opt/ros/<distro>/setup.bash
```


# ROS 的架构

ROS 只是元操作系统，需要依托真正意义的操作系统，目前兼容性最好的是 Linux 的 Ubuntu **所以需要一个对接 OS 的接口层**

其次还要有封装了**关于机器人开发的中间件作为基本的驱动库**。比如
- 基于 TCP/UDP 继续封装的 TCPROS/UDPROS 通信系统
- 用于进程间通信 Nodelet，为数据的实时性传输提供支持
- 数据类型定义、坐标变换、运动控制相关的库

最后就是提供给用户的应用接口层，具体来说就是各种软件功能包以及功能包内的节点，比如主节点、turtlesim 的控制与运动节点

在硬盘上 ROS 源代码的组织形式大致可以如下图所示：
![ROS1工作区结构.png](/img/robots/ROS1工作区结构.png)

```shell
. # 代表 catkin 工作区的文件目录
├── build # 存放 cmake 和 make 输出结果的地方
├── devel # development 存放有初始化空间的脚本/可执行文件/静态库动态库/头文件等等
└── src 
    ├── CMakeLists.txt # 最高级的 CMakeLists 由创建 catkin 工作区时的命令自动生成
    ├── package_1
    ├── package_2
    ├ ...
    └── package_n
        ├── CMakeLists.txt # 配置编译规则，比如源文件、依赖项、目标文件
        ├── package.xml # 包信息，比如:包名、版本、作者、依赖项
        ├── scripts # 存放 python 源文件
        ├── src # 存放 cpp 源文件
        ├── include # 存放 cpp 相关头文件
        ├── msg # 存放话题通信的消息类型格式文件
        ├── srv # 存放服务通信的格式文件
        ├── action # 存放动作格式文件
        ├── launch # 存放一次运行多个节点的启动 launch 文件
        └── config # 存放功能包的配置信息
```

软件包中的 `package.xml` 定义了有关软件包的属性，如软件包名称，版本号，作者，维护者以及对其他 `catkin` 软件包的依赖性，**具体参见软件包工作区的文件**

软件包内部的 `CMakeLists.txt` 是包含 `catkin` 特化的文件，主要多加了运行时依赖，还有编译时添加消息文件和服务文件等等的类型，**具体参见软件包工作区的文件**


# ROS 软件包

## 软件包的概念

**软件包(Packages)** 是ROS代码的软件组织单元。每个软件包都可以包含程序库、可执行文件、脚本或其他构件，一般程序代码散落在许多ROS软件包中

**清单(package.xml)** 是对软件包的描述，它用于定义软件包之间的依赖关系，并记录有关软件包的元信息，如版本、维护者、许可证等

在 ROS1 中，一个 `catkin` 软件包要想合规
1. 一定要有符合 `catkin` 规范的 `package.xml` 和`CMakeLists.txt`
2. 而且**每个包必须有自己的目录**，这意味着在同一个目录下不能有嵌套的或者多个软件包存在

一般而言，一个成熟的软件包的组织结构是
```shell
. # 代表软件包的文件目录
├── CMakeLists.txt # 软件包必须的
├── include
│   └── beginner_tutorials
├── launch # 用来启动节点的
│   ├── talker_listener.launch
│   └── turtlemimic.launch
├── msg # 软件包包含的消息文件目录
│   └── Person.msg
├── package.xml # 软件包必须的
├── src # 存放节点代码实现的
│   ├── listener.cpp
│   └── talker.cpp
└── srv # 软件包包含的服务文件的目录
    └── AddTwoInts.srv
```

## 用 catkin 工作区来创建软件包

虽然可以单独开发软件包，但是比较推荐创建并使用 `catkin` 工作区来开发和管理多个软件包

`catkin` 工作区的组织结构如下，可以管理多个软件包
```shell
. # 代表 catkin 工作区的文件目录
├── build # 存放 cmake 和 make 输出结果的地方
├── devel # development 存放有初始化空间的脚本/可执行文件/静态库动态库/头文件等等
└── src 
    ├── CMakeLists.txt # 最高级的 CMakeLists 由创建 catkin 工作区时的命令自动生成
    ├── package_1
    ├── package_2
    ├ ...
    └── package_n
```

### 创建 catkin 工作区

首先需要创建并 `source` 一个 `catkin` 工作区，下面是步骤
```shell
# 创建一个叫做catkin_ws的工作区
mkdir -p ~/catkin_ws/src
cd ~/catkin_ws/
catkin_make

# 配置source以便加载ROS环境变量，并可以使用ROS终端工具
source devel/setup.bash

# 查看ROS环境变量，检查一下是否包含工作区的路径
echo $ROS_PACKAGE_PATH
/home/<username>/catkin_ws/src:/opt/ros/<distro>/share
```

已经写好一个自动化脚本来生成工作区，这个脚本的工作区会调用 `catkin_make` 自动生成目录结构
```shell
. # 代表 catkin 工作区的文件目录
├── build # 存放 cmake 和 make 输出结果的地方
├── devel # development 存放有初始化空间的脚本/可执行文件/静态库动态库/头文件等等
└── src # 放软件包的地方
```

### 创建软件包

因为软件包都要放在工作区的 `src` 目录里面，所以要先切换到 `catkin` 工作区的 `src` 目录下

然后使用 `catkin_create_pkg` 创建软件包，并添加依赖的软件包（一般是 `std_msgs` `rospy` `roscpp`）

`catkin_create_pkg` 命令会要求你输入你要创建的软件包的名字，然后再输入依赖的软件包的名字。比如 ` catkin_create_pkg package_name std_msgs rospy roscpp`，最终创建一个和你新建软件包同名的文件夹，这份文件夹包含了最原始的软件包组成要素，一个 `package.xml` 和 `CMakeLists.txt`

在 `package.xml` 里面就存放了在使用 `catkin_create_pkg` 命令时的创建的软件包依赖关系
```xml
<package>
...
 <buildtool_depend>catkin</buildtool_depend>
 <build_depend>roscpp</build_depend>
 <build_depend>rospy</build_depend>
 <build_depend>std_msgs</build_depend>
...
</package>
```

在很多情况下，一个被依赖的包还会有它自己的依赖关系，比如，`rospy` 就有其它依赖包

可以使用 `rospack depends1` 查看软件包的第一级依赖
或者使用 `rospack depends` 递归查看所有嵌套的依赖包

命令的更多详细参数参考[catkin/commands/catkin_create_pkg](https://wiki.ros.org/catkin/commands/catkin_create_pkg)
### 编译软件包

新建完软件包、编写完软件包的源代码之后，往往还要配置软件包。也就是配置软件包目录下的 `package.xml` 和 `CMakelists.txt`

在配置完软件包之后才可以编译软件包，然后回到工作区根目录下可以使用 `catkin_make` 同时编译工作区 `src` 目录下的所有项目（也就是所有软件包）
```shell
# 切换到 catkin 工作区的根目录下
catkin_make

# 如果你的软件包不在默认位置（也就是src目录下）
# 用 --source 选项指定软件包的路径
#  catkin_make --source my_src
```

如果发现使用这个 `catkin_make` 命令失败，看看是不是没有配置好 ROS1 的环境，这是第一次 `source`
```shell
source /opt/ros/<distro>/setup.bash
```

第二次还要 `source` 工作区的初始化脚本，之前在创建完工作区之后就已经编译过，然后 `source` 过了，这次应该就不用 `source`

但是有时候启动不了软件包的节点是因为没有 `source` 到的原因，还是要注意一下这一点。

`catkin_make` 是一个按照软件包编译的流程，依次调用了 `make` 和 `cmake` 的脚本工具，简化了编译软件包的流程


# ROS 图

## 图的概念

图是一个由 ROS 进程组成的点对点的网络，在网络中的各个节点可以通过话题通信和服务通信这些通信方式互相交换和处理数据

一个图包含下面几个元素
- **节点(Node)** 由源代码编译链接形成的可执行文件。它可以通过 ROS 与其他节点进行通信
- **消息(Message)** 表征数据类型的结构体。在进行话题通信时，传输数据所使用的数据类型就是这个结构体
- **话题 (Topic)** 话题通信的中介场所，节点可以把消息发布到话题，或者通过订阅话题来接收消息
- **主节点(Master)** ROS 自己保留的一个节点，例如帮助自己编写的节点发现彼此
## 节点的概念

节点实际上只不过是 ROS 软件包中的一个可执行文件。它可以进行话题通信，也可以进行服务通信，是一个能执行特定工作任务的工作单元，并且能够相互通信，从而实现一个机器人系统整体的功能

节点之间相互通信的底层实现是由 ROS 客户端库负责的。而这个客户端库也没那么复杂，就是在创建软件包时添加的依赖包 `rospy` 和 `roscpp`，它们可以让对应的不同编程语言编写的节点进行相互通信

## 话题通信的概念

话题的通信，是一个节点把数据填进消息结构体，发布到话题这个中介场所，然后另一个订阅这个话题的节点就可以接收的放在话题上的消息从而收到数据。

这里发送消息到话题的节点就被称为发布者(Publisher)，订阅话题的节点就是订阅者(Subscriber)

也就是说通过话题这个中介场所机制，发布者和订阅者之间发送和接收的消息的**类型是同一类型的**，并且**话题的类型是由发布在它上面的消息的类型决定的**

它的使用场景在于不断更新的、少逻辑处理的数据传输场景

此外在实际使用节点的时候，都会运行 `roscore` 这个命令。这个命令产生的主节点被称为管理者 (Master) 

所以一个最简单的话题通信实现模型一共需要三个节点完成。首先是 Master 负责保管 Publisher 和 Subscriber 注册的信息，并匹配在同一话题下的 Publisher 和 Subscriber，帮助 Publisher 和 Subscriber 建立连接，连接建立之后，Publisher 就可以发布消息，并且发布的消息会被 Subscriber 订阅

下面是整个话题通信的初始化流程框图，实际上这个初始化并不需要用户去配置，是由 ROS 中间层自动配置的
![](/img/robots/话题通信理论模型.png)
0. Talker 注册：Talker 启动后，会通过RPC在 ROS Master 中注册自身信息，其中包含所发布消息的话题名称。ROS Master 会将节点的注册信息加入到注册表中。
1. Listener 注册：Listener 启动后，也会通过RPC在 ROS Master 中注册自身信息，包含需要订阅消息的话题名。ROS Master 会将节点的注册信息加入到注册表中。
2. ROS Master 实现信息匹配：ROS Master 会根据注册表中的信息匹配Talker 和 Listener，并通过 RPC 向 Listener 发送 Talker 的 RPC 地址信息。
3. Listener 向 Talker 发送请求：Listener 根据接收到的 RPC 地址，通过 RPC 向 Talker 发送连接请求，传输订阅的话题名称、消息类型以及通信协议(TCP/UDP)
4. Talker确认请求：Talker 接收到 Listener 的请求后，也是通过 RPC 向 Listener 确认连接信息，并发送自身的 TCP 地址信息。
5. Listener 和 Talker 建立连接：Listener 根据步骤4 返回的消息使用 TCP 与 Talker 建立网络连接
6. Talker 向 Listener 发送消息：连接建立后，Talker 开始向 Listener 发布消息

Ps：
1. 上述实现流程中，前五步使用的 RPC协议，最后两步使用的是 TCP 协议
2. Talker 与 Listener 的启动无先后顺序要求
3. Talker 与 Listener 都可以有多个
4. Talker 与 Listener 连接建立后，不再需要 ROS Master 也就是说即便关闭了 ROS Master，Talker 与 Listern 照常通信

需要注意的是，在实际操作的过程中，我们只用在 ROS 应用层编写软件包分别实现发布者节点和订阅者节点，以及准备好要发送的消息类型数据就行啦

## 话题通信的操作流程 

之前提到，在实际操作的过程中，ROS Master 由 `roscore` 提供了，而一系列节点连接的建立也已经被 ROS 中间层封装了

实际上只需要在 ROS 应用层编写软件包分别实现发布者节点和订阅者节点，以及准备好要发送的消息类型数据

1. 传递的消息类型 + 其所在的软件包
2. 发布方节点 + 其所在的软件包
3. 订阅方节点 + 其所在的软件包

### 确定所用的话题

首先得确定发布的话题名称，这个一定要对
可以通过 `rostopic` 获取现存话题，或新建话题

### 确定消息类型

然后要确定消息类型，这个一定要符合需求
可以通过 `rosmsg` 获取现存消息类型，或自定义消息类型，然后再补充相关依赖

在 ROS 通信协议中，数据载体是一个较为重要组成部分。具体到话题通信中，就是要确定好消息的类型

消息的类型体现在 `msg` 文件中。其实 `msg` 文件就是一个文本文件，里面就是一些用于描述消息的字段，蕴含的意思很像结构体。它们用于为不同编程语言编写的消息生成源代码。一般 `msg` 文件存放在软件包的 `msg` 目录下

可以把 `msg` 文件看成一个结构体，每行都有一个字段类型和字段名称。可使用的字段类型就是 ROS 约定的数据类型
```
int8, int16, int32, int64 (或者无符号类型: uint*)
float32, float64
string
time, duration
其他 msg 文件
variable-length array[] 和 fixed-length array[C]
```

ROS 中还有一个特殊的数据类型：`Header`，它含有时间戳和 ROS 中广泛使用的坐标帧信息。在 `msg` 文件的第一行经常可以看到 `Header header`。

下面是一个 `msg` 文件的样例，它使用了 `Header`，`string`，和其他另外两个消息类型(就是使用其他 `msg` 文件规定的类型)：
```
  Header header
  string child_frame_id
  geometry_msgs/PoseWithCovariance pose
  geometry_msgs/TwistWithCovariance twist
```

消息的类型根据需要确定好后，可以看看能不能直接使用已经现成的消息软件包里面的消息类型，比如在 `std_msgs` 这个软件包里就封装好了一些消息类型 (`String`、`Int32`、`Int64`、`Char`、`Bool`、`Empty`) 

但是对于上述的各个消息类型而言，它们的 `msg` 文件只包含了各自对应的字段，比如 `String` 的 `msg` 文件就只包含了这一个 `string` 字段，就算从 ` std_msgs ` 软件包里包含了 ` String ` 这个消息类型，每次发布者在发布消息时也只能传输单一 ` string ` 类型的数据

但是如果要传输一些结构复杂的数据，比如传感器的数据。直接传输 `std_msgs` 的单一消息类型往往力不从心。如果不可以满足需求，可以考虑自己在一个软件包来自己写一个 `msg` 文件，并在里面包含多个字段类型，按照需求组成一个消息类型结构体


自定义消息文件的步骤是
1. 按照固定格式创建 `msg` 文件
2. 编辑配置文件
3. 编译生成可以被 `Python` 或 `Cpp` 调用的中间文件

首先在一个软件包路径下创建一个 `msg` 文件夹，然后首字母大写来命名 `.msg` 消息文件
然后编辑软件包路径下的 `package.xml` 文件，添加编译时和启动时依赖
1. 在里面添加 `<build_depend>message_generation</build_depend>` 编译时依赖
2. 在里面添加 `<exec_depend>message_runtime</exec_depend>` 运行时依赖

接着编辑软件包路径下的 `CMakeLists.txt` 文件，在 `CMakeLists.txt` 文件中，为已经存在里面的 `find_package` 调用添加 `message_generation` 依赖项，这样就能生成消息了。直接将 `message_generation` 添加到 `COMPONENTS` 列表中即可：
```CMake
# 不要直接复制这一大段，只需将 message_generation 加在括号闭合前即可
find_package(catkin REQUIRED COMPONENTS
   roscpp
   rospy
   std_msgs
   message_generation
)
```
还要确保导出消息的运行时依赖关系：
```CMake
catkin_package(
  ...
  CATKIN_DEPENDS message_runtime ...
  ...)
```
再找到下面的代码块：
```CMake
# 取消原来 CMakeLists.txt 里的注释得到
add_message_files(
  FILES
  # 在这里填写自己定义的 msg 文件
  # 比如下面这个
  Number.msg
)
```
在手动添加 `msg` 文件后，还要确保 `generate_messages()` 函数被调用
```CMake
# 取消原来 CMakeLists.txt 里的注释得到
generate_messages(
  DEPENDENCIES
  std_msgs
  # 在这里填包含了自己 msg 文件的软件包
)
```
最后回到 `catkin` 工作空间，使用 `catkin_make` 就行啦，最终就会生成可以供 `Cpp` 和 `Python` 使用的消息类型代码了

表达消息类型的时候一般要用两个部分，一个是定义消息的软件包，另一个就是 `msg` 文件的名称，比如 `std_msg/String`。实际上，在终端使用 `rosmsg` 查看消息类型和在 `cpp` 源文件中包含消息类型时，遵循的都是这样的格式。

可以使用 `rosmsg show [package_name]/[message_name]` 来显示消息对应 `msg` 文件的内容。如果忘记了消息属于哪个软件包，也可以省略软件包名称直接使用 `rosmsg show <message_name>`

可以写一个脚本完成

可以积累完成任务的常见软件包和相应的消息类型
1. 速度消息的软件包一般是 `geometry_msgs`

为了方便代码提示以及避免误抛异常，需要先配置 `vscode`，将前面生成的 `head` 文件路径配置进 `c_cpp_properties.json` 的 `includepath` 属性:
```json
{
    "configurations": [
        {
            "browse": {
                "databaseFilename": "",
                "limitSymbolsToIncludedHeaders": true
            },
            "includePath": [
                "/opt/ros/noetic/include/**",
                "/usr/include/**",
                "/xxx/yyy工作空间/devel/include/**" //配置 head 文件的路径 
            ],
            "name": "ROS",
            "intelliSenseMode": "gcc-x64",
            "compilerPath": "/usr/bin/gcc",
            "cStandard": "c11",
            "cppStandard": "c++17"
        }
    ],
    "version": 4
}

```
还要将前面生成的 `python` 文件路径配置进 `settings.json`
```json
{
    "python.autoComplete.extraPaths": [
        "/opt/ros/noetic/lib/python3/dist-packages",
        "/xxx/yyy工作空间/devel/lib/python3/dist-packages"
    ]
}

```

### 初始化 ROS 语言接口

#### Cpp
##### 1.包含头文件

可以先去配置 VScode 中的配置文件
`include` 当前工作目录，防止出现报错，不过报错不影响程序编写

一般要引入头文件 `#include "ros/ros.h"`
目的是导入 ROS 的 `Cpp` 相关库

再引入相应使用的消息或者服务的对应头文件，格式为 
```C
#include "package_name/message_name"
#include "package_name/service_name"
```
目的是可以使用相应的消息类型和服务类型

##### 2. 初始化 ROS 节点

在主函数 `main` 里面可以加入 `setlocale(LC_ALL,"");` 这个函数的调用，来解决中文乱码问题

然后使用 `ros::init` 初始化函数初始化一个新的节点
```C
//1.引入头文件
#include "ros/ros.h"
#include "package_name/message_name"
#include "package_name/service_name"

int main(int argc, char *argv[])
{  
    setlocale(LC_ALL,"");//解决中文乱码

     //2.初始化 ROS 节点,节点名称要保证唯一
    ros::init(argc,argv,"Node_name"); 
    
    return 0;
}
```

##### 3.创建节点句柄

节点句柄是一个句柄类型的对象，这个类型封装好了一些对节点进行操作的接口。如果说节点是一扇门，初始化这样的一个对象，就相当于找到了这个门的把手，通过这个句柄就可以操作门背后节点的一系列东东

```C++
//1.引入头文件
#include "ros/ros.h"
#include "package_name/message_name"
#include "package_name/service_name"
int main(int argc, char *argv[])
{  
    setlocale(LC_ALL,"");//解决中文乱码

    //2.初始化ros节点,节点名称要保证唯一
    ros::init(argc,argv,"Node_name"); 

    //3.创建节点句柄
    ros::NodeHandle nh;
    
    return 0;
}
```

一般创建的节点句柄对象叫做 `nh`
##### 4. 如果有必要使用回调函数

如果需要使用回调函数，那么就要在程序最后运行的代码末尾处最后添加一个 `ros::spin()` 语句

如果最后程序终止在一个循环里，添加的语句就应该是 `ros::spinOnce()`

#### Python
##### 1.导入模块

先是要 `import rospy`，目的是导入这个 ROS 的 python 接口

然后再导入相应的消息类型和服务类型，格式为
```python
from package_name.msg import message_name
from package_name.srv import service_name
```

一个完整的例子如下
```python
#!/usr/bin/env python
# 每个 Python ROS 节点的最开头都有这个声明。这是为了确保脚本作为Python 脚本执行

import rospy 
# 导入 rospy 以便支持使用 ros 的 python 接口编写节点 
from std_msgs.msg import String
# 从对应的软件包导入消息类型
# 要使用 std_msg/String 类型，就要先用这样的语句导入 from std_msgs.msg import String
```


##### 2.初始化 ROS 节点

使用 `rospy.init_node` 这个函数进行节点初始化

```python
rospy.init_node('Node_name', anonymous=True)
# 这一句非常重要，它把该节点的名称告诉了 rospy
# 只有 rospy 掌握了这一信息后，才会开始与 ROS 主节点进行通信
# anonymous = True 会让名称末尾添加随机数
# 来确保节点具有唯一的名称
# 如果两个节点命名空间一样，节点名称也一样，那么ROS会默认杀死先前启动的节点，为了避免节点被顶掉，要使用 anonymous 这个参数为节点名字编号生成随机数
```

和 `Cpp` 不同的是，`python` 没有必要创建节点句柄来实现对节点参数的配置，直接使用导入的 `rospy` 模块的方法和类型就行



##### 3.如果有必要使用回调函数

在程序最后运行的代码末尾处使用 `rospy.spin` 
`rospy.spin` 只是不让这个节点退出，直到节点被明确关闭。与 `roscpp` 不同，`rospy.spin` 不影响程序的回调函数调用，因为 `rospy` 给了它们自己的线程


### 编写发布者节点

#### Cpp

注意在编写节点的具体逻辑前，一定要先初始化 ROS 接口
也就是说要包含相关头文件，然后初始化节点，再创建好句柄对象
##### 创建发布对象

利用句柄创建一个类型为 `ros::Publisher` 的发布对象，这个对象一般叫做 `pub`

具体就是利用 `ros::NodeHandle` 类型的句柄对象 `nh` 的 ` advertise<> ` 模板函数

这个模板函数的模板就是需要发布的消息类型，格式为
```C++
ros::Publisher pub = nh.advertise<package_name::message_name>("topic_name",10)
# 第一个参数是话题名称
# 第二个参数是消息队列
# 就是如果订阅者来不及订阅
# 发布者可以缓冲的消息个数
```


```C++
//1.引入头文件
#include "ros/ros.h"
#include "package_name/message_name"
#include "package_name/service_name"
int main(int argc, char *argv[])
{  
    setlocale(LC_ALL,"");//解决中文乱码

    //2.初始化ros节点,节点名称要保证唯一
    ros::init(argc,argv,"Node_name"); 

    //3.创建节点句柄
    ros::NodeHandle nh;

	//4.创建发布对象
	ros::Publisher pub =  
	nh.advertise<package_name::message_name>("topic_name",10);
    
    return 0;
}
```
##### 创建消息对象并配置数据

之前包含的消息文件头文件就有声明了对应的消息类型
使用下面这样的格式来为消息类型创建对应的消息对象
```C++
package_name::message_name message_object;

# 比如之前包含了 geometry_msgs/Twist
# 就可以创建 geometry_msgs::Twist twist;
```

消息类型对应 `msg` 文件的一系列字段被转换成 `Cpp` 中类型的属性了，直接使用 `.` 句点用对象调用就行

比如依照 `geometry_msgs/Twist` 的 `msg` 文件，利用消息对象 `twist` 组织速度消息
```cpp
twist.linear.x= 1.0;
twist.linear.y= 0.0;
twist.linear.z= 0.0;
twist.angular.x=0.0; //r
twist.angular.y=0.0; //p
twist.angular.z=1.0; //y
```

```c++
//1.引入头文件
#include "ros/ros.h"
#include "package_name/message_name"
#include "package_name/service_name"
int main(int argc, char *argv[])
{  
    setlocale(LC_ALL,"");//解决中文乱码

    //2.初始化ros节点,节点名称要保证唯一
    ros::init(argc,argv,"Node_name"); 

    //3.创建节点句柄
    ros::NodeHandle nh;

    //4.创建发布对象
    ros::Publisher pub =  
    nh.advertise<package_name::message_name>  ("topic_name",10);

	//5.创建消息对象并配置数据
	package_name::message_name message_object;
	/* 配置消息对象的数据 */

    return 0;
}
```
##### 配置发布逻辑

这里第一步是设置发布频率，利用 `ros::Rate` 类型的对象构造函数，创建一个 `rate` 对象，设置发布频率，里面的参数是发布频率，单位是 `Hz`
```C++
ros::Rate rate(10); //使用类的构造函数，参数是 10 Hz
```


完成完上述步骤之后，再引入一个循环进行不断发布。在循环体内利用发布对象 `pub` 的 `publish` 函数，把配置好数据的消息对象 `message_obejct` 发布出去。

注意在循环体内使用先前创建的 `rate` 对象的休眠函数 `rate.sleep` 配合设置发布频率。如果要使用回调函数，就还要再加一个回旋函数 `ros::spinOnce`

```C++
ros::Rate rate(10); //使用类的构造函数，参数是 10 Hz
while (ros::ok()){  //循环条件是节点正常运行
    
    pub.publish(twist);//把消息通过发布对象发布
   
	rate.sleep();//让发布者按照 rate 对象之前设置的频率发布
 
    ros::spinOnce(); //如果这个节点有回调函数则要使用
}
```


```c++
//1.引入头文件
#include "ros/ros.h"
#include "package_name/message_name"
#include "package_name/service_name"
int main(int argc, char *argv[])
{  
    setlocale(LC_ALL,"");//解决中文乱码

    //2.初始化ros节点,节点名称要保证唯一
    ros::init(argc,argv,"Node_name"); 

    //3.创建节点句柄
    ros::NodeHandle nh;

    //4.创建发布对象
    ros::Publisher pub =  
    nh.advertise<package_name::message_name>  ("topic_name",10);

	//5.创建消息对象并配置数据
	package_name::message_name message_object;
	/* 配置消息对象的数据 */

	//6.配置发布逻辑
	ros::Rate rate(10); //使用类的构造函数，参数是 10 Hz
    while (ros::ok()){  //循环条件是节点正常运行
     
      pub.publish(twist);//把消息通过发布对象发布
      rate.sleep();//让发布者按照 rate 对象之前设置的频率发布
      ros::spinOnce(); //如果这个节点有回调函数则要使用
    }
    return 0;
}
```
##### 示例程序

注意 `cpp` 源文件要放在软件包的 `src` 目录下面

```c++
//1.引入头文件
#include "ros/ros.h"
#include "package_name/message_name"
#include "package_name/service_name"
int main(int argc, char *argv[])
{  
    setlocale(LC_ALL,"");//解决中文乱码

    //2.初始化ros节点,节点名称要保证唯一
    ros::init(argc,argv,"Node_name"); 

    //3.创建节点句柄
    ros::NodeHandle nh;

    //4.创建发布对象
    ros::Publisher pub =  
    nh.advertise<package_name::message_name>  ("topic_name",10);

	//5.创建消息对象并配置数据
	package_name::message_name message_object;
	/* 配置消息对象的数据 */

	//6.配置发布逻辑
	ros::Rate rate(10); //使用类的构造函数，参数是 10 Hz
    while (ros::ok()){  //循环条件是节点正常运行
     
      pub.publish(twist);//把消息通过发布对象发布
      rate.sleep();//让发布者按照 rate 对象之前设置的频率发布
      ros::spinOnce(); //如果这个节点有回调函数则要使用
    }
    return 0;
}
```



#### Python

注意在编写节点的具体逻辑前，一定要先初始化 ROS 接口
也就是说要导入相关模块，然后初始化节点
##### 创建发布对象

与 `cpp` 不同，ROS 的 `python` 接口已经有 `rospy` 模块了，不需要再创建一个句柄对象并调用句柄对象的模板函数来声明并初始化发布者对象

这里直接用句点 `rospy.Publisher()` 去调用 `rospy` 的 `Publisher` 函数来初始化发布者对象，还是一般叫做 `pub`
```python
pub = rospy.Publisher('topic_name', message_name, queue_size=10)
```
这里只用传入消息名称，并不需要像模板函数那样严格的指定软件包和消息名称

对比一下 `cpp` 的版本，可以看出来发布对象的构造函数一定要有的参数就是话题名称，以及消息类型，还有消息队列大小
```C++
ros::Publisher pub = nh.advertise<package_name::message_name>("topic_name",10)
# 第一个参数是话题名称
# 第二个参数是消息队列大小
# 就是如果订阅者来不及订阅
# 发布者可以缓冲的消息个数
```


```python
# 1.引入相应模块
#! /usr/bin/env python
import rospy
from package_name.msg import message_name

if __name__ = "__main__":
    # 2.初始化 ROS 节点
	rospy.init_node("Node_name", anonymous=True)

	# 3.创建发布对象
	pub = rospy.Publisher("topic_name",message_name,queque_size = 10)
```
##### 创建消息对象并配置数据

之前导入的消息文件头模块就有对应的消息类型信息
和 `cpp` 直接声明对象不同，`python` 是用构造函数来为消息类型创建对应的消息对象，这个构造函数的名称就是消息名称
```python
message_object = message_name()
```

对比 `cpp` 直接声明消息对象
```C++
package_name::message_name message_object;
```

如果遇到有多个字段的消息类型，一般的经验法是，`python` 的构造函数参数的顺序与 `.msg` 文件中的顺序相同。也可以不传入任何参数，直接初始化字段，然后再通过对象的句点调用赋值

和 `cpp` 一样，消息类型对应 `msg` 文件的一系列字段也被转换成 `python` 中类型的属性了，直接使用 `.` 句点用对象调用就行

比如依照 `geometry_msgs/Twist` 的 `msg` 文件，利用消息对象 `twist` 组织速度消息
```python
twist = Twist() # 创建消息对象 twist

# 配置消息对象的数据
twist.linear.x= 1.0
twist.linear.y= 0.0
twist.linear.z= 0.0
twist.angular.x=0.0 # r
twist.angular.y=0.0 # p
twist.angular.z=1.0 # y
```


```python
# 1.引入相应模块
#! /usr/bin/env python
import rospy
from package_name.msg import message_name

if __name__ = "__main__":
    # 2.初始化 ROS 节点
	rospy.init_node("Node_name", anonymous=True)

	# 3.创建发布对象
	pub = rospy.Publisher("topic_name",message_name,queque_size = 10)

	# 4.创建消息对象并配置数据
	message_object = message_name()
	# 配置消息对象的数据
```
##### 配置发布逻辑

这里第一步是设置发布频率，和 `cpp` 类似，使用 `rospy` 提供的 `Rate` 类构造函数声明和初始化一个设置发布频率的对象
```python
rate = Rate(10) # 10hz
```

对比 `cpp` 版本
```C++
ros::Rate rate(10); //使用类的构造函数，参数是 10 Hz
```


完成完上述步骤之后，再引入一个循环进行不断发布。在循环体内利用发布对象 `pub` 的 `publish` 函数，把配置好数据的消息对象 `message_obejct` 发布出去。这点和 `cpp` 一样。


注意在循环体内使用先前创建的 `rate` 对象的休眠函数 `rate.sleep` 配合设置发布频率。如果要使用回调函数，就还要再加一个回旋函数 `rospy.spin`

```python
# 1.引入相应模块
#! /usr/bin/env python
import rospy
from package_name.msg import message_name

if __name__ = "__main__":
    # 2.初始化 ROS 节点
	rospy.init_node("Node_name", anonymous=True)

	# 3.创建发布对象
	pub = rospy.Publisher("topic_name",message_name,queque_size = 10)

	# 4.创建消息对象并配置数据
	message_object = message_name()
	# 配置消息对象的数据

	# 5.配置发布逻辑
	rate = rospy.Rate(10)
	while not rospy.is_shutdown():
    # 相当标准的结构：先检查rospy.is_shutdown()标志
    # 然后确认正常后再执行代码逻辑
       pub.publish(message_object)
       # 利用 pub 对象发布消息到对应的话题
       
       rate.sleep()
       # 在循环中可以用刚刚好的睡眠时间
       # 来维持构造 rate 对象时所期望的速率
       rospy.spin() # 如果节点有回调函数要调用就加上

```
##### 示例程序

注意 `python` 源文件要放在对应软件包的 `scripts` 目录下

```python
#!/usr/bin/env python
# 每个 Python ROS 节点的最开头都有这个声明。这是为了确保脚本作为Python 脚本执行

import rospy 
# 导入 rospy 以便支持使用 ros 的 python 接口编写节点 
from std_msgs.msg import String
# 从对应的软件包导入消息类型
# 要使用 std_msg/String 类型，就要先用这样的语句导入 from std_msgs.msg import String

def talker():
    pub = rospy.Publisher('chatter', String, queue_size=10)
    # 构造了一个Publisher类型的发布者对象 pub
	# 声明该节点正在使用 String 消息类型发布到 chatter 话题
	# queue_size 参数用于在订阅者接收消息的速度不够快的情况下
	# 限制排队的消息数量

    rospy.init_node('talker', anonymous=True)
	# 这一句非常重要，它把该节点的名称告诉了 rospy
	# 只有 rospy 掌握了这一信息后，才会开始与 ROS 主节点进行通信
	# anonymous = True 会让名称末尾添加随机数
	# 来确保节点具有唯一的名称
	# 如果两个节点命名空间一样，节点名称也一样，那么ROS会默认杀死先前启动的节点，为了避免节点被顶掉，要使用 anonymous 这个参数为节点名字编号生成随机数
	
    rate = rospy.Rate(10) # 10hz
	# 构造了 Rate 类的对象 rate，并以每秒循环 10 次的速率发布

    while not rospy.is_shutdown():
    # 相当标准的结构：先检查rospy.is_shutdown()标志
    # 然后确认正常后再执行代码逻辑
        hello_str = "hello world %s" % rospy.get_time()
        # python 的格式化操作符替换，类似 C 语言
        
        rospy.loginfo(hello_str)
	    # 它可以完成三件事情
	    # 打印消息到屏幕上
	    # 把消息写入节点的日志文件中
	    # 写入 rosout 以便使用 rqt_console 调试

        pub.publish(hello_str)
	    # 利用 pub 对象发布消息到对应的话题

        rate.sleep()
        # 在循环中可以用刚刚好的睡眠时间
        # 来维持构造 rate 对象时所期望的速率
if __name__ == '__main__':

	# 配合 rospy.sleep 抛出的异常进行处理 
    try:
        talker()
    except rospy.ROSInterruptException:
        pass
```

### 编写订阅者节点
#### Cpp

注意在编写节点的具体逻辑前，一定要先初始化 ROS 接口
也就是说要包含相关头文件，然后初始化节点，再创建好句柄对象
##### 创建订阅对象

利用句柄创建一个类型为 `ros::Subscriber` 的发布对象，这个对象一般叫做 `sub`

具体就是利用 `ros::NodeHandle` 类型的句柄对象 `nh` 的 ` subscribe<> ` 模板函数，注意了，` subscribe<> ` 虽然需要消息类型的范型，但是它可以根据回调函数的形参算出，所以不必加 `<>`
```cpp
ros::Subscriber sub = nh.subscribe("topic_name", 10,callBack)
# 第一个参数是话题名称
# 第二个参数是消息队列大小
# 第三个参数是回调函数，通常叫做 ...callBack 或 do...
```

通常处理订阅数据是在回调函数封装完的，所以先在程序的末尾提前写下 `ros::spin()` 或者 `ros::spinOnce()`

```cpp
// 1.包含头文件
#include <ros/ros.h>
#include "package_name/message_name.h"

int main(int argc, char **argv)
{
	// 2.初始化 ROS 节点
	ros::init(argc, argv, "listener"); 

    // 3.创建节点句柄
	ros::NodeHandle nh;

	//4.创建订阅对象
	ros::Subscriber sub = nh.subscribe("topic_name", 10, callback);
	
    ros::spin();
    //进入ROS的事件循环
    //等待接收消息并调用相应的回调函数进行处理
 
    return 0;
 }
```
##### 在回调函数中处理订阅的数据

这里主要注意回调函数的声明格式就行
```c++
void callBack(const package_name::message_name::ConstPtr &message_name_ptr)
{
	attribute_1 = message_name_ptr->attribute_1
	/* · · · */
}
```

注意回调函数的类型是 `void`，参数类型是指向消息对象的一个指针
`const 消息所在软件包::消息名称::ConstPtr &指向消息对象的指针`，这里的指针名字一般是消息名称的小写

在通过这个指针获取消息对象的数据时，要用解引用加句点的混合符 `->` 去或许对象的属性

#### Python

注意在编写节点的具体逻辑前，一定要先初始化 ROS 接口
也就是说要导入相关模块，然后初始化节点
##### 创建订阅对象

大体上和 `cpp` 一样，区别在于 `python` 不依赖句柄对象，直接使用 `rospy` 的 `Subscriber` 函数创建订阅者对象，并且需要指定消息名称
```python
sub = rospy.Subscriber("topic_name", message_name, callBack)
```

对比 `cpp` 的版本
```cpp
ros::Subscriber sub = nh.subscribe("topic_name", 10,callBack)
# 第一个参数是话题名称
# 第二个参数是消息队列大小
# 第三个参数是回调函数，通常叫做 ...callBack 或 do...
```

最后还是得要在程序的末尾处加上 `rospy.spin()` 来触发回调函数

##### 在回调函数中处理订阅的数据

与 `cpp` 版本相比，`python` 版本的回调函数传参就比较简单，形参只有一个代表消息的对象。直接对形参使用句点调用就可以处理消息字段里面的数据了
```python
def callback(message_object):
	attribute_1 = message_object.attribute_1
	# ...
```

##### 示例程序

注意 `python` 源文件要放在对应软件包的 `scripts` 目录下

```python
#!/usr/bin/env python
import rospy
from std_msgs.msg import String

def callback(data):
# 回调函数，一旦接收到话题的消息就会触发
# 触发的效果就是进入这个函数内部
    rospy.loginfo(rospy.get_caller_id() + "I heard %s", data.data)
    
def listener():
 
    rospy.init_node('listener', anonymous=True)
    rospy.Subscriber("chatter", String, callback)
    # 利用 Subscriber 类构造订阅者对象，主要是连接好话题和回调函数
    
    rospy.spin()
    # rospy.spin() 只是不让这个节点退出，直到节点被明确关闭
    # 与 roscpp 不同，rospy.spin()不影响订阅者回调函数
    # 因为 rospy 给了它们自己的线程
    
if __name__ == '__main__':
    listener()
```



### 构建节点并检验

注意 `cpp` 源文件要放在软件包的 `src` 目录下面
注意 `python` 源文件要放在对应软件包的 `scripts` 目录下

对于需要编译成节点的 `cpp` 源文件，就需要回去软件包的 `CMakeLists.txt` 文件依次添加 `add_executable`、`add_dependencies` 还有 `target_link_libraries`
```cmake
add_executable(executable_file_name_1 src/source_1.cpp)
add_executable(executable_file_name_2 src/source_2.cpp)

add_dependencies(executable_file_name_1 ${PROJECT_NAME}_generate_messages_cpp)
add_dependencies(executable_file_name_2 ${PROJECT_NAME}_generate_messages_cpp)


target_link_libraries(executable_file_name_1
  ${catkin_LIBRARIES}
)
target_link_libraries(executable_file_name_2
  ${catkin_LIBRARIES}
)
```

对于要编译成节点的 `python` 源文件，需要将以下内容添加到软件包的 `CMakeLists.txt` 文件，进行 `catkin_install_python()` 调用。这样可以确保正确安装 `Python` 脚本，并使用合适的 `Python` 解释器
```cmake
catkin_install_python(PROGRAMS 
					  scripts/Node_name_1.py
					  ...
					  scripts/Node_name_N.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
```


即使是 `Python` 节点也必须使用 `catkin_make`。这是为了确保能为自己创建的消息和服务文件自动生成 `Python` 代码

回到 `catkin` 工作空间，然后运行 `catkin_make` 


## 服务通信的概念

服务(Service)是节点之间通信的另一种方式，是基于**请求响应**模式的，是一种应答机制。

服务允许一个节点作为服务器，其他任意数量的节点可以向服务端发送一个请求(request)并从服务端获得一个响应(response)

那么它就更适用于偶然的，对时时性有要求、具有固定逻辑处理的应用场景

它较之于话题通信更简单些，Master 负责保管 Server 和 Client 注册的信息，并匹配服务相同的 Server 与 Client ，帮助 Server 与 Client 建立连接，连接建立后，Client 发送请求信息，Server 返回响应信息

下面就是具体的服务通信初始化流程框图，实际上这个初始化并不需要用户去配置，是由 ROS 中间层自动配置的

![服务通信理论模型.png](/img/robots/服务通信理论模型.png)
0. Server注册：Server 启动后，会通过RPC在 ROS Master 中注册自身信息，其中包含提供的服务的名称。ROS Master 会将节点的注册信息加入到注册表中。
1. Client注册：Client 启动后，也会通过RPC在 ROS Master 中注册自身信息，包含需要请求的服务的名称。ROS Master 会将节点的注册信息加入到注册表中。
2. ROS Master实现信息匹配：ROS Master 会根据注册表中的信息匹配Server和 Client，并通过 RPC 向 Client 发送 Server 的 **TCP** 地址信息。
3. Client发送请求：Client 根据步骤 2 响应的信息，使用 TCP 与 Server 建立网络连接，并发送请求数据。
4. Server发送响应：Server 接收、解析请求的数据，并产生响应结果返回给 Client

P.S:
> 1.客户端请求被处理时，需要保证服务器已经启动；
> 
> 2.服务端和客户端都可以存在多个。

## 服务通信的操作流程

### 确定服务通信名称

服务通信的名称服务端和客户端一定要统一。注意了这里的名称可不是服务类型的名称，不要搞混了，这里的服务通信名称是用来连接服务端和客户端的
### 确定服务类型

`srv`文件也和 `msg` 文件一样是一个文本文件，不过是用来描述一个服务的。它由两部分组成：请求（request）和响应（response）

在服务通信中，数据分成两部分，请求与响应，在 `srv` 文件中请求和响应使用 `---` 分割，具体长成下面这样
```
# 客户端请求时发送的数据，这里是两个整数
int32 num1
int32 num2
---
# 服务器响应发送的数据，这里是一个整数
int32 sum
```

这里的 `srv` 文件其实也可以看成两个结构体，`---` 上面的就是请求数据结构体，下面的就是响应数据结构体。每行都有一个字段类型和字段名称作为成员变量。可使用的字段类型和 `msg` 一样是 ROS 约定的数据类型
```
int8, int16, int32, int64 (或者无符号类型: uint*)
float32, float64
string
time, duration
其他 srv 文件
variable-length array[] 和 fixed-length array[C]
```

自定义服务文件具体就需要下面几步
1. 按照固定格式创建 `srv` 文件
2. 编辑配置文件
3. 编译生成中间文件

首先创建 `srv` 文件时，要存放在软件包的 `srv` 目录下
，然后首字母大写命名保持规范。接下来其实编辑配置文件的步骤和 `msg` 一模一样，不要被迷惑了。

先是编辑软件包路径下的 `package.xml` 文件，添加编译时和启动时依赖
1. 在里面添加 `<build_depend>message_generation</build_depend>` 编译时依赖
2. 在里面添加 `<exec_depend>message_runtime</exec_depend>` 运行时依赖

接着编辑软件包路径下的 `CMakeLists.txt` 文件，在 `CMakeLists.txt` 文件中，为已经存在里面的 `find_package` 调用添加 `message_generation` 依赖项，这样就能生成消息了。直接将 `message_generation` 添加到 `COMPONENTS` 列表中即可：
```CMake
# 不要直接复制这一大段，只需将 message_generation 加在括号闭合前即可
find_package(catkin REQUIRED COMPONENTS
   roscpp
   rospy
   std_msgs
   message_generation
)
```
别被名字迷惑，`message_generation` 对 `msg` 和 `srv` 都适用

再找到下面的代码块：
```CMake
# 取消原来 CMakeLists.txt 里的注释得到
add_service_files(
  FILES
  # 在这里填写自己定义的 srv 文件
  # 比如下面这个
  AddTwoInts.srv
)
```
在手动添加 `srv` 文件后，还要确保 `generate_messages()` 函数被调用
```CMake
# 取消原来 CMakeLists.txt 里的注释得到
generate_messages(
  DEPENDENCIES
  std_msgs
  # 在这里填包含了自己 msg 文件的软件包
)
```
注意: 官网没有在 `catkin_package` 中配置 `message_runtime`，经测试配置也可以

### 初始化 ROS 接口

#### Cpp
##### 1. 包含头文件

可以先去配置 VScode 中的配置文件
`include` 当前工作目录，防止出现报错，不过报错不影响程序编写

一般要引入头文件 `#include "ros/ros.h"`
目的是导入 ROS 的 `Cpp` 相关库

再引入相应使用的消息或者服务的对应头文件，格式为 
```C
#include "package_name/message_name"
#include "package_name/service_name"
```
目的是可以使用相应的消息类型和服务类型

##### 2. 初始化 ROS 节点

在主函数 `main` 里面可以加入 `setlocale(LC_ALL,"");` 这个函数的调用，来解决中文乱码问题

然后使用 `ros::init` 初始化函数初始化一个新的节点
```C
//1.引入头文件
#include "ros/ros.h"
#include "package_name/message_name"
#include "package_name/service_name"

int main(int argc, char *argv[])
{  
    setlocale(LC_ALL,"");//解决中文乱码

     //2.初始化 ROS 节点,节点名称要保证唯一
    ros::init(argc,argv,"Node_name"); 
    
    return 0;
}
```

##### 3. 创建节点句柄

节点句柄是一个句柄类型的对象，这个类型封装好了一些对节点进行操作的接口。如果说节点是一扇门，初始化这样的一个对象，就相当于找到了这个门的把手，通过这个句柄就可以操作门背后节点的一系列东东

```C++
//1.引入头文件
#include "ros/ros.h"
#include "package_name/message_name"
#include "package_name/service_name"
int main(int argc, char *argv[])
{  
    setlocale(LC_ALL,"");//解决中文乱码

    //2.初始化ros节点,节点名称要保证唯一
    ros::init(argc,argv,"Node_name"); 

    //3.创建节点句柄
    ros::NodeHandle nh;
    
    return 0;
}
```

一般创建的节点句柄对象叫做 `nh`
##### 4. 如果有必要使用回调函数

如果需要使用回调函数，那么就要在程序最后运行的代码末尾处最后添加一个 `ros::spin()` 语句

如果最后程序终止在一个循环里，添加的语句就应该是 `ros::spinOnce()`

#### Python
##### 1. 导入模块

先是要 `import rospy`，目的是导入这个 ROS 的 python 接口

然后再导入相应的消息类型和服务类型，格式为
```python
from package_name.msg import message_name
from package_name.srv import service_name
```

一个完整的例子如下
```python
#!/usr/bin/env python
# 每个 Python ROS 节点的最开头都有这个声明。这是为了确保脚本作为Python 脚本执行

import rospy 
# 导入 rospy 以便支持使用 ros 的 python 接口编写节点 
from std_msgs.msg import String
# 从对应的软件包导入消息类型
# 要使用 std_msg/String 类型，就要先用这样的语句导入 from std_msgs.msg import String
```


##### 2. 初始化 ROS 节点

使用 `rospy.init_node` 这个函数进行节点初始化

```python
rospy.init_node('Node_name', anonymous=True)
# 这一句非常重要，它把该节点的名称告诉了 rospy
# 只有 rospy 掌握了这一信息后，才会开始与 ROS 主节点进行通信
# anonymous = True 会让名称末尾添加随机数
# 来确保节点具有唯一的名称
# 如果两个节点命名空间一样，节点名称也一样，那么ROS会默认杀死先前启动的节点，为了避免节点被顶掉，要使用 anonymous 这个参数为节点名字编号生成随机数
```

和 `Cpp` 不同的是，`python` 没有必要创建节点句柄来实现对节点参数的配置，直接使用导入的 `rospy` 模块的方法和类型就行



##### 3. 如果有必要使用回调函数

在程序最后运行的代码末尾处使用 `rospy.spin` 
`rospy.spin` 只是不让这个节点退出，直到节点被明确关闭。与 `roscpp` 不同，`rospy.spin` 不影响程序的回调函数调用，因为 `rospy` 给了它们自己的线程



### 编写服务端节点

#### Cpp

注意在编写节点的具体逻辑前，一定要先初始化 ROS 接口
也就是说要包含相关头文件，然后初始化节点，再创建好句柄对象
##### 创建服务对象

利用句柄创建一个类型为 `ros::ServiceServer` 的服务对象，这个对象一般叫做 `serivce`

具体就是利用 `ros::NodeHandle` 类型的句柄对象 `nh` 的 `advertiseService` 函数
```cpp
ros::ServiceServer service = nh.advertiseService("service_communication_name", callBack);
# 第一个参数是服务通信名称
# 第二个参数是回调函数，通常叫做 ...callBack 或 do...
```
这里并不用之前传入服务类型，而是通过传入的回调函数声明格式那里体现服务类型的要求


通常处理服务请求的数据是在回调函数封装完的，所以先在程序的末尾提前写下 `ros::spin()` 或者 `ros::spinOnce()`

```c++
// 1.包含头文件
#include <ros/ros.h>
#include "package_name/message_name.h"

int main(int argc, char **argv)
{
    // 2.初始化 ROS 节点
    ros::init(argc, argv, "listener"); 

    // 3.创建节点句柄
    ros::NodeHandle nh;

    //4.创建服务对象
    ros::ServiceServer service 
    = n.advertiseService("service_communication_name", callBack);
    ROS_INFO("Server Ready.");

    ros::spin();
    //进入ROS的事件循环
    //等待接收消息并调用相应的回调函数进行处理
 
    return 0;
 }
```

##### 回调函数处理请求并产生响应

在服务通信中，回调函数的类型是 `bool`，参数分别是服务类型的请求部分和响应部分对象。
函数传参方式是传入地址，所以对形参做的事情是真的会改变相应的对象的，也就是说可以通过句点调用对应对象的属性的方式实际上操作服务数据的请求部分和响应部分
```c++
bool callBack(package_name::service_name::Request &req
              package_name::service_name::Response &res)
              {
                  data_1 = req.data_1;
                  data_2 = req.data_2;
                  res.result = data_1 + data_2;
                  /*· · ·*/

	              return true;
              }
```
如果回调函数正常完成处理，那么就返回布尔值 `true`

##### 示例程序

```c++
#include "ros/ros.h"
#include "beginner_tutorials/AddTwoInts.h"
bool add(beginner_tutorials::AddTwoInts::Request  &req,
         beginner_tutorials::AddTwoInts::Response &res)
{
  res.sum = req.a + req.b;
  ROS_INFO("request: x=%ld, y=%ld", (long int)req.a, (long int)req.b);
  ROS_INFO("sending back response: [%ld]", (long int)res.sum);
  return true;
}
int main(int argc, char **argv)
{
  ros::init(argc, argv, "add_two_ints_server");
  ros::NodeHandle n;
  ros::ServiceServer service = n.advertiseService("add_two_ints", add);
  ROS_INFO("Ready to add two ints.");
  ros::spin();
  return 0;
}
```
#### Python

注意在编写节点的具体逻辑前，一定要先初始化 ROS 接口
也就是说要导入相关模块，然后初始化节点。注意这里导入的模块多了请求和响应
```python
# 1.导入相关模块
#!/usr/bin/env python
from __future__ import print_function
from beginner_tutorials.srv import AddTwoInts,AddTwoIntsResponse
import rospy
```
##### 创建服务对象

和 `cpp` 不一样，使用 `rospy` 的 `Service` 类构造函数需要直接传入服务类型名称才可以构造一个 `Service` 类型的服务对象出来
```python
service = rospy.Service('service_communication_name', service_name, callBack)
# 第一个参数是服务通信名称
# 第二个参数是服务类型的名称
# 如果服务类型是 package_name/servive_name 那么名称就是 servive_name
# 第三个参数是回调函数，通常叫做 ...callBack 或 do...
```
通常处理服务请求的数据是在回调函数封装完的，所以先在程序的末尾提前写`rospy.spin()` 
##### 回调函数处理请求并产生响应

和 `cpp` 不同，`python` 的回调函数接受的参数只有自己先预设的请求对象
然后在回调函数里面通过句点调用请求对象里面的数据进行处理后，记得使用 `service_nameResponse` 类的构造函数创建请求对象并返回
```python
# 1.导入相应模块
import rospy
from package_name.srv import service_name, service_nameRequest,service_nameResponse
# 注意了，导入模块的时候要多导入 Request 和 Response 部分

def callBack(req): # req 是自己先预设的请求对象
    
    # 处理请求的数据
    sum = req.data_1 + req.data_2
    
	# 利用构造函数创建响应对象
	res = service_nameResponse()
	res.result = sum
	# 这种先声明再初始化的方法当响应对象的字段比较多时比较好用

	return res
```


##### 示例程序

```python
# 1.导入相关模块
#!/usr/bin/env python
from __future__ import print_function
from beginner_tutorials.srv import AddTwoInts,AddTwoIntsResponse
import rospy

# 5.回调函数处理请求数据
def handle_add_two_ints(req):
    print("Returning [%s + %s = %s]"%(req.a, req.b, (req.a + req.b)))
    return AddTwoIntsResponse(req.a + req.b)

def add_two_ints_server():
	# 2.初始化 ROS 节点
    rospy.init_node('add_two_ints_server')

	# 3.创建了服务对象
    s = rospy.Service('add_two_ints', AddTwoInts, handle_add_two_ints)
    print("Ready to add two ints.")
    
    # 4.等待回调函数
    rospy.spin()
if __name__ == "__main__":
    add_two_ints_server()
```


### 编写客户端节点

#### Cpp

注意在编写节点的具体逻辑前，一定要先初始化 ROS 接口
也就是说要包含相关头文件，然后初始化节点，再创建好句柄对象
```cpp
//1.引入头文件
#include "ros/ros.h"
#include "package_name/message_name"
#include "package_name/service_name"
#include <cstdlib> //读取命令行参数
```
##### 需要进行参数个数检查

```cpp
//1.引入头文件
#include "ros/ros.h"
#include "package_name/message_name"
#include "package_name/service_name"
#include <cstdlib> //读取命令行参数

int main(int argc, char *argv[])
{  
	setlocale(LC_ALL,"");//解决中文乱码
	
	if (argc != 3) 
	// 这是直接使用 rosrun 的参数个数
	/*
	* 0: rosrun
	* 1: package_name
	* 2: argument_1
	* 3: argument_2
	*/
	// 如果使用 roslaunch 要进行修改
    {
      ROS_INFO("该服务需要两个参数");
      return 1;
    }

    // 2.初始化 ROS 节点
	ros::init(argc, argv, "Node_name");
	// 3.创建节点句柄
	ros::NodeHandle nh;
}
```
##### 创建客户端对象

利用句柄创建一个类型为 `ros::ServiceClient` 的客户端对象，这个对象一般叫做 `client`

具体就是利用 `ros::NodeHandle` 类型的句柄对象 `nh` 的 `serviceClient<>` 模板函数

这个模板函数的模板就是需要请求的服务类型，格式为
```C++
ros::ServiceClient client = nh.serviceClient<package_name::service_name>("servicecommunication_name")
```


```C++
//1.引入头文件
#include "ros/ros.h"
#include "package_name/message_name"
#include "package_name/service_name"
#include <cstdlib> //读取命令行参数

int main(int argc, char *argv[])
{  
    setlocale(LC_ALL,"");//解决中文乱码

	//进行参数个数检查
    if (argc != 3) 
    // 这是直接使用 rosrun 的参数个数
    /*
    * 0: rosrun
    * 1: package_name
    * 2: argument_1
    * 3: argument_2
    */
    // 如果使用 roslaunch 要进行修改
    {
      ROS_INFO("该服务需要两个参数");
      return 1;
    }

    // 2.初始化 ROS 节点
    ros::init(argc, argv, "Node_name");
    // 3.创建节点句柄
    ros::NodeHandle nh;

    // 4.创建客户端对象
	ros::ServiceClient client = nh.serviceClient<package_name::service_name>("servicecommunication_name");
	
}
```
##### 创建服务对象并配置请求部分的数据

之前包含的服务文件头文件就有声明了对应的服务类型，使用下面这样的格式来为服务类型创建对应的服务对象。然后再通过句点调用对象属性配置请求部分的数据
```C++
package_name::service_name service_object;

service_object.request.data_1 = atoll(argv[1]);
service_object.request.data_2 = atoll(argv[2]);
```

```C++
//1.引入头文件
#include "ros/ros.h"
#include "package_name/message_name"
#include "package_name/service_name"
#include <cstdlib> //读取命令行参数

int main(int argc, char *argv[])
{  
    setlocale(LC_ALL,"");//解决中文乱码

	//进行参数个数检查
    if (argc != 3) 
    // 这是直接使用 rosrun 的参数个数
    /*
    * 0: rosrun
    * 1: package_name
    * 2: argument_1
    * 3: argument_2
    */
    // 如果使用 roslaunch 要进行修改
    {
      ROS_INFO("该服务需要两个参数");
      return 1;
    }

    // 2.初始化 ROS 节点
    ros::init(argc, argv, "Node_name");
    // 3.创建节点句柄
    ros::NodeHandle nh;

    // 4.创建客户端对象
	ros::ServiceClient client = nh.serviceClient<package_name::service_name>("servicecommunication_name");

	// 5.创建服务对象并配置请求部分的数据
	package_name::service_name service_object;
 
    service_object.request.data_1 = atoll(argv[1]);
    service_object.request.data_2 = atoll(argv[2]);	
}
```
##### 等待服务准备并发送请求，接受响应

第一步是等待服务是否启动成功，使用了一个阻塞式函数，只有服务启动成功后才会继续执行
```cpp
ros::service::waitForService("servicecommunication_name");
// 或者: client.waitForExistence();
```

然后设置一个标志位来接收返回值，用客户端对象 `client` 的 `call` 方法发送服务对象来发送服务请求，并查看服务是否正常返回
```c
bool flag = client.call(service_object);
if(flag)
{
	//如果服务成功返回，此时服务对象的响应部分已经有值了
	service_object.response.result;//是有值的 
}
else
{
	ROS_INFO("请求处理失败");
	return 1;
}
```

```C++
//1.引入头文件
#include "ros/ros.h"
#include "package_name/message_name"
#include "package_name/service_name"
#include <cstdlib> //读取命令行参数

int main(int argc, char *argv[])
{  
    setlocale(LC_ALL,"");//解决中文乱码

	//进行参数个数检查
    if (argc != 3) 
    // 这是直接使用 rosrun 的参数个数
    /*
    * 0: rosrun
    * 1: package_name
    * 2: argument_1
    * 3: argument_2
    */
    // 如果使用 roslaunch 要进行修改
    {
      ROS_INFO("该服务需要两个参数");
      return 1;
    }

    // 2.初始化 ROS 节点
    ros::init(argc, argv, "Node_name");
    // 3.创建节点句柄
    ros::NodeHandle nh;

    // 4.创建客户端对象
	ros::ServiceClient client 
	= nh.serviceClient<package_name::service_name>("servicecommunication_name");

	// 5.创建服务对象并配置请求部分的数据
	package_name::service_name service_object;
 
    service_object.request.data_1 = atoll(argv[1]);
    service_object.request.data_2 = atoll(argv[2]);

	// 6.等待服务准备并发送请求，接受响应
	bool flag = client.call(service_object);
    if(flag)
    {
       //如果服务成功返回，此时服务对象的响应部分已经有值了
       service_object.response.result;//是有值的 
    }
    else
    {
       ROS_INFO("请求处理失败");
       return 1;
    }
}
```
##### 示例程序

注意 `cpp` 源文件要放在软件包的 `src` 目录下面

```c++
#include "ros/ros.h"
#include "beginner_tutorials/AddTwoInts.h"
#include <cstdlib>
int main(int argc, char **argv)
{
  ros::init(argc, argv, "add_two_ints_client");
  if (argc != 3)
  {
    ROS_INFO("usage: add_two_ints_client X Y");
    return 1;
  }
  ros::NodeHandle nh;
  ros::ServiceClient client = nh.serviceClient<beginner_tutorials::AddTwoInts>("add_two_ints");
  beginner_tutorials::AddTwoInts srv;
  srv.request.a = atoll(argv[1]);
  srv.request.b = atoll(argv[2]);
  if (client.call(srv))
  {
    ROS_INFO("Sum: %ld", (long int)srv.response.sum);
  }
  else
  {
    ROS_ERROR("Failed to call service add_two_ints");
    return 1;
  }
  return 0;
}
```

#### Python

注意在编写节点的具体逻辑前，一定要先初始化 ROS 接口
也就是说要导入相关模块，然后初始化节点。注意这里导入的模块多了请求和响应
```python
# 1.导入相关模块
#!/usr/bin/env python
from __future__ import print_function
from beginner_tutorials.srv import * # 使用通配符一次性导入多个模块
import rospy
import sys # 实现检测命令行参数
```

##### 需要进行参数个数检查

```python
# 1.导入相关模块
#!/usr/bin/env python
from __future__ import print_function
from beginner_tutorials.srv import * # 使用通配符一次性导入多个模块
import rospy
import sys # 实现检测命令行参数

if __name__ == "__main__":

    # 检查命令行参数个数
    if len(sys.argv) != 3:
        rospy.logerr("请正确提交参数")
        sys.exit(1)
    
	# 2.初始化 ROS 节点
	rospy.init_node('Node_name', anonymous=True)
```

##### 创建客户端对象

与 `cpp` 不同，ROS 的 `python` 接口已经有 `rospy` 模块了，不需要再创建一个句柄对象并调用句柄对象的模板函数来声明并初始化客户端对象

这里直接用句点 `rospy.ServiceProxy()` 去调用 `rospy` 的 `ServiceProxy` 函数来初始化客户端对象，还是一般叫做 `client`
```python
client = rospy.ServiceProxy("servicecommunication_name", service_name)
```
这里只用传入服务名称，并不需要像模板函数那样严格的指定软件包和服务名称

对比一下 `cpp` 的版本，可以看出来客户端对象的构造函数一定要有的参数就是服务通信的名称，以及服务类型
```C++
ros::ServiceClient client = nh.serviceClient<package_name::service_name>("servicecommunication_name")
```

```python
# 1.导入相关模块
#!/usr/bin/env python
from __future__ import print_function
from beginner_tutorials.srv import * # 使用通配符一次性导入多个模块
import rospy
import sys # 实现检测命令行参数

if __name__ == "__main__":

    # 检查命令行参数个数
    if len(sys.argv) != 3:
        rospy.logerr("请正确提交参数")
        sys.exit(1)
    
	# 2.初始化 ROS 节点
	rospy.init_node('Node_name', anonymous=True)

	# 3.创建客户端对象
	client = rospy.ServiceProxy("servicecommunication_name", service_name)
```


##### 创建请求对象并配置数据

之前导入的服务文件头模块就有对应的服务类型信息，和 `cpp` 直接声明服务对象不同，`python` 是用一个构造函数来为服务的请求部分创建对象，这个构造函数的名称就是 `service_nameRequest` 然后再通过句点调用对象属性配置请求部分的数据
```python
req = service_nameRequest()

req.data_1 = sys.argv[1]
req.data_2 = sys.argc[2]
```

对比 `cpp` 直接声明服务对象，包含请求和响应部分
```C++
package_name::service_name service_object;

service_object.request.data_1 = atoll(argv[1]);
service_object.request.data_2 = atoll(argv[2]);
```

```python
# 1.导入相关模块
#!/usr/bin/env python
from __future__ import print_function
from beginner_tutorials.srv import * # 使用通配符一次性导入多个模块
import rospy
import sys # 实现检测命令行参数

if __name__ == "__main__":

    # 检查命令行参数个数
    if len(sys.argv) != 3:
        rospy.logerr("请正确提交参数")
        sys.exit(1)
    
	# 2.初始化 ROS 节点
	rospy.init_node('Node_name', anonymous=True)

	# 3.创建客户端对象
	client = rospy.ServiceProxy("servicecommunication_name", service_name)

	# 4.创建请求对象并配置数据
	req = service_nameRequest()
    req.data_1 = sys.argv[1]
    req.data_2 = sys.argc[2]
```
##### 等待服务准备并发送请求，接受响应

第一步是等待服务是否启动成功，使用了一个阻塞式函数，只有服务启动成功后才会继续执行
```python
# rospy.wait_for_service("servicecommunication_name")
client.wait_for_service()
```

然后是用客户端对象 `client` 的请求方法把请求对象发送出去，并用一个变量来接住返回的响应对象
```python
res = client.call(req)
# 此时 res 已经是响应对象，有处理后的数据值了
rospy.loginfo("响应结果 %d", res.result)
```

对比 `cpp` 的版本
```c
bool flag = client.call(service_object);
if(flag)
{
	//如果服务成功返回，此时服务对象的响应部分已经有值了
	service_object.response.result;//是有值的 
}
else
{
	ROS_INFO("请求处理失败");
	return 1;
}
```

```python
# 1.导入相关模块
#!/usr/bin/env python
from __future__ import print_function
from beginner_tutorials.srv import * # 使用通配符一次性导入多个模块
import rospy
import sys # 实现检测命令行参数

if __name__ == "__main__":

    # 检查命令行参数个数
    if len(sys.argv) != 3:
        rospy.logerr("请正确提交参数")
        sys.exit(1)
    
	# 2.初始化 ROS 节点
	rospy.init_node('Node_name', anonymous=True)

	# 3.创建客户端对象
	client = rospy.ServiceProxy("servicecommunication_name", service_name)

	# 4.创建请求对象并配置数据
	req = service_nameRequest()
    req.data_1 = sys.argv[1]
    req.data_2 = sys.argc[2]

	# 5.等待服务准备完毕，发送请求并接受响应
	client.wait_for_service()
	res = client.call(req)
    # 此时 res 已经是响应对象，有处理后的数据值了
    rospy.loginfo("响应结果 %d", res.result)
```
##### 示例程序

注意 `python` 源文件要放在对应软件包的 `scripts` 目录下

```python
#!/usr/bin/env python
from __future__ import print_function
import sys
import rospy
from beginner_tutorials.srv import *

def add_two_ints_client(x, y):
    rospy.wait_for_service('add_two_ints')
    try:
        add_two_ints = rospy.ServiceProxy('add_two_ints', AddTwoInts)
        resp1 = add_two_ints(x, y)
        return resp1.sum
    except rospy.ServiceException as e:
        print("Service call failed: %s"%e)

def usage():
    return "%s [x y]"%sys.argv[0]
    
if __name__ == "__main__":
    if len(sys.argv) == 3:
        x = int(sys.argv[1])
        y = int(sys.argv[2])
    else:
        print(usage())
        sys.exit(1)
    print("Requesting %s+%s"%(x, y))
    print("%s + %s = %s"%(x, y, add_two_ints_client(x, y)))
```

### 构建节点并检验

注意 `cpp` 源文件要放在软件包的 `src` 目录下面
注意 `python` 源文件要放在对应软件包的 `scripts` 目录下

对于需要编译成节点的 `cpp` 源文件，就需要回去软件包的 `CMakeLists.txt` 文件依次添加 `add_executable`、`add_dependencies` 还有 `target_link_libraries`
```CMake
add_executable(executable_file_name_1 src/source_1.cpp)
add_executable(executable_file_name_2 src/source_2.cpp)

add_dependencies(executable_file_name_1 ${PROJECT_NAME}_generate_messages_cpp)
add_dependencies(executable_file_name_2 ${PROJECT_NAME}_generate_messages_cpp)


target_link_libraries(executable_file_name_1
  ${catkin_LIBRARIES}
)
target_link_libraries(executable_file_name_2
  ${catkin_LIBRARIES}
)
```

对于要编译成节点的 `python` 源文件，需要将以下内容添加到软件包的 `CMakeLists.txt` 文件，进行 `catkin_install_python()` 调用。这样可以确保正确安装 `Python` 脚本，并使用合适的 `Python` 解释器
```cmake
catkin_install_python(PROGRAMS 
					  scripts/Node_name_1.py
					  ...
					  scripts/Node_name_N.py
  DESTINATION ${CATKIN_PACKAGE_BIN_DESTINATION}
)
```


即使是 `Python` 节点也必须使用 `catkin_make`。这是为了确保能为自己创建的消息和服务文件自动生成 `Python` 代码

回到 `catkin` 工作空间，然后运行 `catkin_make` 


# ROS 终端工具

善用 `-h` 选项，子命令也可以用
## rospack 获取软件包信息

`rospack [...] [package_name]` 可以获取到软件包相关的信息（比如软件包间的依赖关系）

在 `package.xml` 里面就存放了在使用 `catkin_create_pkg` 命令时的创建的软件包依赖关系
```xml
<package>
...
 <buildtool_depend>catkin</buildtool_depend>
 <build_depend>roscpp</build_depend>
 <build_depend>rospy</build_depend>
 <build_depend>std_msgs</build_depend>
...
</package>
```

在很多情况下，一个被依赖的包还会有它自己的依赖关系，比如，`rospy` 就有其它依赖包

可以使用 `rospack depends1 [package_name]` 查看软件包的第一级依赖
或者使用 `rospack depends [package_name]` 递归查看所有嵌套的依赖包


## roscd 切换到软件包的路径

`roscd [package_name]` 可以切换到某一个软件包，也支持切换到软件包的子目录中，具体的使用例子如下
```bash
roscd roscpp
roscd roscpp/cmake # cmake 是 roscpp 软件包的一个子目录
```

注意软件包的路径要包含在 `ROS_PACKAGE_PATH` 环境变量中，也就是要在已经 `source` 过的工作区中


`ROS_PACKAGE_PATHS` 这个环境变量会包含那些认证的 ROS 软件包的路径，并且各个路径之间用 `:` 分隔开来，可以自己添加自己的路径，只要遵守 `:` 分隔就行

### roscd log 切换到日志文件的路径

`roscd log` 会进入存储 ROS 日志文件的目录，注意，如果没有执行过任何 ROS 程序，系统会报错说该目录不存在

## rosls 显示软件包路径下的东东 

`rosls [package_name]` 允许你直接按软件包的名称执行 `ls` 命令而不必输入绝对路径，也支持显示软件包的子目录

```bash
rosls roscpp
rosls roscpp/cmake # cmake 是 roscpp 软件包的一个子目录
```

按一下 `tab` 键可以自动补全命令剩余部分
`rosls` 按两下 `tab` 键可以输出可能的结果


## roscore = 主节点 + rosout节点 + 参数服务器

`roscore` 是在运行所有ROS程序前首先要运行的命令。运行完它之后，会启动一个叫做 `rosout` 的节点。这个节点用于收集和记录节点的调试输出，类似于 `c++` 的 `stdout/stderr`


## rosnode 获取当前正在运行的节点

`rosnode list` 会列出所有正在运行的节点

`rosnode info [node_name]` 会返回某个指定节点的相关信息，比如
```shell
rosnode info /rosout
# 下面是返回的节点信息
------------------------------------------------------------------------
    Node [/rosout]
    Publications:
     * /rosout_agg [rosgraph_msgs/Log]
    Subscriptions:
     * /rosout [unknown type]
    Services:
     * /rosout/get_loggers
     * /rosout/set_logger_level
    contacting node http://machine_name:54614/ ...
    Pid: 5092
```
这个可以看出来实际上它有发布了一个 `/rosout_agg` 话题

`rosnode ping [node_name]` 可以来测试正在运行的节点连接正不正常

## rosrun 直接运行软件包对应的节点

`rosrun` 使用对应的软件包名直接运行软件包内的节点，而不需要知道包的路径。它的用法就是 `rosrun [package_name] [node_name]`

在使用 `rosrun` 的时候可以通过选项来重新指定节点的名称，就比如
```shell
rosrun turtlesim turtlesim_node __name:=my_turtle
# 这样运行的节点就从原本的名称 turtlesim_node 改变成 my_turtle
```



## rqt_graph 查看系统的节点信息

`rqt_graph` 用动态的图显示了系统中正在发生的事情
使用方法是打开一个新终端然后运行 `rosrun rqt_graph rqt_graph`
## rostopic 获取话题信息

可以使用 `-h` 选项获取子命令的帮助信息。或者是在输入 `rostopic` 之后双击 `tab` 输出可能的子命令

`rostopic echo [topic]` 可以在屏幕显示发布到某个话题的消息

`rostopic list` 能够列出当前已被订阅和发布的所有话题

`rostopic pub [topic] [msg_type] [args]` 可以把数据以指定消息的类型发布到当前某个正在广播的话题上。注意了，消息的类型是像 `package_name/message_name` 这样的形式

```shell
rostopic pub -1 /turtle1/cmd_vel geometry_msgs/Twist -- '[2.0, 0.0, 0.0]' '[0.0, 0.0, 1.8]'

# rostopic pub 这条命令将消息发布到指定的话题

# -1 这个选项会让 rostopic 只发布一条消息，然后退出
# -r 1 选项是以 1 Hz 的频率源源不断发布消息

# /turtle1/cmd_vel 这是要发布到的话题的名称

# geometry_msgs/Twist 这是发布到话题时要使用的消息的类型

# -- 这一选项（两个破折号）用来告诉选项解析器，表明之后的是参数，而不是选项。如果参数前有破折号（-）比如负数，那么这是必需的

# '[2.0, 0.0, 0.0]' '[0.0, 0.0, 1.8]' 这些参数实际上使用的是YAML语法
```

`rostopic hz [topic]` 这个命令可以查看话题给订阅者发布消息的频率 
## rosmsg 查看消息类型的结构

`rosmsg show [msg_type]` 就可以查看这个消息类型对应的 `msg` 文件对应有多少字段，也就是说这个结构体里面有多少个成员变量

表达消息类型的时候一般要用两个部分，一个是定义消息的软件包，另一个就是 `msg` 文件的名称，比如 `std_msg/String`。实际上，在终端使用 `rosmsg` 查看消息类型和在 `cpp` 源文件中包含消息类型时，遵循的都是这样的格式。
```shell
rosmsg show geometry_msgs/Twist

# geometry_msgs/Vector3 linear
#      float64 x
#      float64 y
#      float64 z
# geometry_msgs/Vector3 angular
#      float64 x
#      float64 y
#      float64 z
```

可以使用 `rosmsg show package_name/message_name` 来显示消息对应 `msg` 文件的内容。如果忘记了消息属于哪个软件包，也可以省略软件包名称直接使用 `rosmsg show <message_name>`
## rqt_plot 为发布在话题数据绘制函数图像

`rosrun rqt_plot rqt_plot` 这个命令会弹出一个新窗口，可以在左上角的文本框里面添加任何想要绘制的话题。在里面输入对应的`话题/成员变量`后，之前不能按下的加号按钮将会变亮

## rosservice 获取服务信息

`rosservice list` 输出活跃服务的信息，直接使用这个命令会列出当前 ROS 图里面所有的活跃的服务名称

`rosservice type [service]` 可以确定服务的类型，注意了，服务的类型是像 `package_name/service_name` 这样的形式。然后还可以配合 `rosservice type [service] | rossrv show` 来查看服务类型的具体结构，有多少参数之类的

`rosservice call [service] [args]` 按照服务类型指定的参数调用服务，注意一定要先知道服务的类型来确定参数的个数有无和值

`rosservice find` 按服务的类型查找服务
`rosservice uri` 输出服务的ROSRPC uri
## rossrv 查看服务类型的具体结构

`rssrv show [srv_type]` 就可以查看对应服务类型的结构了，具体就是请求部分和响应部分有多少参数，注意了，服务的类型是像 `package_name/service_name` 这样的形式


## rosparam 查看和配置参数服务器

`rosparam` 能在 ROS 参数服务器（Parameter Server）上存储和操作数据。

参数服务器能够存储整型（integer）、浮点（float）、布尔（boolean）、字典（dictionaries）和列表（list）等数据类型。

`rosparam` 使用 `YAML` 标记语言的语法。一般而言，`YAML` 的表述很自然：`1` 是整型，`1.0` 是浮点型，`one` 是字符串，`true` 是布尔型，`[1, 2, 3]` 是整型组成的列表，`{a: b, c: d}` 是字典。

`rosparam list` 可以列出参数服务器上当前存在的所有参数名称，注意，参数的名称是像 `/.../.../argument_1` 这样的，前面的第一级 `/` 代表的是参数服务器的根目录，后面的子目录是这个参数的命名空间 `namespace` 类似 `cpp` 中命名空间的作用

`rosparam get [param_name]` 可以通过参数的名称获取该参数对应的值，如果直接输入的参数名字是根目录 `/` ，也就是输入 `rosparam get /` 可以得到参数服务器的所有内容

`rosparam set [param_name]` 可以通过参数的名称设置对应的参数值

`rosparam dump [file_name] [namespace]` 可以将此时参数服务器的所有参数都写入自己定义的 `yaml` 文件中，比如 `rosparam dump params.yaml`

`rosparam load [file_name] [namespace]` 这个命令就是从 `yaml` 文件中读取参数信息并载入到参数服务器中，这里如果不加上命名空间的话就是之前存到根目录 `/` 以后，如果加上命名空间就是存到对应的命名空间 ` /namespace/... ` 以后

`rosparam delete` 可以用来删除参数
## roslaunch 启动软件包的一系列节点

`roslaunch` 可以用来启动定义在launch（启动）文件中的一系列节点，最好 `roscd` 去到最主要的 ROS 软件包下面的路径去新建一个 `launch` 文件夹，专门存放各种 `.launch` 文件

虽然存放 `.launch` 文件的目录不一定非要命名为 `launch` 文件夹，事实上都不用非得放在目录中，但是规范地管理还是有必要的

注意你放置的.launch文件一定要在某个软件包下面，因为 `roslaunch` 命令会自动查找经过的包并检测可用的 `.launch` 文件

接着使用 ` roslaunch [package] [filename.launch]` 就好了

# 经验

`Python` 中的格式化操作符类似于 `C` 语言中的 `printf` 函数的格式化功能。下面是一些不同情况下的格式化操作符和替换的例子：展示了如何使用不同的格式化操作符（如 `%s` 用于字符串，`%d` 用于整数，`%.2f` 用于浮点数并保留两位小数）来替换字符串中的占位符。

```python
# 数字替换
number = 10
formatted_string = "The number is %d" % number
print(formatted_string)  # 输出: The number is 10

# 浮点数替换，控制小数点后的位数
pi_value = 3.14159
formatted_string = "The value of pi is %.2f" % pi_value
print(formatted_string)  # 输出: The value of pi is 3.14

# 多个字符串替换
name = "Alice"
age = 30
formatted_string = "Name: %s, Age: %d" % (name, age)
print(formatted_string)  # 输出: Name: Alice, Age: 30

# 使用字典进行格式化
data = {'name': 'Bob', 'age': 25}
formatted_string = "Name: %(name)s, Age: %(age)d" % data
print(formatted_string)  # 输出: Name: Bob, Age: 25
```

具体的用到的 ROS 接口自己在 Vscode 里面跳转查看
