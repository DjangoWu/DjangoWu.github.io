---
title: Hexo-Next配置
date: 2018-01-05 21:08:10
tags: Hexo, Next
categories: 网站搭建
copyright:true
top:110
---



###1.添加[关于][标签]等页面
>- hexo new page about
>- hexo new page tags

###2.头像设置
> 修改站点配置文件，修改**avatar**,值设置成头像的链接或者地址
```
# Sidebar Avatar
# in theme directory(source/images): /images/avatar.gif
# in site  directory(source/uploads): /uploads/avatar.gif
avatar: /images/avatar.jpg
```

###3. 圆形头像（添加旋转）
> Next主题默认不支持，修改**source/css/_common/components/sidebar/sidebar-author.styl**
```css 
.site-author-image {
  display: block;
  margin: 0 auto;
  padding: $site-author-image-padding;
  max-width: $site-author-image-width;
  height: $site-author-image-height;
  border: $site-author-image-border-width solid $site-author-image-border-color;

  /* 头像圆形 */
  border-radius: 80px;
  -webkit-border-radius: 80px;
  -moz-border-radius: 80px;
  box-shadow: inset 0 -1px 0 #333sf;
  
  /* 设置循环动画 [animation: (play)动画名称 (2s)动画播放时长单位秒或微秒 (ase-out)动画播放的速度曲线为以低速结束 
    (1s)等待1秒然后开始动画 (1)动画播放次数(infinite为循环播放) ]*/
  -webkit-animation: play 2s ease-out 1s 1;
  -moz-animation: play 2s ease-out 1s 1;
  animation: play 2s ease-out 1s 1; 

  /* 鼠标经过头像旋转360度 */
  -webkit-transition: -webkit-transform 1.5s ease-out;
  -moz-transition: -moz-transform 1.5s ease-out;
  transition: transform 1.5s ease-out;
}

img:hover {
  /* 鼠标经过停止头像旋转 
  -webkit-animation-play-state:paused;
  animation-play-state:paused;*/

  /* 鼠标经过头像旋转360度 */
  -webkit-transform: rotateZ(360deg);
  -moz-transform: rotateZ(360deg);
  transform: rotateZ(360deg);
}

/* Z 轴旋转动画 */
@-webkit-keyframes play {
  0% {
    -webkit-transform: rotateZ(0deg);
  }
  100% {
    -webkit-transform: rotateZ(-360deg);
  }
}
@-moz-keyframes play {
  0% {
    -moz-transform: rotateZ(0deg);
  }
  100% {
    -moz-transform: rotateZ(-360deg);
  }
}
@keyframes play {
  0% {
    transform: rotateZ(0deg);
  }
  100% {
    transform: rotateZ(-360deg);
  }
}

.site-author-name {
  margin: $site-author-name-margin;
  text-align: $site-author-name-align;
  color: $site-author-name-color;
  font-weight: $site-author-name-weight;
}

.site-description {
  margin-top: $site-description-margin-top;
  text-align: $site-description-align;
  font-size: $site-description-font-size;
  color: $site-description-color;
}
```

###4. 删除底部HEXO主题和主题
> 修改**\themes\next\layout_partials\footer.swig**文件，删除：
```
<div class="powered-by">{#
  #}{{ __('footer.powered', '<a class="theme-link" target="_blank" href="https://hexo.io">Hexo</a>') }}{#
#}</div>

<div class="theme-info">{#
  #}{{ __('footer.theme') }} &mdash; {#
  #}<a class="theme-link" target="_blank" href="https://github.com/iissnan/hexo-theme-next">{#
    #}NexT.{{ theme.scheme }}{#
  #}</a>{% if theme.footer.theme.version %} v{{ theme.version }}{% endif %}{#
#}</div>
```

###5.网站底部添加访问量
> 打开**\themes\next\layout_partials\footer.swig**文件，在**copyright**前加上：
```
<script async src="https://dn-lbstatics.qbox.me/busuanzi/2.3/busuanzi.pure.mini.js"></script>
```
> 然后再合适的位置添加显示统计的代码:
```
<div class="powered-by">
<i class="fa fa-user-md"></i><span id="busuanzi_container_site_uv">
  本站访客数:<span id="busuanzi_value_site_uv"></span>
</span>
</div>
```
这里有两种计算方式的统计代码
>- pv方式，单个用户连续点击n篇文章，记录n次访问量
>- uv方式，单个用户连续点击n篇文章，只记录1次访客量

###6. 网站底部字数统计
> 切换到根目录下，然后运行如下代码
> ```$ npm install hexo-wordcount --save```
> 然后在**/themes/next/layout/_partials/footer.swig**恰当位置添加：```<div class="theme-info">
  <div class="powered-by"></div>
  <span class="post-count">博客全站共{{ totalcount(site) }}字</span>
</div>```

###7.设置网站的图标Favicon
> 在[EasyIcon](http://www.easyicon.net/)中找到适当的比例的ico图标，或者从别的网站下载或制作，并将名称修改为favicon.ico，然后将图标放在**/themes/next/source/images**，并修改主题配置文件：
```
favicon: 
  small: /images/favicon.ico
  #favicon-16x16-next.png
  medium: /images/favicon.ico
  #favicon-32x32-next.png
  apple_touch_icon: /images/favicon.ico
  #apple-touch-icon-next.png
  safari_pinned_tab: /images/favicon.ico
```
  
###8.实现统计功能
> 在根目录下安装**hexo-wordcount**，运行```$ npm install hexo-wordcount --save```
> 然后在主题配置文件中，配置如下：
```post_wordcount:
  item_text: true
  wordcount: true
  min2read: true
```
  
###9.添加顶部加载条
> 修改主题配置文件，将```pace: false```改为```pace: true```即可，里面还内置了很多样式，可直接更换。

###10.设置阅读全文
>- 在文章中使用手动进行截断(推荐): ```<!-- more -->```
>- 在文章的 front-matter 中添加 description，并提供文章摘录
>- 自动形成摘要，在主题配置文件中添加：
```
auto_excerpt:
enable: true
length: 150
```

###10. 文章打赏
> 在文章开头添加```reward: true```，并修改主题配置页面：
```
# Reward
reward_comment: 坚持原创技术分享，您的支持将鼓励我继续创作！
wechatpay: /images/wechatpay.jpg
alipay: /images/alipay.jpg
```

###11.返回顶部组件增加百分比
> 在**主题配置文件**中修改**scrollpercent**

###12.博文置顶
> 将文件```node_modules/hexo-generator-index/lib/generator.js```中代码修改为
```
'use strict';
var pagination = require('hexo-pagination');
module.exports = function(locals){
  var config = this.config;
  var posts = locals.posts;
    posts.data = posts.data.sort(function(a, b) {
        if(a.top && b.top) { // 两篇文章top都有定义
            if(a.top == b.top) return b.date - a.date; // 若top值一样则按照文章日期降序排
            else return b.top - a.top; // 否则按照top值降序排
        }
        else if(a.top && !b.top) { // 以下是只有一篇文章top有定义，那么将有top的排在前面（这里用异或操作居然不行233）
            return -1;
        }
        else if(!a.top && b.top) {
            return 1;
        }
        else return b.date - a.date; // 都没定义按照文章日期降序排
    });
  var paginationDir = config.pagination_dir || 'page';
  return pagination('', posts, {
    perPage: config.index_generator.per_page,
    layout: ['index', 'archive'],
    format: paginationDir + '/%d/',
    data: {
      __index: true
    }
  });
};
```
> 在文章中添加**top**值，数值越大，文章越靠前。

###13.修改打赏字体不闪动
> 修改文件```next/source/css/_common/components/post/post-reward.styl```，然后注释其中的函数```wechat:hover```和```alipay:hover```，如下：
```
/* 注释文字闪动函数
 #wechat:hover p{
    animation: roll 0.1s infinite linear;
    -webkit-animation: roll 0.1s infinite linear;
    -moz-animation: roll 0.1s infinite linear;
}
 #alipay:hover p{
   animation: roll 0.1s infinite linear;
    -webkit-animation: roll 0.1s infinite linear;
    -moz-animation: roll 0.1s infinite linear;
}*/
```

###14.添加评论功能（liveRe 来必力）
> 注册[来必力](https://livere.com)，获取```liver_uid```，编辑主题配置文件，编辑```livere_uid```即可。

###15.添加阅读次数
> 注册[LeanCloud](https://leancloud.cn/)

>- 首先在**应用**创建一个应用，名字随意
>- 然后在**存储->数据**中创建class，取名为**Counter**
>- 在**设置->应用Key**中取得APP ID和APP Key
>- 回到设置，选择安全中心，在Web安全域名中添加博客地址

> 修改主题配置文件:
```
leancloud_visitors:
  enable: true
  app_id: #你的app_id
  app_key: #你的的app_key
```

###16. 修改文章底部带#标签
> 修改模板**/themes/next/layout/_macro/post.swig**，搜索```rel=”tag”>#```，将 # 换成```<i class="fa fa-tag"></i>```

###17. 修改文章内链接文本样式
> 将链接文本设置为蓝色，鼠标划过时文字颜色加深，并显示下划线。
找到文件**themes\next\source\css\_custom\custom.styl**，添加如下 css 样式：
```
.post-body p a {
  color: #0593d3;
  border-bottom: none;
  &:hover {
    color: #0477ab;
    text-decoration: underline;
  }
}
```

###18.搜索功能
> 安装**hexo-generator-searchdb**，在站点根目录下执行```$ npm install hexo-generator-searchdb --save```，编辑站点配置文件，新增以下内容到任意位置：
```
search:
  path: search.xml
  field: post
  format: html
  limit: 10000
```
> 修改主题配置文件：
```
# Local search
# Dependencies: https://github.com/flashlab/hexo-generator-search
local_search:
  enable: true
  # if auto, trigger search by changing input
  # if manual, trigger search by pressing enter key or search button
  trigger: auto
  # show top n results per article, show all results by setting to -1
  top_n_per_article: 1
```

###19.创建分类页面
>- 新建一个页面， ```hexo new page categories```
>- 编辑新建的页面，增加```type: categories```
>- 屏蔽评论功能，```comments: false```
>- 菜单中添加链接，编辑主题配置文件，解注释```categories```
>- (tags页面类似)

---