title: Responsive Web Design 学习用例
date: 2015-06-01 15:07:26
category: tech
tags: [tech css]
---

## RWD的概念

响应式网页设计能让不同大小的设备上呈现同样的网页内容。"一次设计，普遍适用"的概念让网站开发可以不用专门为手机做一套定制版本，减少维护和内容适配的问题。下面是来自百度百科的关于“响应式Web设计(Responsive Web design)”的理念：

>  页面的设计与开发应当根据用户行为以及设备环境(系统平台、屏幕尺寸、屏幕定向等)进行相应的响应和调整。具体的实践方式由多方面组成，包括弹性网格和布局、图片、CSS media query的使用等。无论用户正在使用笔记本还是iPad，我们的页面都应该能够自动切换分辨率、图片尺寸及相关脚本功能等，以适应不同设备；换句话说，页面应该有能力去自动响应用户的设备环境。响应式网页设计就是一个网站能够兼容多个终端——而不是为每个终端做一个特定的版本。这样，我们就可以不必为不断到来的新设备做专门的版本设计和开发了。

接下来通过下面一个例子来逐步学习RWD。

## 代码实现
###  HTML 代码

```
<html>
  <head>
    <meta http-equiv="content-type" content="text/html; charset=utf-8" />
    <meta name="viewport" content="width=device-width, initial-scale=1" />
    <title>Responsive Demo</title>
  </head>
  <link rel="stylesheet" href="index.css" type="text/css"/>
  <body>
    <div class="header"></div>
    <div class="content">
      <div class="left"> </div>
      <div class="middle"> </div>
      <div class="right"> </div>
    </div>
    <div class="footer"></div>
  </body>
</html>
```

### CSS 代码

```
* {
  margin: 0px;
  padding: 0px;
}

.header,
.content,
.footer {
  margin: 10px auto;
  width: 960px;
}

.header {
  height: 100px;
  background-color: #D2691E;
}

.left,
.middle,
.right {
  background-color: #6495ED;
  height: 200px;
  width: 300px;
}

.left,
.middle {
  margin-right: 10px;
}

.footer {
  height: 100px;
  background-color: #5F9D88;
}
```

"自适应网页设计"的核心是CSS3引入了[Media Query](http://www.w3.org/TR/CSS21/media.html)模块。能够通过检测屏幕宽度的像素加载相应的css代码。

例如，屏幕宽度的最小像素在980以上，则显示header + 3 columns +
footer 效果：

![full screen](/assets/fullscreen.png)

```
@media screen and (min-width: 980px) {
  .header,
  .content,
  .footer {
    width: 960px
  }

  .left,
  .middle,
  .right {
    float: left;
    height: 500px;
  }

  .left,
  .right {
    width: 220px;
  }

  .middle {
    width: 500px;
  }

  .content {
    height: 500px;
  }
}
```

如果屏幕宽度的像素在600到980之间，则显示header + 2 columns + footer，隐藏第三行column：

![two columns](/assets/twocolumns.png)


```
@media screen and (min-width: 600px) and (max-width: 980px) {
  .header,
  .content,
  .footer {
    width: 600px;
  }

  .left,
  .middle {
    float: left;
    height: 400px;
  }
  .right {
    display: none;
  }

  .left {
    width: 200px;
  }

  .middle {
    width: 380px;
  }

  .content {
    height: 400px;
  }
}
```

然后到了适配手机版本的屏幕了。这时我们出来像素宽度从400到600的界面，变成一个类似“汉堡”的排列样式：

![hanburger](/assets/hanburger.png)

```
@media screen and (min-width: 400px) and (max-width: 600px) {
  .header,
  .content,
  .footer {
    width: 400px;
  }

  .left,
  .right,
  .middle {
    float: left;
    margin-bottom: 5px;
    height: 100px;
    width: 400px;
  }

  .content {
    height: 310px;
  }
}
```

asdfasdfasd

## 总结

上面是比较简单的操作用例，下面是工作中通过模仿上面的用例重构的网站。思路大同小异，只是需要重新设计一个导航条。默认的大屏幕下的导航条在小屏幕下不显示，反而加载“汉堡导航”。当然了，RWD的设计固然能够同时整合pc和mobile的要求，它同时也会引起性能问题。试想同样的html/js/css/images等资源也一遍加载到mobile上，手机的显示速度也会打折扣。

### 大屏幕下的原始网站

![asicsfullscreen](/assets/asicsfullscreen.png)

### 手机和小屏幕版本的网站

![asics](/assets/asics.png)
![navigation](/assets/navigation.png)
