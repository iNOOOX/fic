# 提醒：本程序没有客服！没有客服！没有客服！

FakeInChina(假装在中国) 


这原本就是一个写来自己用的小程序，并不是什么高深的科研项目，全部程序用Shell 的脚本写成。
用途：与“由于版权限制，你所在的地区不能播放”告别，目前支持大多数主流的视音频app，包括：youku、iqiyi、qq（音乐、视频）、网易、乐视、CNTV等等，数量太多，不全部列出了。
# 这个功能模块需要使用国内SS服务器
其实最早让 Hiboy 把ss-server集成到 PADAVAN 基础固件就是为了这一个模块，只是由于前一段时间基本上在国内，也就一直没有时间去调试这个模块，这段时间终于有时间和条件进行调试了。
模块运行时候，仅对检测服务器进行流量伪装（可能会包括部分网页文字以及图片），视音频码流依然直连线路，因此，对国内的SS服务器流量需求极低，一般普通家庭用的宽带便可以使用，在国内家人或者朋友的路由上运行SS-server，你在国外就可以正常使用。
毕竟大多数国内宽带IP是动态的，并且很多地区会限制时间自动断线，模块支持多台ss服务器备份，会自动检测可用的SS服务器，自动重连。
目前这个模块在我自己的路由上跑了将近半年了，前一段时间由于检测地区的网站在国内很难打开，因此升级了一个小版本，顺便开源了。

由于各大公司检测IP的方法可能会变，某些规则会出现失效，某些服务商针对不同地区会用不同IP的服务器进行检测（例如youku，美国和德国地区用于地区检测的服务器就不同），所以我仅能保证我所居住的德国地区可以使用正常。
如果你所用的app出现了地区限制，可以在github 发Issues，但我能处理的只有 ipad、iphone，至于安卓什么的，包括手机、电视盒、智能电视（德国的安卓智能电视很贵，我很穷，买不起），我统统没有，所以无法解决。并且如果是由于地区不同而导致检测服务器不同出现的地区限制，恕我无能为力。

# 系统原理

利用 ipset ，对域名、IP、IP段的流量进行监控，把所有检测IP的请求都通过国内的走，所幸的是大部分视频服务本身的码流服务器都不会检测IP，所以这个程序对流量的要求很低，一般家庭的宽度上行流量都可以支撑。

程序会建立 2 个ipset 的表项，你可以在 start.sh 里面找到：

tocn ：对于单个IP 的伪装

rtocn ： 对于IP段的伪装

在 start.sh 里面的独立IP设置是已知的无域名IP和无域名IP段，需要独立添加，所以在脚本里面处理了。对于有域名的，则利用 dnsmasq 的ipset 扩展，加入一个 r.tocn.conf 的配置文件，自动添加到 tocn 的ipset 表项里面。

原理就这么多了，有兴趣自己看源码。

# 数据来源

大部分需要伪装流量的IP列表和域名来自半年前的 unblockyouku 这个浏览器插件的 proxy 配置文件，我自己也只是添加了少量几个IP和域名，严格来说不超过10个. :D ，目前肯定有些过时，但由于我是一个懒人，我的APP正常的，我就懒得去管了。