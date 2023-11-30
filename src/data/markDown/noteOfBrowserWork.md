# **浏览器工作原理**
***
## 导航
导航是指用户通过在地址栏输入一个 URL、点击一个链接、提交表单或者是其他的行为。
1. DNS查询
即将文字链接转换为机器可读的IP地址的过程
2. TCP
获取到IP地址后，进行TCP三次握手尝试与服务器建立连接
具体步骤为: 客户端向服务器发送SYN帧，服务器向客户端发送SYN-ACK帧，最后服务器向客户端发送ACK帧建立连接
3. TLS
即传输层安全性协议。决定了什么密码将会被用来加密通信，验证服务器，在进行真实的数据传输之前建立安全连接。

Tips: 建立连接一共需要8次往返

## 响应
一旦建立连接，浏览器会代替用户发送get请求，通常是html文件
使用TCP慢启动，即第一个响应数据包大小为14kB，后续每次翻倍直至网络拥塞或达到最大阈值

## 解析
解析是浏览器将网络接收的数据转换为Dom和Cssom的步骤，以便渲染器将其绘制成界面
1. **构建Dom树**
HTML 标记包括开始和结束标记，以及属性名和值。如果文档格式良好，则解析它会简单而快速。解析器将标记化的输入解析到文档中，构建文档树。
其中， html 是文档的根结点
当解析器发现非阻塞资源，例如一张图片，浏览器会请求这些资源并且继续解析。当遇到一个 CSS 文件时，解析也可以继续进行，但是对于script标签（特别是没有 async 或者 defer 属性的）会阻塞渲染并停止 HTML 的解析。
**预加载扫描器**: 它将在后台检索资源，以便在主 HTML 解析器到达请求的资源时，它们可能已经在运行，或者已经被下载。预加载扫描仪提供的优化减少了阻塞。如script文件中加入async关键字
2. **构建Cssom树**
它将在后台检索资源，以便在主 HTML 解析器到达请求的资源时，它们可能已经在运行，或者已经被下载。预加载扫描仪提供的优化减少了阻塞。通常速度很快
### 其他解析
1. JavaScript编译
当 CSS 被解析并创建 CSSOM 时，其他资源，包括 JavaScript 文件正在下载（借助预加载扫描器）。JavaScript 被解释、编译、解析和执行。脚本被解析为抽象语法树。一些浏览器引擎使用抽象语法树并将其传递到解释器中，输出在主线程上执行的字节码。这就是所谓的 JavaScript 编译。
2. 构建辅助功能树
浏览器还构建辅助设备用于分析和解释内容的辅助功能（accessibility）树。无障碍对象模型（AOM）类似于 DOM 的语义版本。当 DOM 更新时，浏览器会更新辅助功能树。辅助技术本身无法修改无障碍树。
在构建 AOM 之前，屏幕阅读器（screen readers (en-US)）无法访问内容。
## 渲染
渲染步骤包括样式、布局、绘制，在某些情况下还包括合成。在解析步骤中创建的 CSSOM 树和 DOM 树组合成一个 Render 树，然后用于计算每个可见元素的布局，然后将其绘制到屏幕上。
3. Style
**将 DOM 和 CSSOM 组合成一个 Render 树**
header和script标签中的内容不会包括到Render树中
4. Layout(布局)
**在渲染树上运行布局以计算每个节点的几何体。**
布局是确定呈现树中所有节点的宽度、高度和位置，以及确定页面上每个对象的大小和位置的过程。回流是对页面的任何部分或整个文档的任何后续大小和位置的确定。
**第一次确定节点的大小和位置称为布局。随后对节点大小和位置的重新计算称为回流。**
5. 绘制
**将各个节点绘制到屏幕上**

# 重绘(repaint)和重排(又叫回流 reflow)
- 重排一定导致重绘，反之则不一定
- 发生重排的场景:
    1. 页面初始渲染，这是开销最大的一次重排
    2. 添加/删除可见的DOM元素
    3. 改变元素位置
    4. 改变元素尺寸，比如边距、填充、边框、宽度和高度等
    5. 改变元素内容，比如文字数量，图片大小等
    6. 改变元素字体大小
    7. 改变浏览器窗口尺寸，比如resize事件发生时
    8. 激活CSS伪类（例如：:hover）
    9. 设置 style 属性的值，因为通过设置style属性改变结点样式的话，每一次设置都会触发一次reflow
    10. 查询某些属性或调用某些计算方法：offsetWidth、offsetHeight等
- 减少重排的一些方法
    1. 样式集中改变
    2. 分离读写操作，即读值和赋值分开操作
    3. 将Dom离线，即使用如display一类属性将元素从页面上移除，修改完毕后再加入页面
    4. 将其从文档流中脱离，如使用absolute或fixed函数
    5. 优化动画，使用transform 3d强制开启GPU加速
- 重绘指一个元素的外观发生改变，但没有改变布局,重新把元素外观绘制出来的过程，常见会引起重绘的场景:
color, border-style, visibility background, text-decoration background-image, background-position, background-repeat, outline-color, outline, outline-style, border-radiusout, line-width, box-shadow, background-size
