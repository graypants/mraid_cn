（美国）互动广告局

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

**IAB联系方式**

IAB移动营销中心高级总监：Joe Laszlo，mobile@iab.net
#	内容提要
在过去的几年中，由于在移动应用和移动网络上的富媒体展示广告已经变得越来越流行，各种创新型企业都面临创建一个移动广告投放生态系统的挑战。内容出版商和广告商的移动富媒体广告投放的创新导致了许多令人兴奋的可能性，但是它同样也导致了效率低下，经常延缓和阻碍最优的商业化内容。简化移动应用广告创意设计师的流程会大大增加代理商将移动纳入其媒体购买清单的意愿。不管是哪个移动设备平台，应用程序或技术用于显示媒体，广告主想要看到让人惊叹的创意，批准它并且决定购买指定的移动媒体。

# 	规范
下列术语用于整个MRAID规范。

**广告容器:**广告容器是一个显示广告创意的区域。它是一个提供屏幕区域的容器，我们称之为MRAID容器。它基于web视图，用于广告显示。SDK通常都会提供广告容器，但不是必须。应用程序可能包含来自一个SDK的多个广告容器。

**关闭广告区:**在广告创意中有一块可以轻触的区域叫做关闭广告区，它会引发广告回到默认状态（在缩放式广告中）或者从屏幕中整个移除（在插播式广告中）

**关闭指引:**关闭指引对于用户来说是一个视觉提醒，告诉他们关闭广告的位置。

**控制器:**控制器是一段提供给广告设计者访问MRAID方法和事件的JavaScript代码。广告创意使用控制器执行和广告容器之间的交互，也可以间接的调用应用程序和设备。

**Density-Independent Pixels**