（美国）互动广告局（简称IAB）

移动富媒体广告接口规范（MRAID）2.0版本

发布于2012.9.27

最后修订时间2013.4.16


#	贡献者
IAB MRAID的工作小组包括来自以下公司的代表：
<pre>
24/7 Real Media, Inc. 					Medialets
AccuWeather.com 						MediaMind
AdMarvel								Microsoft Advertising
AdMeld									Mixpo
ADTECH									Mocean Mobile
Adobe Systems Inc. 						NBC Universal Digital Media
AOL										New York Times Co.
CBS Interactive							Nexage
Celtra									Pandora
Crisp Media								PointRoll
Dow Jones & Company						Rhythm NewMedia
ESPN									Spongecell
FreeWheel								Sprout
Goldspot Media							TargetSpot
Google									Time Inc.
Greystripe								Turner Broadcasting System, Inc.
IDG										Univision
inMobi									The Weather Channel
Innovid									Yahoo!, Inc.
Jumptap
</pre>

#	感谢
IAB感谢ORMMA.org API项目贡献者，给本文档提供了一个起点。ORMMA.org是一批行业思想领袖自2010年春季以来一起开发和测试的一个完整灵活的移动富媒体广告API。在IAB推出MRAID项目以前，ORMMA的贡献者包括：
<pre>
Adam Schuetz, AdMarvel					Philippe Laporte, Goldspot Media
Dennis Doughty, Jumptap					Robert Hedin, The Weather Channel
Jon Badenell, The Weather Channel		Todd Pasternack, Pointroll
Nathan Carver, Crisp Media				Wook Chung, Google
Neal Karasic, Jumptap					Xavier Facon, Crisp Media
</pre>

#	关于MRAID
(美国）互动广告局（简称IAB），其成员和其它重要贡献者联合创建这份文档，对于移动富媒体广告来说，它是一份标准的接口规范。MRAID项目的目的是为了解决出版商的移动应用、不同的广告服务器和不同的富媒体平台之间已知的互操作性问题。

##	IAB联系方式

IAB移动营销中心高级总监：Joe Laszlo，mobile@iab.net

#	内容提要
在过去的几年中，由于在移动应用和移动网络上的富媒体展示广告已经变得越来越流行，各种创新型企业都面临创建一个移动广告投放生态系统的挑战。内容出版商和广告主的移动富媒体广告投放的创新导致了许多令人兴奋的可能性，但是它同样也导致了效率低下，经常延缓和阻碍最优的商业化内容。

简化移动应用广告创意设计者的流程会大大增加代理商将移动纳入其媒体购买清单的意愿。不管是哪个移动设备平台，应用程序或技术用于显示媒体，广告主想要看到让人惊叹的创意，批准它并且决定购买指定的移动媒体库存。

# 	规范
下列术语用于整个MRAID规范。

**广告容器：**广告容器是一个显示广告创意的区域。它是一个提供屏幕区域的容器，我们称之为MRAID容器。它基于web视图，用于广告显示。SDK通常都会提供广告容器，但不是必须。应用程序可能包含来自一个SDK的多个广告容器。

**关闭广告区：**在广告创意中有一块可以轻触的区域叫做关闭广告区，它会引发广告回到默认状态（在缩放式广告中）或者从屏幕中整个移除（在插播式广告中）

**关闭指引：**关闭指引对于用户来说是一个视觉提醒，告诉他们关闭广告区的位置。

**控制器（Controller）：**控制器是一段提供给广告设计者访问MRAID方法和事件的JavaScript代码。广告创意使用控制器执行和广告容器之间的交互，也可以间接的调用应用程序和设备。

**设备独立像素（DIP）：**通过MRAID API，在控制器和创意之间传递的所有长度值都是设备独立像素。

DIP是对物理屏幕像素的一个抽象，旨在简化应用和内容开发跨设备。

使用DIP意味着，例如：视网膜显示屏的iPhone和老的iPhone将返回相同的尺寸，尽管它们之间有着不同的物理像素值。1 DIP大约相当于160分之一英寸（在大约160 DPI设备上的1设备像素）.

在iOS中，这些对应到“点”；Android中，对应到“设备独立像素”。

注意：只有视口比例为1.0时1 DIP才等于1 CSS像素。为了对应CSS像素和DIP，创意应使用如下公式：

`css像素 * 视口比例 = DIP`

**内嵌式广告：**伴随着其它内容一起出现在屏幕上的一种广告，例如网页上或者app内的一个banner。

**插播式广告：**一种显示在内容上方的全屏广告——也被叫做“障碍物”或者“覆盖物”。这类广告必须驳回用户返回到内容的请求（译者注：也就是不允许主动关闭广告）。一些插播式广告可以出现在游戏关卡之间，或者一段视频剪辑的前后，再或者其他动态内容中。（在许多杂志类app中，滑动页面之间的广告在MRAID规范下被认为是内嵌式广告。）

**物理像素：**设备屏幕的实际像素。例如，视网膜显示屏的iPhone的物理像素为960x640。MRAID API长度值总是使用DIP（上文定义的）来计算，而不是使用物理像素。

**SDK：**软件开发工具包。一段可重复使用的代码（类库），集成在出版商app中以激活广告容器。一个SDK，其本身不是一个可视化组件。

**Web View：**基于HTML显示广告创意的视图。Web View是用来执行渲染使用HTML和JavaScript编写的广告。

![规范](./images/a.jpg)

#	支持MRAID的基本须知
本节详细介绍兼容MRAID的应用程序内广告投放SDK的要求。

预计实现将分为两部分。第一部分为富媒体广告在app中的显示定义一个本地容器，第二部分为广告创建者定义一个JavaScript控制器交互用。本地容器封装一个支持HTML和JavaScript的Web View，比如iOS中的UIWebView，控制器充当一个桥梁可以将本地功能集成到基于HTML的广告中。具体实现或许有所不同。

在规划时，关键的设计考虑因素：

* 在应用程序发布者和广告卖家允许的情况下，以一致的方式访问设备的本机功能（方向、位置、加速度等。）
* 行业标准的广告开发（HTML和JavaScript）
* 渐进式的复杂度（简单的事情很简单，复杂的事情都是可能的，但更难）

##	技术受众
规范文档按性质来说是技术性的，但这并不妨碍我们创新。这份文档主要是针对出版商或SDK供应商，并解决广告设计者的需求。
>###	本地应用开发者
在这份规范中对于本地应用开发者没有任何要求。他们应该遵循他们的SDK开发者提供的用于集成广告到他们应用程序的说明。
>###	SDK开发者
SDK架构者有这些建议以外的责任（参考“超出范围”）。正如前面提到的，预计SDK开发者将提供两个接口用于实现这些建议：通过SDK给本地开发者一个控制器用于集成，给广告设计者一个控制器直接使用。
>本文档概述了广告设计者所需要的控制器要求。作者意图对于已有的SDK，这些概念可以用一个表层来管理。
>###	广告设计者
本文档对于广告设计者来说，除了使用标准的web，没有有关创意的要求。使用了本规范中方法的广告设计者，可以提供给消费者丰富的媒体体验，并且同时跨平台和出版商。

对于广告设计者来说，认识到调用本机设备必须是异步的（设计使然），这一点非常重要。对于大多数web开发者，这种情况类似于AJAX编程。

##	视觉窗口和默认的容器设置
创意设计者应该意识到需要了解——他们正在运行的程序可能会覆盖默认的web视图设置。他们应该查询MRAID控制器，就像他们为，了解环境查询一个网页那样做一样。这些设置包括容器的高、宽、比例、用户是否可以改变容器的比例。

在Web视图中，虽然MRAID并不建立任何新的参数或控制，也建议检查和校准参数，除非创意的作者说：我们不妨把他们设置的不同于默认容器设置。

##	超出范围
每条MRAID规范提供给开发者唯一的功能集。这份文档为互操作概述了一个最小的功能集，那些可能是SDK中的一部分的功能，它并不定义，例如：

* 从广告服务器，广告网络或本地资源检索广告
* 报告
* IDE集成
* 安全/隐私
* 国际化
* 错误报告
* 日志
* 结算和付款
* 广告尺寸和广告行为（MRAID不定义广告尺寸或者用户和广告互动过程中，广告如何移动和改变）
* 为了缓冲或者离线使用下载资源到本地文件系统

当然，对于广告模块，SDK开发者必须实现在特定区域渲染web内容的功能。虽然一部分开发者可能不得不开发额外的功能，但是对于大部分环境来说，要支持这些规范，这个功能作为一个web view组件已经可用。

作者的意图是：供应商（SDK提供者）不限于只开发API中描述的功能。他们应该不断创新、不断推出区别于市场上已存在的新功能。

一个只使用SDK特定功能、抛开MRAID功能的广告，从MRAID跨平台的意义上来讲，已不能算作一个标准MRAID广告。

##	标准Web技术
对于互操作性，只有web兼容的语言应被用于标记和脚本语言。本文假定使用HTML/JavaScript/CSS。广告设计者应能够在浏览器上开发和测试广告模块。如果广告设计者使用的标记、样式、方法只兼容一种浏览器（例如WebKit CSS3），那么这个广告会有针对性（只针对WebKit CSS3）的兼容设备。

当新的web标准统一提供时，广告设计者们被鼓励使用它们。这些或许包括像短信和电话这样的草案，以及一些广泛实施，但尚未完成的HTML5规范。设计师需要了解，在这些情况下，预期的协议和实现，可能不是真正的跨所有设备和平台。

##	广告服务器需知
用于传输富媒体广告的服务器应当支持使用JavaScript的HTML广告。

##	广告渲染需知
###	显示HTML广告——广告容器
一个MRAID兼容的广告容器必须能显示任何HTML广告。容器应调用HTML与JavaScript渲染引擎渲染广告。在本文中，渲染引擎将被称作web view。web view需要尽可能的体现设备的浏览器特性。比如，iOS开发者或许会使用UIWebView。一个给定的App视图可以有一个或多个广告视图容器，它们都将相互独立的运行而互不干涉。

##	广告设计者需知
###	控制富媒体广告显示——广告控制器
广告设计者如果希望他/她的广告使用MRAID，需要尽可能的在广告载入之前通过调用mrad.js做出指示。这标志着SDK向素材中注入MRAID javascript代码。

MRAID保持在后台运行，把控制广告显示的权利留给广告设计者，当创意需要使用MRAID API时（如果广告需要访问MRAID特性和功能），它又是可用的。创意和富媒体SDK之间的内部交互，对于广告设计者和App开发者来说，都是隐藏的。

一个不利用任何设备特性的广告根本不需要使用MRAID API。然而，如果广告不调用MRAID，它将得到SDK的默认解决方案。一些广告使用MRAID API的例子：

* 打开一个嵌入式的Web浏览器
* 检测广告是否可见
* 扩大一个广告，比如从banner变成更大尺寸
* 点击广告内触发一个动作

我们鼓励广告设计者依靠MRAID来达成上述效果。

##	生存周期实例
###	简单型广告生存周期实例
非富媒体广告（例如基础banner）可以选择性地调用MRAID。如果广告没有调用MARID（通过mraid.js），那么应用程序或者SDK通常会处理此类广告。

如果广告设计者更想使用MRAID标准容器，而不是SDK默认容器，那么他们可能希望为这种简单的HTML广告调用mraid.js。在这种情况下，广告设计者必须确保对于任何链接都使用`mraid.open()`打开，以保持统一的行为。

###	MRAID展开式广告生存周期实例
在富媒体广告生存周期实例中，广告设计者使用JavaScript API和原生层通信，同时和设备及操作系统的功能交互。
![生存周期](./images/b.jpg)
举个例子，当用户触摸广告时，广告会使用MRAID API请求展开自己。SDK（虽然它不是MRAID规范的一部分）应通知APP广告即将展开，以便于App停止任何事情，用户此时将不能和App交互。SDK随后调整web view，占据设备的整个屏幕或者全尺寸的扩大广告。展开后的广告容器会在右上角保留一片空白，作为MRAID强制性关闭广告区，如果广告指定，允许提供创意内的关闭指引，否则使用默认的关闭指引。

当用户完成展开式广告后，他们点击关闭按钮触发广告回到它们原始大小，显示广告的banner状态，同时通知App可以继续运行。

###	MRAID插播式广告生存周期
插播式广告的情况和上述非常类似。无论广告容器在屏幕上可见或不可见，广告都可以使用MRAID API查询它，在采取其它行动之前一直等到广告容器在屏幕上可见。和展开式广告一样，展开后的广告容器会在右上角保留一片空白，作为MRAID强制性关闭广告区，如果广告指定，允许提供创意内的关闭指引，否则使用默认的关闭指引。当用户完成插播式广告后，轻触关闭按钮，此时广告的状态变为hidden，注销所有事件监听，同时通知App继续运行。

##	MRAID版本
采纳MRAID标准在整个广告社区是一项高优先级的事务，并且对于移动富媒体广告跨平台的成功有着至关重要的作用。出于这个原因，IAB发布具有完整功能集的MRAID版本。这将允许SDK供应商以一致的方式见到符合标准的MRAID API，防止在实现中仅用一部分标准可能出现的碎片继承。

在MRAID中，维护完全向后兼容是这个项目的一个关键目标。无论如何，一个MRAID2.0兼容的SDK应能够毫无问题的运行一个MRAID1.0广告，一个MRAID1.0兼容的SDK应可以处理一个MRAID2.0广告中MRAID1.0兼容的特性。

在确立的两个MRAID版本中，IAB和其MRAID工作组侧重于六个关键目标：

* __高互用性__ —— 开发运行在一个MRAID容器中的广告可以运行在多个平台和操作系统的MRAID容器中。
* __优雅降级__ —— 利用所有MRAID特性开发的广告也有能力根据需要优雅降级。在未来，当获取设备的功能访问权变成MRAID的一部分时，这一点尤为重要。
* __渐进的复杂性__ —— 广告设计使用的API应该是简单的，只在必要时增加复杂性。
* __以统一的方式改变广告大小和打开新页面，最好都在嵌入式浏览器中__ —— 对于展开和打开App内嵌浏览器（或者本机浏览器）的行为，MRAID提供的广告，都以统一的方式和富媒体SDK通信。
* __消费者一致的方法退出广告__ —— MRAID广告总是有相同的退出控制，通过它，用户可以表明他们想退出广告，并返回到他们的应用程序或内容中。
* __出版商的灵活性__ —— 虽然符合MRAID规范的SDK必须支持所有的MRAID特性，但是App发布者、广告卖家可以灵活的允许或禁止使用MRAID特性的广告。也就是说，MRAID允许富媒体广告功能，但并不规定所有卖家的富媒体广告必须支持所有这些功能。

###	1.0版本
MRAID1.0中的方法和事件，为富媒体广告提供了一个最低级别的需求，主要用于显示一个在固定容器中可改变大小的HTML广告（例如，把一个广告从banner扩展到更大尺寸或全屏）和插播式广告。

###	2.0版本
MRAID2.0继承了MRAID1.0的特性，不但在展开式广告之上，给广告设计者们更多的控制权，并且提供了一个新的方法`resize()`，使广告创意中更微妙更有趣的尺寸改变成为可能。

另外，MRAID2.0规定了关于查询设备某些功能的标准方式，提出处理视频创意的统一处理方法，并且解决了目前HTML5没有很好实现的两个本地特性：给设备日历添加一个条目和向设备相册存储图片。

对于可以使用MRAID2.0 API开发的广告示例，请看附录部分。

#	接口需求和定义
下列清单概括了在MRAID2.0下，广告设计者有权访问的所有方法和事件。MRAID2.0中新增的方法和事件用*标识。

**方法**
<pre>
	addEventListener					getVersion
	createCalendarEvent*				isViewable
	close 								open
	expand 								playVideo*
	getCurrentPosition*					removeEventListener
	getDefaultPosition*					resize
	getExpandProperties					setExpandProperties
	getMaxSize*							setResizeProperties*
	getPlacementType					storePicture*
	getResizeProperties*				supports*
	getScreenSize*						useCustomClose
	getState
</pre>

**事件**
<pre>
	error 								stateChange
	ready 								viewableChange
	sizeChange*
</pre>

##	标识
广告必须标识它们自己是符合MRAID规范的。完成这项工作，通过尽早（在任何MRAID函数被创意引用之前）的加入MRAID脚本引用。换言之，MRAID标识脚本必须被符合MRAID规范的容器或SDK最先识别出来。

**MRAID**脚本引用

MRAID标签遵循HTML JavaScript语法，以便完整的网页和HTML片段都可以被标识为MRAID广告。mraid.js作为一个脚本将被引入到文档中，或者使用HTML标签，或者使用JavaScript代码。MRAID示例广告（见[www.iab.net/mraid](http://www.iab.net/mraid)）说明了脚本引用标签应该放在哪儿。

`<script src="mraid.js"></script>`

虽然MRAID广告需要尽早的标识它们自己（例如像这样通过mraid.js脚本）以便于容器可以注入MRAID类库，但是当广告创意中为了其它目的时，广告设计者也应避免使用mraid.js字符串，因为这样做可能会导致容器或者SDK错误的注入多个MRAID类库副本。

##	初始化
MRAID管理广告和容器之间的交互，并且标识容器是兼容MRAID规范的。广告设计者必须为MRAID包含JavaScript引用，但是实际的JavaScript类库由容器提供；容器的职责是在脚本引入后，确保它们对广告尽早的可用，完成这些则通过触发ready事件来告知可用。

下面分步骤总结了从广告初始化加载到MRAID容器注入API类库的行为。

1. 调用MRAID脚本标签`<script src="mraid.js"></script>`，标识为MRAID广告。
2. SDK/MRAID兼容的容器

	a. 可选的检测脚本调用

	b. 总是为MRAID广告提供MRAID JavaScript桥接

	c. 提供一个state = 'loading'和具有查询状态功能的受限对象

3. 如果广告使用createElement，需要等mraid.js完成加载
4. 如果state = 'loading'，那么广告使用`mraid.addEventListener('ready')`监听ready事件
5. SDK/容器完成向webview中初始化MRAID类库
	
	a. 变更state为default，同时StateChange事件被触发

	b. 触发MRAIDready事件

6. 广告的ready事件监听被触发，此时广告JavaScript代码可以使用所有MRAID API

**ready**事件

ready事件在容器完全加载时触发，完成初始化并且准备好来自广告创意的任意调用。

在加载广告创意之前，MRAID容器负责预备API方法。这会阻止一种情况：因为API方法不可用导致的广告不能为ready事件注册监听。虽然容器可以一次性加载所有的MRAID API，但在广告加载过程中，容器最少要准备好getState和addEventListener；否则将没有办法为广告注册ready事件。在事件中，容器可能仍然需要较多的时间初始化设置或者准备附加功能，只有当它完全准备好接受MRAID请求时，ready才应被触发。

在执行任何富媒体操作之前，广告应该总是试图等待ready事件。由于时间问题，比如要在广告注册监听之前触发ready事件，广告设计者可以配合getState()方法来使用ready事件。

例子

```
function showMyAd() {
    ...
}
if (mraid.getState() === 'loading') {
    mraid.addEventListener('ready', showMyAd);
} else {
    showMyAd();
}
```

“ready”
<pre>
参数：
· none
侧面影响：
· 使MRAID JavaScript类库对广告模块可用
返回值：
· none
触发事件：
· none
</pre>

**getVersion**方法

getVersion方法允许广告在显示前确认基础功能集。版本号必须符合MRAID规范（例如1.0或者2.0），不是供应商的SDK版本号。

getVersion() -> String
<pre>
参数：
· none
返回值
· String – MRAID兼容的SDK版本号或者IAB针对性认证的SDK版本号。举个例子，对于本文的版本号，getVersion()讲返回“2.0”
</pre>

##	初始显示
当其它资源在后台加载时，广告设计者最好提供一段简单的HTML，比如一个`<img>`标签，用于他们广告的初始化显示。在JavaScript使用控制器请求调用额外功能时，这段HTML将显示在容器中。最终，一旦所有的资源准备完成，初始显示用的HTML可能就会完全被富媒体广告替代，这些视创意的需求而定。

##	事件处理
事件处理是MRAID的关键概念。web层和原生层的通信本质上异步的。通过事件处理，广告设计者能够监听特定的事件，并且根据需要对这些事件作出回应。MRAID主张广播式事件，在保持最强统一性的同时，支持最广泛的功能性/灵活性。

控制器开放这些方法。

**addEventListener**方法

使用本方法为特定事件订阅一个指定的处理方法。如此以来，多个监听器可以订阅一个特定事件，一个监听器可以处理多个事件。在同一时间，一个广告可能注册不止一个监听器，前提是广告必须允许这样做。MRAID2.0支持的事件：

<table border="1" cellpadding="3">
	<tr>
		<th>事件</th>
		<th>描述</th>
	</tr>
	<tr>
		<td>ready</td>
		<td>报告初始化完成</td>
	</tr>
	<tr>
		<td>error</td>
		<td>报告发生错误</td>
	</tr>
	<tr>
		<td>stateChange</td>
		<td>报告状态变化</td>
	</tr>
	<tr>
		<td>viewableChange</td>
		<td>报告显示状态变化</td>
	</tr>
	<tr>
		<td>sizeChange</td>
		<td>报告广告的尺寸信息变化</td>
	</tr>
</table>

addEventListener(event, listener)
<pre>
参数：
· event – 字符串，要监听的事件名称
· listener – 执行监听的函数
返回值：
· none
侧面影响
· none
</pre>

**removeEventListener**方法

使用本方法从一个特定的事件退订指定的处理方法。当事件监听器不再有用时，它们应该总是被移除，以避免出错。如果没有监听功能被指定，那么正在监听事件的所有函数将被移除。

removeEventListener(event, listener)
<pre>
参数：
· event – 字符串，事件名称
· listener – 要被移除的函数
返回值：
· none
触发事件：
· none
</pre>

##	错误处理
当容器中发生错误，带有关于事件错误诊断信息的“error”事件会被触发。任意数量的监听器可以用来监控不同类型的错误，以根据需要进行响应。

**error**事件

每当容器错误发生时，该事件就被触发。该事件包含错误发生时的描述，以及，在适当的时候，包含导致错误的操作名称（没有关联动作时，动作参数为null）。JavaScript错误仍然是广告设计师的全部责任。

“error” -> function(message, action)
<pre>
参数：
· message: 字符串，错误描述
· action: 字符串，引发错误的动作
经由什么触发：
· 任何出错
</pre>
广告设计者需要注意：错误可以通过底层SDK/容器以异步或同步的方式来处理。

MRAID规范没有定义错误事件中的“message”部分，并且主要计划用于广告发布前的创意调试，错误的“action”部分始终是发生错误时尝试使用的方法名称。原则上讲，任何MRAID方法都可能引发错误，因此广告设计者在使用错误监听器时应监听下列所有可能的错误动作：
<pre>
	addEventListener					getVersion
	createCalendarEvent 				isViewable
	close 								open
	expand 								playVideo 
	getCurrentPosition 					removeEventListener
	getDefaultPosition 					resize
	getExpandProperties					setExpandProperties
	getMaxSize 							setResizeProperties 
	getPlacementType					storePicture 
	getResizeProperties 				supports 
	getScreenSize 						useCustomClose
	getState
</pre>
虽然任何MRAID方法都可能导致错误，在实践中，使用resize、添加一张图片到相册或者添加一个事件到日历，是最易产生错误的MRAID方法。广告设计者们在使用这些方法时，应该格外注意添加错误监听器，来检查在执行resize、storePhoto、createCalendarEvent动作时是否发生错误，以便于广告创意有可能采取不同的动作。

##	控制广告显示
除了广告初始化显示，广告设计者或许会有各种理由需要控制广告显示。

* 为了解决广告请求延迟问题，应用程序可能在后台加载视图，但是对用户不可见。
* 广告可能扩展到超出应用程序内容的默认大小。
* 一旦用户交互完成，广告可能返回到默认尺寸。

**getState**方法，**stateChange**事件

每个广告容器（或者web view）都有下列其中之一的状态：

<table border="1" cellpadding="3">
	<tr>
		<th>状态</th>
		<th>描述</th>
	</tr>
	<tr>
		<td>loading</td>
		<td>容器还没有准备好和MRAID实现交互</td>
	</tr>
	<tr>
		<td>default</td>
		<td>应用程序和SDK设定好广告容器的初始位置和大小</td>
	</tr>
	<tr>
		<td>expanded</td>
		<td>广告容器已展开覆盖到应用程序内容的最顶层</td>
	</tr>
	<tr>
		<td>resized</td>
		<td>广告容器已改变大小（通过MRAID2.0 resize()方法）</td>
	</tr>
	<tr>
		<td>hidden</td>
		<td>插播式广告转变为关闭后的状态。在被支持的情况下，banner广告转变为关闭后的状态</td>
	</tr>
</table>

getState方法返回广告容器的当前状态。

当状态被广告（或者环境）程序改变时，会触发stateChange事件。当广告视图在default、expanded、resized、hidden状态之间改变时触发stateChange事件（这些状态都是在调用expand(), resize(), close()产生的）。容器或SDK也可以关闭广告作为用户或系统操作的结果，比如从后台恢复运行。

任何MRAID广告在同一时间只能有一个状态。在two-part展开式广告中，这个要求意味着对于所有web view都只有一种状态。对于只要在屏幕上展开的视图，使用getState()都将返回“expanded”。

调用expand(), resize(), close()对于状态的影响定义如下表：

如何阅读：创意的初始状态在表格最左列；当使用一个MRAID方法时，可以从方法名那列往下查到状态如何变更。举个列子，如果广告的状态是“expanded”并且同时使用了close()方法，那么这个广告的状态变为“default”。

<table border="1" cellpadding="3">
	<tr>
		<th>初始状态</th>
		<th>expand()</th>
		<th>resize()</th>
		<th>close()</th>
	</tr>
	<tr>
		<td>loading</td>
		<td>无影响</td>
		<td>无影响</td>
		<td>无影响</td>
	</tr>
	<tr>
		<td>default</td>
		<td>对于banner广告，状态变为“expanded”，插播式广告则无影响</td>
		<td>对于banner广告，状态变为“resized”，插播式广告则无影响</td>
		<td>对于banner广告，状态变为“hidden”（如果SDK/容器支持的话），插播式广告状态则变为“hidden”</td>
	</tr>
	<tr>
		<td>expanded</td>
		<td>无影响（仍然保持“expanded”状态）</td>
		<td>引发错误；（仍然保持“expanded”状态）</td>
		<td>状态变为“default”</td>
	</tr>
	<tr>
		<td>resized</td>
		<td>状态变为“expanded”</td>
		<td>状态变为“resized”（也就是说，事件监听器将监听到一个新的stateChange事件，尽管事件触发后状态依旧是“resized”）</td>
		<td>状态变为“default”</td>
	</tr>
	<tr>
		<td>hidden</td>
		<td>无影响</td>
		<td>无影响</td>
		<td>无影响</td>
	</tr>
</table>

在two-piece展开式广告中，一种新类型广告，TODO紧接着“ready”事件被触发，然后广告的状态变为“expanded”。two-piece广告中的banner（第一片）同样也将它的状态从“default”变为“expanded”。

对于插播式广告，web view从“loading”到“default”，当广告关闭时，状态变为“hidden”。

getState() -> String
<pre>
参数：
· none
返回值：
· String: "loading"或"default"或"expanded”或“resized”或“hidden” 
相关事件：
· stateChange
</pre>

“stateChange” -> function(state)
<pre>
参数：
· state - String, "loading"或"default"或"expanded”或“resized”或“hidden” 
经由触发：
· expand()方法、resize()方法、close()方法或者App
</pre>

**getPlacementType()**方法

为了提高效率，广告设计师有时会在banner广告和插播式广告放置点全都使用简单的创意。所以创意应能够知道他们的放置类型，并因此引发不同的行为，每个广告容器都有一个放置类型，来确定广告是嵌入在内容中显示（即banner）还是作为插播覆盖到内容上（比如在内容传输过程中）。容器返回放置类型的值以便于创意在需要时引发不同的行为。容器并不确定banner是否能展开（这个由创意决定），因此不会专门为展开式广告返回一个放置类型。

<table border="1" cellpadding="3">
	<tr>
		<th>值</th>
		<th>描述</th>
	</tr>
	<tr>
		<td>inline</td>
		<td>默认的放置类型是嵌入到内容中显示（即banner）</td>
	</tr>
	<tr>
		<td>interstitial</td>
		<td>这种广告放置类型是覆盖在内容上面</td>
	</tr>
</table>

getPlacementType应该总是返回广告最初显示时设定的值。也就是说，在two-part展开式广告中，如果调用getPlacementType，第二部分（展开的部分）应当同样返回“inline”。

getPlacementType() -> String
<pre>
参数：
· none
返回值：
· String: "inline", "interstitial" 
相关事件：
· none
</pre>

**isViewable**方法，**viewableChange**事件

除了广告容器的状态，有一种可能情况就是，为了协助带来流畅的用户体验，广告容器作为App缓存区的一部分已经在屏幕之外加载。在插播式广告中或者采用滚动视图的App中，这种方式格外流行，比如在游戏关卡之间。

isViewable方法返回广告容器当前是否在屏幕上。当广告从屏幕上离开时会触发viewableChange事件，反之亦然。对于two-piece展开式广告，当广告状态为expanded时，isViewable将基于可见的expanded piece返回一个值。

广告在任何情况下都有可能在屏幕外加载，对于广告来说，最佳实践是：检查自身可见状态或者在采取任何行动之前注册viewableChange监听事件。

注意：MRAID并不定义在屏幕上构成广告可见性的最小像素或百分比阀值。（IAB内部目前有一个进行中的项目，为App中的广告尺寸建立指导方针，可能会引入可见性阀值）

isViewable() -> boolean
<pre>
参数：
· none
返回值：
· boolean - 
true：容器在屏幕上并且对用户可见；
false：容器不在屏幕上并且不可见
相关事件：
· viewableChange
</pre>

“viewableChange” -> function(boolean)
<pre>
参数：
· boolean - 
true：容器在屏幕上并且对用户可见；
false：容器不在屏幕上并且不可见
经由触发：
· 应用程序视图控制器的改变
</pre>

下面是一个在行动之前同时考虑到“ready”和“isViewable”的广告示例。

```
// Wait for the SDK to become ready
if (mraid.getState() === 'loading') {
	mraid.addEventListener('ready', onSdkReady);
} else {
	onSdkReady();
}

function onSdkReady() {
	// Wait for the ad to become viewable for the first time
	if (mraid.isViewable()) {
		showMyAd();
	} else {
		mraid.addEventListener('viewableChange',function(viewable) {
			if (viewable) {
				mraid.removeEventListener('viewableChange', arguments.callee);
				showMyAd();
			}
		}); 
	}
}

function showMyAd() {
... 
}
```











