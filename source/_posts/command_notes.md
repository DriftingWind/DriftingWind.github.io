title: 一些命令和文章的记录 
date: 2016-06-24 00:14:54
categories: sh
tags: [Hexo,bash,zsh,nvm]
description: Hexo、bash、zsh、nvm

---

平时用到的命令记录，防止忘记，好记性不如烂笔头。

<!--more-->
### Hexo 部署一些命令
	我的nvm命令 在zsh 下才能用 如果不是zsh 需要切回zsh
	nvm ls 列出当前安装的node版本

	nvm use 用具体的版本，然后hexo 命令就能使用了，否则会出现 zsh: command not found:hexo

	hexo clean && hexo g && hexo d 发布(清理 生成 部署)
	
[https://www.zhihu.com/question/21193762](https://www.zhihu.com/question/21193762)
修改默认分支hexo 并且为空内容
把博客的hexo文件，拷贝到空分支内
修改_config.yml中的deploy参数，branch:master；
7. 依次执行git add .、git commit -m "..."、git push origin hexo提交网站相关的文件；

所有操作只需要在hexo分支下进行就可以了
先提交 hexo分支  再部署博客

三、本地资料丢失后的流程
当重装电脑之后，或者想在其他电脑上修改博客，可以使用下列步骤：

1. 使用git clone git@github.com:CrazyMilk/CrazyMilk.github.io.git拷贝仓库（默认分支为hexo）；
2. 在本地新拷贝的[http://CrazyMilk.github.io](http://CrazyMilk.github.io)文件夹下通过Git bash依次执行下列指令：npm install hexo、npm install、npm install hexo-deployer-git（记得，不需要hexo init这条指令）。

### 命令行切换
切换回`bash`的命令是chsh -s /bin/bash

切换回`zsh`的命令是chsh -s /bin/zsh

### android源码编译
在[Android源码的下载、编译与导入到Android Studio](http://blog.csdn.net/wl9739/article/details/51429242) 可能会出现下面的问题：
执行POSIXLY_CORRECT=1 sudo port install gmake libsdl git gnupg 出现 `Port gmake not found`
>根据官方文档在安装了MacPorts之后，使用MacPorts安装依赖工具时，可能会遇到`Port gmake not found`的错误，在安装之前需要先执行下面的命令。

>sudo port -d sync
然后就可以安装相关的工具了。

>POSIXLY_CORRECT=1 sudo port install gmake libsdl git gnupg

如果安装始终失败，请直接写下载源码，下载完成后，再执行上面的命令试下

在上面文章内，我运行的时候，`没有下载驱动`，也成功启动模拟器，进入系统了

### Swift记录
###### 提示
	1.UIScrollView自动布局不需要设置contentSize，只要最后一个控件的bottom与uiscrollview的bottom相同即可
	2.tabbarcontroller 和 navigation controller 组合使用 self.view会被这两个挡住下面两句是处理代码 
	    if (self.parent?.parent as? IAWTabBarController) != nil {
            print("执行这里")
            self.navigationController?.navigationBar.isTranslucent = false;
            self.tabBarController?.tabBar.isTranslucent = false
        }
### Ps记录
	1.Ps修改图片大小
	  Ps打开图片 看右下角图层位置，如果是索引颜色，需要修改成背景，点菜单位置 图像 模式 修改成rgb颜色，然后再修改图像 图像大小，修改像素（索引颜色不能修改）
	  
	  
### 开源让世界更美好   
### 感谢
[Hexo你的博客](http://ibruce.info/2013/11/22/hexo-your-blog/?utm_source=tuicool&utm_medium=referral)

[5分钟 搭建免费个人博客](http://www.jianshu.com/p/4eaddcbe4d12)

[Android源码的下载、编译与导入到Android Studio](http://blog.csdn.net/wl9739/article/details/51429242)

[OS X 10.11下载和编译Android6.0源码](https://liuzhichao.com/2016/osx-download-and-build-android-source.html)

[Android 镜像使用帮助 清华源](https://mirrors.tuna.tsinghua.edu.cn/help/AOSP/)