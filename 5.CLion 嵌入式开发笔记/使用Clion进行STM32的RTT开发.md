<style>
.red {
  color: #ff0000;
}
.green {
  color:rgb(10, 162, 10);
}
.blue {
  color:rgb(17, 0, 255);
}
</style>





# <span class="green"><font size=4>一、STM32-RTT构建</font></span>
```bash
create_at：2025/06/09
aliases：使用Clion进行STM32的RT-Thread开发
```
## <font size=2>1.RTT Studio 与 Clion 的差异</font>
```bash
1.Clion使用CMake,Make或者Makefile来构建项目
2.RTT使用scons来构建项目
```
## <font size=2>2.使用CLion构建RTT工程关心的核心问题</font>
```bash
1.用什么编译器：CLion自动捆绑或者自己安装的arm编译器（arm-none-eabi-gcc.exe）
2.编写CMakeLists.txt需要关注的要点
  ① 关心哪些源文件需要被编译 
  ② 关心头文件的搜索地址
  ③ 关心启动汇编文件
  ④ 关心链接文件
```


# <span class="green"><font size=4>二、建立以工作区为形式的开发环境</font></span>
## <font size=2>1.构建目录大纲</font>
<font size=2>为了方便将项目代码最小化管理，将rt-thread以及stm32系列单片机的Hal库文件均放置在rtos文件夹下，作为整个工作区的依赖库，然后在bsp文件夹下新建项目文件，以达到模仿rt-thread官方制作BSP板级项目的效果。新建目录大纲如下所示。<br>这样可以达到bsp文件夹内的所有项目可以共享上层路径中的RTOS、APP、TOOLS、DOCS的资源。</font>
```bash
E:.
├─apps                        # 存放上层app相关文件
├─bsp                         # 存放项目源码
│  └─project_demo1
├─docs                        # 存放项目文档
├─rtos                        # 存放一些库源码
│  ├─packages                 # rt-thread 的 组件 源码
│  ├─platform                 # 跨平台软件包源码
│  │  └─stm32_libraries
│  │      ├─STM32F1xx_HAL
│  │      ├─STM32F4xx_HAL
│  │      ├─STM32F7xx_HAL
│  │      └─STM32H7xx_HAL
│  └─rt-thread                # rt-thread 源码
└─tools                       # 工具插件脚本类源码
```
![目录大纲](./images/CLION1.png)


## <font size=2>2.下载必要的库文件以及工具</font>
<font size=2>使用CLion的CMake进行STM32项目构建，在此之前需要先下载几个必要的库文件，分别是rt-thread操作系统源码、STM32_HAL库源码、rt-thread的软件组件包源码等。除此之外，需要注意的是CLion使用的CMake是基于gcc编译器，而RTT编译使用的Scons是基于python编译器进行的。因此，需要额外安装rtt官方的env工具，将Cmake与Scons联合。</font>

<font size=2>[rt-thread官方env工具下载链接](https://www.rt-thread.org/download.html#download-rt-thread-env-tool "rtt官方链接")

[rt-thread的操作系统源码github下载链接](https://github.com/RT-Thread/rt-thread "github链接")

[rt-thread的packages源码github下载链接](https://github.com/RT-Thread/packages "packages下载链接")

[STM32源码github下载链接](https://github.com/orgs/STMicroelectronics/repositories?type=all
 "packages下载链接")</font>

## <font size=2>3.Clion的RTT环境搭建与移植</font>

<font size=2>首先，按照目录大纲将RT-Thread的基础目录搭建出来，然后按照上面的链接去下载源码，并将库源码全部放置于rtos文件夹中。<br>如上述目录大纲所示，在bsp文件夹下新建一个project_demo1文件夹用于存放项目源码，并到rt-thread的文件内部去找官方的bsp板级支持包，找到如图所示的文件内容，并粘贴到项目文件夹中。</font>
![移植文件](./images/CLION3.png)

<font size=2>然后，点击鼠标右键，点击弹出菜单选项中的"ConEmu Here"在这个项目文件夹路径中打开env工具。</font>



# <span class="green"><font size=4>三、scons --dist工具快速生成</font></span>





