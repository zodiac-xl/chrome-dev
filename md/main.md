###Chrome develop tools
Chrome develop tools是chrome提供的调试工具，其本质是一项工具，作为工具，旨在让大家了解它，然后形成自己的使用方法，方便我们调试网站

此次分享旨在分享个人调试日常总结，有新思路随时可随时打断



###常规调试网站流程

发现问题=》定位问题=》分析问题=》调试=》解决问题=》测试=》思考

正确调试的前提是正确定位问题

本次分享会从  **浏览器工作原理**  和   **开发者工具8大模块** 两方面帮助大家了解chrome和其开发者工具



###浏览器大致工作原理

* 浏览器的高层结构
* 呈现引擎详解
* JavaScript解释器详解



####浏览器的高层结构

1. **用户界面** - 包括地址栏、前进/后退按钮、书签菜单等。除了浏览器主窗口显示的您请求的页面外，其他显示的各个部分都属于用户界面。

2. **浏览器引擎** - 在用户界面和呈现引擎之间传送指令。

3. **呈现引擎** - 负责显示请求的内容。如果请求的内容是 HTML，它就负责解析 HTML 和 CSS 内容，并将解析后的内容显示在屏幕上。

4. **网络** - 用于网络调用，比如 HTTP 请求。其接口与平台无关，并为所有平台提供底层实现。

5. **用户界面后端** - 用于绘制基本的窗口小部件，比如组合框和窗口。其公开了与平台无关的通用接口，而在底层使用操作系统的用户界面方法。（驱动）

6. **JavaScript 解释器**。用于解析和执行 JavaScript 代码。

7. **数据存储**。这是持久层。浏览器需要在硬盘上保存各种数据，例如 Cookie。新的 HTML 规范 (HTML5) 定义了“网络数据库”，这是一个完整（但是轻便）的浏览器内数据库。


![浏览器的高层结构](http://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/layers.png)

>备注：和大多数浏览器不同，Chrome 浏览器的每个标签页都分别对应一个呈现引擎实例。每个标签页都是一个独立的进程。

看完结构图我们可以了解下呈现引擎和JavaScript 解释器工作流程



####呈现引擎的基本流程
关于呈现引擎；Firefox 使用的是 Gecko，这是 Mozilla 公司“自制”的呈现引擎。而 Safari 和 Chrome 浏览器使用的都是 WebKit。

WebKit 是一种开放源代码呈现引擎，起初用于 Linux 平台，随后由 Apple 公司进行修改，从而支持苹果机和 Windows。有关详情，请参阅 webkit.org。

呈现引擎的基本流程

![呈现引擎的基本流程](http://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/flow.png
)

这是一个渐进的过程。为达到更好的用户体验，呈现引擎会力求尽快将内容显示在屏幕上。它不必等到整个 HTML 文档解析完毕之后，就会开始构建呈现树和设置布局。在不断接收和处理来自网络的其余内容的同时，呈现引擎会将部分内容解析并显示出来。


WebKit 主流程

![](http://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/webkitflow.png)

Mozilla 的 Gecko 呈现引擎主流程 (3.6)

![](http://www.html5rocks.com/zh/tutorials/internals/howbrowserswork/image008.jpg)

Gecko 将视觉格式化元素组成的树称为“**框架树**”。每个元素都是一个**框架**。WebKit 使用的术语是“**呈现树**”，它由“呈现对象”组成。对于元素的放置，WebKit 使用的术语是“**布局**”，而 Gecko 称之为“**重排**”。



###浏览器的JavaScript引擎

单线程

* 词法分析器（Lexical Analyser）
* 句法解析器（Syntax Parser）
* 字节码生成器（Bytecode generator）
* 字节码解释器（Bytecode interpreter）


* 词法分析器的作用，是将一行行的源码拆解成一个个词义单位（token）。所谓“词义单位”，指的是语法上不可能再分的、最小的单个字符或字符组合。

		var sum = 30;

		// 词法分析后的结果
		[
		  "var" : "keyword",
		  "sum" : "identifier",
		  "="   : "assignment",
		  "30"  : "integer",
		  ";"   : "eos" (end of statement)
		]
		上面代码中，源代码经过词法分析后，返回一组词义单位，以及它们各自的词类。
		

* 句法解析器 是将上一步生成的数组，根据语法规则，转为抽象语法树（Abstract Syntax Tree，简称AST）
		
		{
		  operation: "=",
		  left: {
		    keyword: "var",
		    right: "sum"
		  }
		  right: "30"
		}

* 字节码生成器 字节码生成器的作用，是将抽象语法树转为JavaScript引擎可以执行的二进制代码

* 字节码解释器 字节码解释器的作用是读取并执行字节码。



###开发者工具8大模块

Chrome V35 版本中的开发者工具分为 8 个大模块，每个模块及其主要功能为：

* Console 标签页：用于显示脚本中所输出的调试信息，或运行测试脚本等。
* Element 标签页： 用于查看和编辑当前页面中的 HTML 和 CSS 元素。
* Source 标签页：用于查看和调试当前页面所加载的脚本的源文件。
* Network 标签页：用于查看 HTTP 请求的详细信息，如请求头、响应头及返回内容等。
* TimeLine 标签页： 用于查看脚本的执行时间、页面元素渲染时间等信息。
* Profiles 标签页：用于查看 CPU 执行时间与内存占用等信息。
* Resource 标签页：用于查看当前页面所请求的资源文件，如 HTML，CSS 样式文件等。
* Audits 标签页：用于优化前端页面，加速网页加载速度等。



###console

基础：

* console.info() 输出提示信息
* console.debug() 输出调试信息
* console.warn() 输出警示信息
* console.error() 输出错误信息

console.log() 是 printf 风格，因此你可以这么做：

	console.log("%s is %d years old.", "Bob", 42)。
	
console自定义

	console.log("%cThis is green text on a yellow background.", "color:green; background-color:yellow");

你可以输出多个值，用逗号隔开即可，方便在同一行输出对象和数组，例如：console.log('Kings: ', kings);

>不赞成使用 console.debug() ，因此没有图标标识，赞成用 console.log() 代替。


其他：

1. 重新运行命令

    如果已经在 JavaScript 控制台输入过命令，你想重新运行它，简单点一下“上”箭头即可翻看原先的命令列表。

2. 改变框架焦点

    切换document  （iframe等 插件document等）

3. 引用当前和前一个元素

    如果你的“Elements”标签选择了一个元素，你可以用 $0 调用它
    在 Chrome 和 Safari，你可以通过 $1 - $4 选择更靠前的元素。

4. 最终求值表达式

    如果你需要先前表达式的结果用于后面的求值，你可以使用便利 $_ ，最终表达式结果自动展示给你。



###element

* 查看dom的所有属性 方法 事件 样式

* 查看dom 有text和html 2种模式 （使用text编辑 然后查看html 即可得到字符实体） 拖拽重排dom
添加dom变动断点等 

* 模拟:hover等伪类状态

* 模拟点击等事件

* 添加样式规则 

* 放慢动画等

* 查看事件监听（筛选 祖先、JS框架自定义的事件类型）

* dom css js修改实时生效

    sourcemao的文件是根据源文件映射出来的（规则映射 或 文件映射）
    记得sourcemap的文件修改是不支持反向修改的  修改sourcemap文件并不会修改源文件 所以不会生效

* 通过CSS选择器搜索DOM元素

    CMD + F / CTRL + F和输入你需要的类名或ID名，可以搜索到相应的选择器。

>学习dom和css的权威方法  直接查看改元素的所有属性和方法 查看computerd下面支持的所有css属性



###Source 

用于查看和调试当前页面所加载的脚本的源文件 +

* 断点

* wokspace 使用本地文件编写代码  调试

* snippets 持久的可执行代码片段

* cotent scripts 插件相关代码

* 格式化代码等



###network

所有请求的详细日志request response 和各阶段的时间等

日常功能

* 持久日志
* 模拟网络
* 禁用cache
* 录制页面快照等
* 网络时间轴导出为 HAR 文件 qa测试等

timeline

* 排队时间发指令
* 挂起时间 浏览器得到要发出这个请求的指令，到请求可以发出的等待时间，一般是代理协商、以及等待可复用的TCP连接释放的时间，不包括DNS查询、建立TCP连接等时间
* Request sent 请求第一个字节发出前到最后一个字节发出后的时间，也就是上传时间
* Waiting 请求发出后，到收到响应的第一个字节所花费的时间(Time To First Byte)
* Content Download 收到响应的第一个字节，到接受完最后一个字节的时间，就是下载时间




###模拟

* 模拟User Agent
* 模拟触控设备
* 覆盖地理位置
* 重写设备方向
* 模拟网络情况



###timeline测量渲染性能和 FPS

Chrome 提供了帧率时间轴，可以逐帧调试网站。

要创建帧时间轴，首先打开 Chrome 开发工具，选择“Timeline”标签，底部黑色的圆形代表“record 记录”，按下开始时间轴记录，按第二次停止记录。记录的同时，你可以跟网页正常交互，帧模式会记录所有帧的行为。

**时间轴图表**

时间轴图表每一条代表单帧动画，不同的颜色表示如下：
蓝色表示加载任务，黄色表示脚本任务，紫色表示渲染任务，绿色表示绘制任务，白色区域代表空闲时间。

**性能的瀑布图**

* 时间轴图表下面是网页性能的瀑布图，帧显示在标题列，标题单元格还包含画这帧花费的毫秒数；相应的动作也记录在左侧。

* 你可以拖动两个灰色的鼠标滑块，改变你想检查的帧的数量。

* 悬停在时间轴的标题行显示帧的持续时间；FPS 数值；CPU 时间和与帧交互的合计时间。

主要查看 内存消耗，渲染消耗 和  js消耗



###resource

Resources标签页可以查看到请求的资源情况，包括CSS、JS、图片等的内容，同时还可以查看到存储相关的如Cookies、HTML5的Database和LocalStore等，你可以对存储的内容编辑和删除。



###Profiles标签页
这个主要是做性能优化的，包括查看CPU执行时间与内存占用



###Audits标签页

这个对于优化前端页面、加速网页加载速度很有用哦
它甚至可以分析出页面上样式表中有哪些CSS是没有被使用的哦



###移动端
####开发工具调试 IOS

把 IOS 设备连接到可用的 Mac，打开 Mac 和 IOS 设备的 Safari 。

找到 Safari 的“Develop 开发”菜单，第一个菜单分隔线的后面应该是你要连接的 IOS 设备的名字。在子菜单选择你想调试的页面。

Safari 的开发工具将以弹出窗口的式出现，你可以调试移动设备的站点了。


####开发工具调试 Android

你可以用Mac 上的 Chrome Canary 开发工具调试安卓设备上的公共和本地网站，你可以在手机上访问 Chrome 开发工具的广泛的特性集。

首先，需要在安卓设备安装 Chrome BETA 。

然后，需要下载 ADB 插件扩展，安装到 Chrome Canary 上。从未在 Google Play 商店下载过的话，需要拖拽文件到扩展页 （chrome://extensions/）。

确保已经在 chrome://flags 里面打开“Enable Developer Tools experiments 启用开发工具实验”。



###其他技巧

1. 展开所有子节点

    选择DOM元素和在带有剪头的地点按住Alt +点击鼠标左键，可以展开所有子节点

2. 通过CSS选择器搜索DOM元素
    
    CMD + F / CTRL + F和输入你需要的类名或ID名，可以搜索到相应的选择器。

3. 多个光标

    移动光标按住CMD + 点击可以添加多个光标，你也可以使用CMD + U撤销你选择的最后一个光标。

4. 复制图片的Data URI

    选择"Network"面板
    在“Resources”面板选择你的图片
    在图片上右击，选择“Copy Image as Data URI”选项

5. 通过拖拽选择多列

    选择“Sources”面板
    在“Sources”面板编辑器中选择你需要的文件
    按住Alt并拖动鼠标

6. console中的dom对象 可以在元素中显示

    在“Console”面板中右击
    选择"Reveal in Elements Panel"

7. Workspaces

    选择“Sources”面板
    在Sources面板中右击并选择“Add Folder to Workspaces”
    选择你的文件和右击，并选择Map to Network Resources
    修改你的文件代码和查看



