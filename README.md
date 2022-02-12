# Python自动爬取花瓣网任意面板中所有图片

## 需要安装的库

- urllib
- easygui
- selenium
- webdriver_manager

## 获取过程

1. 进入面板内
2. 复制当前面板url
3. 启动该脚本按提示进行即可

## 代码编写

### 分析pin图特点

查看面板源码，可以在对应的script中找到面板中图片的json数据。

在app.page["board"]下可以找到"pins":[{...}],主要图片ID(pin)位于这里面。

![](https://pic.imgdb.cn/item/6207dafb2ab3f51d91dca7a5.png)

获取到图片的ID(pin)之后可以对应访问点击图片后进入的地址http://huaban.com/pins/pinId/，并获取页面源码：

![](https://pic.imgdb.cn/item/6207db172ab3f51d91dcc00b.png)

显然可见主要图片的源码特征，书写对应正则表达式可以获取图片真实地址。

### 分析滚动特点

通过滚动页面我们可以发现**加载规律**：

原来的图片对应的代码：
![](https://pic.imgdb.cn/item/6207dbbc2ab3f51d91dd5590.png)

经过滚动，原来的代码逐渐被一些新的代码取代：

![](https://pic.imgdb.cn/item/6207dbca2ab3f51d91dd60a0.png)

而不难发现他们都有对应的**data-id**!而data-id就是图片地址中对应的**pin**。

所以我们可以通过**webdriver滚动加载页面**，**每滚动一次就进行一次data-id的读取**，并利用**集合进行去重**即可。

### 编写代码

...