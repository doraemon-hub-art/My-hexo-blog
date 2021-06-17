---
title: Hexo-Butterfly添加天气插件
date: 2021-06-17 19:18:33
tags:
  - -Hexo
categories:
  - Hexo
cover: /img/taohua.png
---

**我的小站**——[半生瓜のblog](http://doraemon2.xyz/)

**感谢糖果屋Akilar老哥的帮助**——[Akilarの糖果屋](https://akilar.top/)

# **Hexo-Butterfly添加天气插件**

**效果如图所示**：![image-20210617185624577](/images/Hexo-Butterfly添加天气插件.assets/image-20210617185624577.png)



就是JS插件的字体大小我不会调，稍微有一点瑕疵，不过影响不大。

我用的是心知天气的天气插件(其他天气插件同理)。

**首先**，先到心知天气的官网注册一个账号——[心知天气](https://www.seniverse.com)。

登录，申请一个免费版，然后在产品的下拉栏中选择天气插件，然后点击立即免费使用。

**然后**，在下面选择显示参数。

![image-20210617190318207](/images/Hexo-Butterfly添加天气插件.assets/image-20210617190318207.png)

**点击生成代码并复制**。

**打开主题文件目录**

![image-20210617190559552](/images/Hexo-Butterfly添加天气插件.assets/image-20210617190559552.png)

打开**nav.pug**

将下面这行代码放入合适的位置

```html
<div id="tp-weather-widget"></div>
```

**例如**:

![image-20210617190804513](Hexo-Butterfly添加天气插件.assets/image-20210617190804513.png)

**保存并关闭**

**接着**，在此路径下创建一个JS文件，名称随意。

![image-20210617190908876](/images/Hexo-Butterfly添加天气插件.assets/image-20210617190908876.png)

**打开**，将刚才复制代码中的 <script></script>中间的内容粘贴进去。

就是将下图中画横线的代码删除。

![image-20210617191118038](/images/Hexo-Butterfly添加天气插件.assets/image-20210617191118038.png)

**保存并关闭**

**最后**，找到主题配置文件_config.yml

**打开**，在**inject**处引入刚才创建的JS文件(注意文件名称)。

![image-20210617191324806](Hexo-Butterfly添加天气插件.assets/image-20210617191324806.png)

**保存并退出**

**hexo clean&&hexo g&&hexo d即可完成**

