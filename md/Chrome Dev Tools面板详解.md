###面板

* 设置

* Console

* Elements

* Network

* Source

* Timeline面板

* Profile面板

* Resources面板

* Audits


###设置

1. 任务管理器：类是windows的任务管理器，管理chrome所有进程
2. 编码：默认为自动检测，对应meta标签的charset的值，设置浏览器解析文档的编码格式。
2. 开发者工具：打开chrom调试器，同F12和鼠标右键审查元素。
3. 显示源码：查看网页源码，同view-source:url和鼠标右键显示网页源码
4. JavaScript控制台：console控制台



###Console
除了常用的console.log外控制台还有一些额外的命令

1. 你可以在里面运行一些JavaScript表达式，操作当前页面的DOM，监听当前页面的事件等。
2. 使用$0-$4可以选择最近的5个DOM元素或JavaScript对象，使用$(选择器)可以选择元素，等价于document.querySelector(),$$(选择器)等价于document.querySelectorAll()，
3. $x(xpath)命令可以执行一个xpath查询，如$x(‘//img’);会找出页面中所有的图片，你也可以右键点击元素选择“在Elements面板显示”看对应的DOM，也可以通过inspect命令切换到审查元素模式
4. $_可以访问最近的控制台结果，清除控制台可以使用clear()或console.clear()命令。
当你访问新页面时如果不想清除控制台的信息可以点击右键，选择“Preserve Log upon Navigation”
5. console.profile和console.profileEnd可以检测JavaScript占用的CPU信息
profile

在控制台中有用的快捷键有：

* Ctrl+` 控制台获得焦点
* Ctrl+L 清除控制台
* Shift+Enter 多行输入



###Elements

Elements面板让你可以看到DOM树中所有的东西，并可以让你实时编辑DOM树。

1. 移动元素

	你可以直接拖拽元素来放置到DOM树中

2. 删除元素

	直接选择元素然后按delete键

3. 滚动到视图内

	想要把页面滚动到视图内，右键点击元素，选择”Scroll into View”
4. 设置DOM断点
	
	DOM断点和Source面板的断点类似，当指定的DOM属性、子元素发生变化时可以触发断点，在Elements面板的扩展面板里的DOM Breakpoints里可以看到设置断点的元素。

5. 查看事件监听器：
	
	Elements面板的扩展面板里的Event Listeners里可以查看绑定在DOM节点的JavaScript事件监听器。查看jquery绑定的事件：$(ele).data("events").click

6. Style扩展面板

	可以查看CSS属性源代码的位置，还可以添加和触发伪类如鼠标滑过时的CSS。

在Elements中有用的快捷键有：

* F2 以HTML格式来编辑切换
* Esc 停止修改DOM
* Ctrl+Z 撤销上一个操作
* Up/Down CSS属性值增加1/-1
* Alt + Up/Down CSS属性值增加0.1/-0.1
* Shift+Up/Down CSS属性值增加10/-10
* Shift+PageUp/PageDown CSS属性值增加100/-100



###Network
查看所有所有网络请求

1. XHRs

	你可以右击XHR请求选择“Replay XHR”来重新运行一个XHR请求，还可以点击preview来预览资源的内容，如果响应是json格式的话比较有用。

2. 清除网络缓存或cookie

	在Network面板右击选择“Clear Browser Cache/Network Cache”来清除缓存或cookie

3. 导出瀑布流

	在空白处右击，选择“Save as HAR with content”保存为一个HAR文件，然后可以在一些第三方工具网站如http://ericduran.github.io/chromeHAR/导入这个文件重现网络瀑布流情况。

4. 查看WebSocket

	在网络面板，还可以使用filters按钮来查看WebSocket消息结构。例如，打开http://www.websocket.org/echo.html，通过这个面板可以看到websocket的消息结构以及连接状况。

在Network中有用的快捷键有：

* Ctrl+F 根据关键词查找你想要的请求
* chrome://net-internals/ 可以查看网络状况



###Source

1. 格式化JavaScript

	选择你想要格式化的脚本文件，点击按钮pretty就可以了。

2. 调试XHR

	打开Source面板的debugger，在“XHR Breakpoints”下指定XHR的URL或URL部分，当发生XHR请求时会开启断点调试。

3. SourceMap

	点击设置->通用里的“Enable javascript source maps”或“Enable css source maps”可以调试
JavaScipt源代码、CoffeeScript，甚至Sass，只需要在你的源码加入类似 //@ sourceMappingURL=/path/to/file.js.map 即可。

4. Workspace

想让本地的文件夹在Source面板下可以编辑，右键单击Source面板选择“Add Folder to Workspace”，之后就可以直接编辑该目录下的任何文件，把Chrome当做一个编辑器来用。

5. Snippets

	有时你想保存小段的脚本、书签或是你在浏览器中调试时经常用到的，都可以使用Snippets，你可以在Source面板里创建、存储和运行这些Snippets.

在Source中有用的快捷键有：

* Ctrl+F 根据关键词查找你想要的请求
* Ctrl+Shift+F 在所有加载的文件下查找
* ctrl+o 打开一个js文件
* ctrl+p 同ctrl+o
* ctrl+f 查找当前js文件中的关键字
* ctrl+shift+f 全局查找关键字
* ctrl+shift+e 在控制台运行当前选中的代码片段



###Timeline面板

Timeline有三种模式来观察记录：

1. Events 根据事件分类显示所有记录的事件
2. Frames 显示页面的渲染性能
3. Memory 显示页面随时间的内存使用情况

布局是Chrome计算页面中所有元素的位置和尺寸的过程

**强制同步布局** 

Chrome会“懒惰的”进行布局以响应你应用中的CSS和DOM更新,这允许Chrome批量处理样式和布局的变化而不是一次响应一个变化。

然而，应用可以强制Chrome通过查询特定的布局独立元素属性（如element.offsetWidth）来立刻异步的实现布局。

如果重复的太频繁或者对大型DOM树进行操作的话，这种所谓的“强制同步布局”是影响页面性能的一大瓶颈，当你的应用引起强制异步布局时，

Timeline面板会识别出来并在旁边标记一个警告按钮（warning）。

当你选中这条记录时，细节面板将会包含这段突兀代码的栈追踪信息。

你还可以通过console.timeStamp()方法在Timeline记录中加入注释，这可以帮助你把你的代码和其它浏览器事件或活动联系起来。

在Timeline中有用的快捷键有：

* Ctrl+E 开始/结束录制


####Timeline面板名词解释：

send request	发送请求	evaluate script	评估脚本	parse html	解析html	recalculate style	重新计算显示样式
layout	计算布局	paint setup	绘制设置【准备绘制】	paint	绘制	composite layers	组合层
timer fired	触发定时器	function call	函数调用	receive data	接收数据	receive response	接收响应
finish loading	结束加载	GC event	浏览器垃圾回收	pevaluate script	评估脚本	rasterize	光栅化


###Profile面板

Profile里面提供了三种类型，通过这三种分析可以查看内存泄露和占用情况。

1. 收集JavaScript的CPU占用信息
2. 获取堆的快照
3. 录制堆的分配情况

>视图列表通过对性能的影响列出函数，允许我们检查这些函数的调用路径。

在Profile中有用的快捷键有：

* Ctrl+E 开始/结束录制



###Resources面板

Resources可以让你检查应用的本地数据资源，包括

* indexedDB/WebSql DB
* localStorage/sessionStorage
* cookie
* AppCache

>我们可以检查应用的视觉资源，包括图片、字体和样式等。比如AppCache中，你可以检查Chrome已经缓存了当前文档的哪些资源以及AppCache的状态和浏览器连接情况。

AppCache的资源包括以下属性：

* Resource – 资源的URL
* Type – 缓存资源的类型，可能是下面值的一种
* Master – 这个资源的manifest属性表明这是它的缓存，这个资源也被加到了缓存中
* Explicit – 这个资源被列在manifest文件中
* Network – 这个资源被列在manifest文件中作为network访问
* Fallback – 资源不可用时该资源被指定为fallback
* Size – 缓存资源的大小



###Audits

这个对于优化前端页面、加速网页加载速度很有用哦
它甚至可以分析出页面上样式表中有哪些CSS是没有被使用的哦


