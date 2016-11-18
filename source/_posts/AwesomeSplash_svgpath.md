title: 一步步教你利用AwesomeSplash开源项目做炫酷启动页
date: 2016-06-23 22:26:33
categories: Splash
tags: [AwesomeSplash,GIMP,PicGif]
description: AwesomeSplash、GIMP、PicGif、Splash


---

最近项目需要开展，想做个炫酷的启动页，在github上看到开源项目[AwesomeSplash](https://github.com/ViksaaSkool/AwesomeSplash)，打算利用这个项目做个启动页。

<!--more-->
先上效果图：
![移动执法](http://o8wlu6q9f.bkt.clouddn.com/%E7%A7%BB%E5%8A%A8%E6%89%A7%E6%B3%95.gif)
### AwesomeSplash配置
按照AwesomeSplash基本配置项目。

         configSplash.setBackgroundColor(R.color.primary); //any color you want form colors.xml
            configSplash.setAnimCircularRevealDuration(2000); //int ms
            configSplash.setRevealFlagX(Flags.REVEAL_RIGHT);  //or Flags.REVEAL_LEFT
            configSplash.setRevealFlagY(Flags.REVEAL_BOTTOM); //or Flags.REVEAL_TOP

            //Choose LOGO OR PATH; if you don't provide String value for path it's logo by default

            //Customize Logo
            configSplash.setLogoSplash(R.mipmap.ic_launcher); //or any other drawable
            configSplash.setAnimLogoSplashDuration(2000); //int ms
            configSplash.setAnimLogoSplashTechnique(Techniques.Bounce); //choose one form Techniques (ref: https://github.com/daimajia/AndroidViewAnimations)


            //Customize Path
            configSplash.setPathSplash(Constants.DROID_LOGO); //set path String
            configSplash.setOriginalHeight(400); //in relation to your svg (path) resource
            configSplash.setOriginalWidth(400); //in relation to your svg (path) resource
            configSplash.setAnimPathStrokeDrawingDuration(3000);
            configSplash.setPathSplashStrokeSize(3); //I advise value be <5
            configSplash.setPathSplashStrokeColor(R.color.accent); //any color you want form colors.xml
            configSplash.setAnimPathFillingDuration(3000);
            configSplash.setPathSplashFillColor(R.color.Wheat); //path object filling color


            //Customize Title
            configSplash.setTitleSplash("My Awesome App");
            configSplash.setTitleTextColor(R.color.Wheat);
            configSplash.setTitleTextSize(30f); //float value
            configSplash.setAnimTitleDuration(3000);
            configSplash.setAnimTitleTechnique(Techniques.FlipInX);
            configSplash.setTitleFont("fonts/myfont.ttf"); 
        
配置后发现跑不起来，把项目中需要的颜色值替换成我们自己的。发现作者提供的依赖包里没有DROID_LOGO找到这个，实际上作者应该想让我们自己配置的。我这里要先看到作者放在git上的效果图，所以我从看了下作者github里的源码，从源码里找到DROID_LOGO 作者demo的设置的值。
     
     public static final String DROID_LOGO = "M 149.22,22.00\n" +
            "           C 148.23,20.07 146.01,16.51 146.73,14.32\n" +
            "             148.08,10.21 152.36,14.11 153.65,16.06\n" +
            "             153.65,16.06 165.00,37.00 165.00,37.00\n" +
            "             194.29,27.24 210.71,27.24 240.00,37.00\n" +
            "             240.00,37.00 251.35,16.06 251.35,16.06\n" +
            "             252.64,14.11 256.92,10.21 258.27,14.32\n" +
            "             258.99,16.51 256.77,20.08 255.78,22.00\n" +
            "             252.53,28.28 248.44,34.36 246.00,41.00\n" +
            "             252.78,43.16 258.78,48.09 263.96,52.85\n" +
            "             281.36,68.83 289.00,86.62 289.00,110.00\n" +
            "             289.00,110.00 115.00,110.00 115.00,110.00\n" +
            "             115.00,110.00 117.66,91.00 117.66,91.00\n" +
            "             120.91,76.60 130.30,62.72 141.04,52.85\n" +
            "             146.22,48.09 152.22,43.16 159.00,41.00\n" +
            "             159.00,41.00 149.22,22.00 149.22,22.00 Z\n" +
            "           M 70.80,56.00\n" +
            "           C 70.80,56.00 97.60,100.00 97.60,100.00\n" +
            "             101.34,106.21 108.32,116.34 110.21,123.00\n" +
            "             113.76,135.52 103.90,147.92 91.00,147.92\n" +
            "             78.74,147.92 74.44,139.06 69.00,130.00\n" +
            "             69.00,130.00 39.80,82.00 39.80,82.00\n" +
            "             35.73,75.29 28.40,66.08 29.20,58.00\n" +
            "             30.26,47.20 38.61,40.47 49.00,39.72\n" +
            "             61.22,40.24 64.96,46.28 70.80,56.00 Z\n" +
            "           M 375.80,58.00\n" +
            "           C 376.60,66.08 369.27,75.29 365.20,82.00\n" +
            "             365.20,82.00 336.00,130.00 336.00,130.00\n" +
            "             330.71,138.82 326.73,147.24 315.00,147.89\n" +
            "             301.74,148.63 291.14,135.87 294.79,123.00\n" +
            "             296.68,116.34 303.66,106.21 307.40,100.00\n" +
            "             307.40,100.00 333.00,58.00 333.00,58.00\n" +
            "             339.02,47.98 342.23,40.92 355.00,39.72\n" +
            "             365.83,40.00 374.69,46.77 375.80,58.00 Z\n" +
            "           M 289.00,116.00\n" +
            "           C 289.00,116.00 289.00,239.00 289.00,239.00\n" +
            "             288.98,249.72 285.92,257.31 275.00,261.10\n" +
            "             265.22,264.50 258.37,259.56 255.02,264.43\n" +
            "             253.78,266.24 254.00,269.84 254.00,272.00\n" +
            "             254.00,272.00 254.00,298.00 254.00,298.00\n" +
            "             254.00,304.85 254.77,310.07 250.36,315.93\n" +
            "             242.35,326.68 226.84,326.49 218.80,315.93\n" +
            "             215.07,311.00 215.01,306.83 215.00,301.00\n" +
            "             215.00,301.00 215.00,262.00 215.00,262.00\n" +
            "             215.00,262.00 190.00,262.00 190.00,262.00\n" +
            "             190.00,262.00 190.00,301.00 190.00,301.00\n" +
            "             189.99,306.83 189.93,311.00 186.20,315.93\n" +
            "             178.16,326.49 162.65,326.68 154.64,315.93\n" +
            "             151.09,311.22 151.01,307.61 151.00,302.00\n" +
            "             151.00,302.00 151.00,272.00 151.00,272.00\n" +
            "             151.00,269.84 151.22,266.24 149.98,264.43\n" +
            "             146.53,259.42 138.97,264.76 129.00,260.86\n" +
            "             118.39,256.72 116.02,248.29 116.00,238.00\n" +
            "             116.00,238.00 116.00,116.00 116.00,116.00\n" +
            "             116.00,116.00 289.00,116.00 289.00,116.00 Z";
把一些都改了，就能看到看到作者的效果图。

### 利用GIMP做个SVG
从作者的demo中我们能看出上面的android机器人，是利用svg的path值绘制的，作者在github上也说了
>So before diving into AwesomeSplash library, look into the libraries. Especially make sure you understand the concept of SVG path and look deeply on how to [create](http://www.useragentman.com/blog/2013/04/26/how-to-create-svg-paths-easily-using-the-gimp/) you custom svg (and get the string values needed for AwesomeSplash).

`create`的链接里说了，用GIMP创建的svg内的path才能用到，目前我试了一些svg生成工具。GIMP下载地址我会放在末尾.

我这里从美工哪里拿到一个“移动执法.png“ 图片，然后用GIMP打开。如果你没有看到下面 工具箱
![GIMP工具箱](http://o8wlu6q9f.bkt.clouddn.com/GIMP%E5%B7%A5%E5%85%B7%E7%AE%B1.png)
菜单 -> `工具` -> `工具箱（或新建工具箱）`就会弹出工具箱，
然后选择 `模糊选择工具`(上图工具箱内第二版第2个)，然后把鼠标移动到图片上使你需要的汉字都变成虚线闪烁，这个时候，你会发现整个图片，包裹边框都变闪烁虚线了，但是我这里就上面那4个字变虚线就行了，我不要边框。这时候，你选择菜单 `查看` 点一下 `显示图层边界`，把当前选项前面的勾去掉，然后在菜单—> `选择`-`反转`，你就会发现边框闪烁虚线没有了。

### 生成SVG
然后看到右边路径面板如下图 标注的`红色按钮` 点击

![路径按钮](http://o8wlu6q9f.bkt.clouddn.com/%E8%B7%AF%E5%BE%84%E9%9D%A2%E6%9D%BF.png)

点击后会在当前路径面板生成 选区 ，在`选区`上点击右键选择`导出路径` 填上名称比如 测试.svg,然后选择保存路径就会生成svg。

然后用文本格式打开，就会看到svg内`path`字段，拷贝内部数值替换DROID_LOGO，
把
	   
	configSplash.setOriginalHeight(400); //in relation to your svg (path) resource
    configSplash.setOriginalWidth(400);
    换成我们自己的svg的宽高
然后把

    configSplash.setRevealFlagX(Flags.REVEAL_RIGHT);  //or Flags.REVEAL_LEFT
            configSplash.setRevealFlagY(Flags.REVEAL_BOTTOM); //or       Flags.REVEAL_TOP
    
      改成
       
    configSplash.setRevealFlagX(Flags.REVEAL_LEFT);  //or Flags.REVEAL_LEFT
    configSplash.setRevealFlagY(Flags.REVEAL_TOP); //or Flags.REVEAL_TOP
        
就变成从左上，到左右铺展了。 我这里发现，我把上面的图片换成字体后，我发现我下面的就多余，我现在直接把下面设置成空字符串，时间设置为0 

    configSplash.setTitleSplash("");//去除字体
    configSplash.setAnimTitleDuration(0); 
   
然后效果图，就有了大致的效果了，为什么说大致呢，你有没有发现作者原来的都是`OriginalHeight`和`OriginalWidth`都是400，如果你的svg的宽高大于这个值得时候，就会字体`变形`。svg path引入的始终那么大，后来，我看了作者的源码，我没有发现对那个地方设置的，后来我找到
initPathAnimation方法，对path做处理的
接着看这个方法：

    int viewSize =      getResources().getDimensionPixelSize(R.dimen.fourthSampleViewSize);//获取大小
    //对布局进行设置
        FrameLayout.LayoutParams params = new FrameLayout.LayoutParams(viewSize, viewSize);

        params.setMargins(0, 0, 0, 50);
        FillableLoaderBuilder loaderBuilder = new FillableLoaderBuilder();
        mPathLogo = loaderBuilder
                .parentView(mFl)
                .layoutParams(params)
                .svgPath(mConfigSplash.getPathSplash())
                .originalDimensions(mConfigSplash.getOriginalWidth(), mConfigSplash.getOriginalHeight())
                .strokeWidth(mConfigSplash.getPathSplashStrokeSize())
                .strokeColor(Color.parseColor(String.format("#%06X", (0xFFFFFF & getResources().getColor(mConfigSplash.getPathSplashStrokeColor())))))
                .fillColor(Color.parseColor(String.format("#%06X", (0xFFFFFF & getResources().getColor(mConfigSplash.getPathSplashFillColor())))))
                .strokeDrawingDuration(mConfigSplash.getAnimPathStrokeDrawingDuration())
                .fillDuration(mConfigSplash.getAnimPathFillingDuration())
                .clippingTransform(new PlainClippingTransform())
                .build();
        mPathLogo.setOnStateChangeListener(new OnStateChangeListener() {
            @Override
            public void onStateChange(int i) {
                if (i == State.FINISHED) {
                    startTextAnimation();
                }
            }
        });
 看到这里获取大小的值，对应path布局设置大小，但是我找了源码没有看到找个数值的值，于是我在自己的项目的dimens.xml 中定义了
 
    //处理启动页的宽度
    <dimen name="fourthSampleViewSize">350dp</dimen>
定义后，我发现我的图片可以正常展示了。好了这样一个简单炫酷的加载页就完成了    
    
### 开源让世界更美好   
### 感谢
[GIMP下载](http://gensho.acc.umu.se/pub/gimp/gimp/v2.8/osx/gimp-2.8.16-x86_64-1.dmg)

[Mac屏幕录制与gif图片制作教程](http://www.jianshu.com/p/545014e51ad5)
