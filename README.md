# CSS、JS碰上DOM解析和渲染

[代码地址](https://github.com/Ortonzhang/js-css-block-draw)

**代码运行环境 node >= 7.0**


#### 场景一：

假设 CSS会阻塞DOM的解析和渲染

此时页面代码如下

![image.png](http://upload-images.jianshu.io/upload_images/4060631-3ae95d41f7abd22f.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/840)

script标签使用了defer, 关于defer的使用[请戳这里](http://www.w3school.com.cn/html5/att_script_defer.asp)

 打开浏览器，并打开开发者模式。console里能看到打印出来了div，此时的页面还是一张空白。等到差不多3s后，浏览器显示了一个浅红色的div。

结论: css不会阻塞DOM的解析，但会阻塞其渲染。

#### 场景二：

我们改下代码，把js文件放在样式文件的下面，将defer去掉。在css上面写点js。如下：


![场景二](http://upload-images.jianshu.io/upload_images/4060631-f3ca079fd2db8dd9.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)

浏览器打印了before js，然后转了3s后，显示出了div，控制台才打印出了 [ ]。

我去，不对啊，css不是不会阻塞DOM的解析吗。其实阻塞DOM的是JS，但是JS就有两行代码，绝对不会阻塞这么长时间，那就是在等待css的下载。

浏览器看到js文件后就会阻塞DOM渲染，如果此时CSS文件还没有加载完后，就会等待CSS加载完后在去执行js，因为浏览器不知道js要做什么，会不会获取样式，所以就会等所有css下载完成后，再执行js。

结论：css会阻塞后面JS的的执行

#### 场景三：
将block.js引入，在浏览器中运行。

浏览器会转3s后，然后打印出`null` 然后出现改浅灰色的div。

js会阻塞DOM的渲染，因为是浏览器不知道脚本的内容，如果浏览器解析下DOM后，js又全部删除了，就白费功夫了。浏览器无法预料js的内容，所以就直接停止，等脚本执行完后在进行解析。

#### 场景四：


![image.png](http://upload-images.jianshu.io/upload_images/4060631-8b4f2b5184bc405d.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)


此时的页面会怎么显示的？

首先是个红色的div，3s后显示灰色的div，并打印div。再过2s后显示桃红色的div，并再次打印div。

浏览器碰到没有defer、async属性的script标签的时候，会触发页面渲染，当前面的css没有下载完成的时候，浏览器会等到css加载完成后再执行。

## 总结：
CSS不会阻塞DOM解析，但会阻塞DOM渲染

JS会阻塞DOM解析和渲染

CSS会阻塞后面的JS的执行

浏览器碰到`script`标签的并且没有defer、async的时候会触发页面的渲染。

JS如果放在head中，放在css头部

所以会把CSS放在head标签里面，JS文件放在底部


原文地址：
[原来 CSS 与 JS 是这样阻塞 DOM 解析和渲染的](https://juejin.im/post/59c60691518825396f4f71a1)


参考地址：
[css阻塞与js阻塞](http://blog.csdn.net/qq_36631168/article/details/53131336)



