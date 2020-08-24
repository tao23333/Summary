# 简介

> Anaconda指的是一个开源的Python发行版本，其包含了conda、Python等180多个科学包及其依赖项；
>
> 1、提供了包管理功能，Windows平台安装第三方包经常失败的场景得以解决
>
> 2、提供环境管理的功能，解决了多版本Python并存，切换的问题



# 下载

清华大学开源镜像：https://mirrors.tuna.tsinghua.edu.cn/anaconda/archive/

运行命令将包管理源修改为国内：

```bash
conda config --add channels https://mirrors.tuna.tsinghua.edu.cn/anaconda/pkgs/free/
conda config --set show_channel_urls yes
```

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

# 环境管理

> conda是Anaconda下用于包管理和环境管理的工具，功能上类似pip和vitualenv的结合；
>
> 安装成功后conda会默认添加到环境变量中，因此可在cmd中直接运行conda

## 命令相关

> 查看conda版本：conda -V
>
> 查看所有已经安装的环境(*代表当前所处的环境)：conda info -e
> ![img](https://img-blog.csdnimg.cn/20200823224332609.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)​
>
> 
>
> 创建基于python3.7虚拟环境python37（--name可写为-n）：
> ![img](https://img-blog.csdnimg.cn/20200823224739426.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)​
>
> 
>
> 激活/切换虚拟环境(Windows下)：activate python37
> ![img](https://img-blog.csdnimg.cn/20200823225026317.png)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)​
>
> 
>
> 退出当前虚拟环境：deactivate 
>
> 删除虚拟环境：conda remove -n python37 --all

# 包管理

> conda的包管理功能与pip类似，当然选择pip来进行包管理也是没有问题的
>
> 查看已安装的包：conda list
>
> 当前环境安装matplotlib：conda install matplotlib
> 指定环境中安装包：conda install -n 环境名 包名
>
> 包更新：conda update matplotlib
>
> 当前环境包删除：conda remove matplotlib
> 指定环境包删除：conda remove -n 环境名 包名

## 注意：

> - 在conda中anything is a package；conda本身可以看作是一个包，python环境可以看作是一个包，anaconda也可以看作是一个包，因此除了普通的第三方包支持更新之外，这3个包也支持
>   - **更新conda本身：**conda update conda
>   - **更新anaconda应用：**conda update anaconda
>   - **更新python：**conda update python
> - pip只是包管理器，无法对环境进行管理。因此如果想在指定环境中使用pip进行安装包，则需要先切换到指定环境中，再使用pip命令安装包
> - pip无法更新python，因为pip并不将python视为包
> - pip可以安装一些conda无法安装的包；conda也可以安装一些pip无法安装的包。因此当使用一种命令无法安装包时，可以尝试用另一种命令

## 其他方法：

**当使用`conda install`无法进行安装时，可以考虑从Anaconda.org**中获取安装包的命令，并进行安装

- 从Anaconda.org安装包时，无需注册。
- 在**当前环境**中安装来自于Anaconda.org的包时，需要通过输入要安装的包在Anaconda.org中的路径作为获取途径（channel）。查询路径的方式如下：
  1. 在浏览器中输入：[http://anaconda.org](https://link.jianshu.com?t=http%3A%2F%2Fanaconda.org)，或直接点击[Anaconda.org](https://link.jianshu.com?t=http%3A%2F%2Fanaconda.org)
  2. 在新页面“Anaconda Cloud”的上方搜索框中输入要安装的包名，然后点击右边“放大镜”标志

![img](https://img-blog.csdnimg.cn/20200823231948155.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0xlZ2VuZHNfTmV2ZXJfRGll,size_16,color_FFFFFF,t_70)

![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)

**这里搜索Numpy:
![img](https://img-blog.csdnimg.cn/20200823232134451.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0xlZ2VuZHNfTmV2ZXJfRGll,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)​**

**选择一条命令复制到终端执行即可：**
![img](https://img-blog.csdnimg.cn/20200823232302403.png?x-oss-process=image/watermark,type_ZmFuZ3poZW5naGVpdGk,shadow_10,text_aHR0cHM6Ly9ibG9nLmNzZG4ubmV0L0xlZ2VuZHNfTmV2ZXJfRGll,size_16,color_FFFFFF,t_70)![点击并拖拽以移动](data:image/gif;base64,R0lGODlhAQABAPABAP///wAAACH5BAEKAAAALAAAAAABAAEAAAICRAEAOw==)​