# V2Ray搭建介绍    

**什么是V2Ray(2024/02/15更)**  

Project V 提供了单一的内核和多种界面操作方式。内核（V2Ray）用于实际的网络交互、路由等针对网络数据的处理，而外围的用户界面程序提供了方便直接的操作流程，简单来说，V2Ray就是一个代理软件，可以用来科学上网学习国外先进科学技术。

<br>

**V2Ray与Shadowsocks区别**

V2Ray是在Shadowsocks的作者被请喝茶之后出现的一个开源项目，目的就是为了更好的科学上网。相比于ss，V2Ray的定位是一个平台，任何开发者都可以在这个平台上利用V2Ray开发出一个新的代理软件，简单来说，ss的定位比较简单，功能也比较单一，而V2Ray的功能非常强大，相对的，V2Ray的**配置就会复杂很多**，喜欢鼓捣的同学可以试试。

<br>

**V2Ray的优势**



> * **更完善的协议:** V2Ray 使用了新的自行研发的 VMess 协议，改正了 Shadowsocks 一些已有的缺点，更难被墙检测到（不保证可靠性）
> * **更强大的性能:** 网络性能更好，具体数据可以看 V2Ray 官方博客
> * **更丰富的功能:**  
> 1.mKCP: KCP 协议在 V2Ray 上的实现，不必另行安装 kcptun  
> 2.动态端口：动态改变通信的端口，对抗对长时间大流量端口的限速封锁  
> 3.路由功能：可以随意设定指定数据包的流向，去广告、反跟踪都可以  
> 4.传出代理：看名字可能不太好理解，其实差不多可以称之为多重代理。类似于 Tor 的代理  
> 5.数据包伪装：类似于 Shadowsocks-rss 的混淆，另外对于 mKCP 的数据包也可伪装，伪装常见流量，令识别更困难  
> 6.WebSocket 协议：可以 PaaS 平台搭建V2Ray，通过 WebSocket 代理。也可以通过它使用 CDN 中转，抗封锁效果更好  
> 7.Mux:多路复用，进一步提高科学上网的并发性能  


<br>
<br>

**搭建步骤**  

[1.购买一台境外服务器](#%E7%BE%8E%E5%9B%BDvps-hostwinds-%E8%B4%AD%E4%B9%B0)   
[2.使用 ssh 软件链接远程服务器](#%E8%BF%9C%E7%A8%8B%E8%BF%9E%E6%8E%A5hostwinds-vps)  
[3.执行V2ray 安装命令](#VPS一键脚本搭建V2Ray)  
[4.配置客户端实现连接外网](#V2Ray客户端配置)  


<br>

# 美国VPS Hostwinds 购买

[Hostwinds](https://www.hostwinds.com/11003.html) 是一家美国主机商，成立于 2010 年，国内站长使用较多的是 Hostwinds 美国 VPS 主机产品。由于 Hostwinds 美国 VPS 主机采用的是 SSD 硬盘，而且所有方案都有全球 CDN 加速功能，因而也备受用户青睐。 如今 Hostwinds 主机商提供的产品方案也非常丰富，包括虚拟主机、云主机、VPS主机以及独立主机等。目前 Hostwinds 主要有达拉斯、西雅图 2 个数据中心，其中西雅图数据中心在国内访问速度最快。现在 Hostwinds 提供免费更换IP了，没错，就是免费，免费，随意更换，可以一键解决 IP 被墙的问题了。通过 Hostwinds 搭建 V2ray 是不错的选择，今天就讲解下 Hostwinds 搭建 V2ray 教程。

首先确认不要使用任何代理，网络是什么 IP 就是什么 IP ，不然可能需要人工审核，导致 Hostwinds VPS 购买显示 "Pending" 状态， 不能即时创建服务激活。

**1.通过 [Hostwinds 优惠链接进入](https://www.hostwinds.com/11003.html)Hostwinds 首页**  
**2.在注册Hostwinds的时候，我们为了注册的方便，这里的语言就调整为中文，以前的语言只有英文，注册的过程中都是依靠谷歌浏览器中的翻译功能。**
   我们在右上角找到“English”，然后在出来的语言菜单中找到中文。
[![1.jpg](https://i.postimg.cc/CMk0ypbp/1.jpg)](https://postimg.cc/G9LVYNMX)  


**3.购买的产品有很多，你可以在Hostwinds上面购买虚拟主机、VPS和域名，这里我只推荐去购买VPS，其他的产品性价比并不是很高，我们在左边的菜单栏中找到“VPS”。**

然后选择“Linux unmanaged”，这个是非托管的Linux VPS主机，也是价钱最便宜的VPS。

上面要根据你的实际情况进行购买，如果你想购买Windows VPS，那么就选择 Windows VPS，购买 Linux VPS，就购买 Linux VPS，不要买错了。

Managed 跟 Unmanned 是托管与非托管的关系，托管的作用就是官方会帮助你进行管理，有问题可以请求官方进行处理，你无需操心别的事情。

[![2.jpg](https://i.postimg.cc/kGQcmPrf/2.jpg)](https://postimg.cc/jnjNH9S7)  



**4.进入 VPS 选择页面后，根据自己的需要的配置选择套餐，一般我们选择最低配置就够用了，然后点击 “Order” 按钮进入信息填写页面，如下所示：**  

[![3.jpg](https://i.postimg.cc/7ZD2w7Gf/3.jpg)](https://postimg.cc/PP3xMCMj)  
  

**5.进入信息填写页面后首先填写账号信息，一般是新用户我们填写左边的姓、名、邮箱、密码，然后点击 “Submit” 进入下一步，如下图所示：**  

 [![4.jpg](https://i.postimg.cc/cJx5zXQh/4.jpg)](https://postimg.cc/k2Hc6NYR)  


**6. 在“Client Information”中填入你的详细信息，跟上面的账号注册一样，也是使用拼音进行填写。**

而且填写的内容最好是真实的，因为在国外VPS购买的过程中都会有“欺诈”检测，如果你购买VPS的信息不是真实的话，VPS主机商会对你的账号进行核查，这样你购买就会很麻烦的。
填写的内容这里也有一个样例，根据上面的信息进行填写就行了，我这里只是做一个示范，你要根据你自身的信息进行填写。
如果实在是看不懂下面的图片，可以借助百度翻译。**  
 [![5.jpg](https://i.postimg.cc/0Qb6V1rs/5.jpg)](https://postimg.cc/BLfQvyxY)   


**7. 在“Package Information”中可以选择你购买VPS的详细配置。**  

“Billing Cycle”是账单周期，也就是付费周期，我们这里推荐购买的时间长一点，在你首次购买的时候会有一个百分之十的折扣，你购买的时间越长，得到的优惠也就越多，如果你只是想使用一下，尝尝鲜，那么就先去购买一个月的使用期限。

“Location”是机房的位置，你可以购买阿姆斯特丹、西雅图和达拉斯，我这里推荐购买的机房位置是西雅图(Seattle)，西雅图位于美国的西面，这个机房的位置离我们是最近的，所以在速度上面会有一定的保障。

“Operating System”就是操作系统的安装选择了，如果没有特殊的需求，那么就使用默认的Centos 8。

“IP Addresses”是IP地址，我们可以购买多个IP地址，这样可以随时更换。

其他的默认就行了。  

[![6.jpg](https://i.postimg.cc/vmyL2Tdd/6.jpg)](https://postimg.cc/rDhrK8mh)   


**8. “Optional Add Ons”中是额外的付费功能，分别是云备份和监控，我觉得这两个功能都没有多大的用，因为我可以自己设置这两个功能，无需花费多余的钱。**

这里要注意的是云备份这个选项还是默认的，如果你不需要一定要记得取消掉，不然每个月会多花费你1美元。  

[![7.jpg](https://i.postimg.cc/k4smR27M/7.jpg)](https://postimg.cc/628SD57F)   


**9. “Payment Information”中可以选择你的支付方式，我们这里就选择最后一个支付宝的支付方式，简单方便。**

核对好价格后，如果觉得满意我们就可以进行购买了，点击下方的“I have read and agree to theTerms of ServiceandPrivacy Policy”，同意Hostwinds的服务条款和隐私条款，点击下方的“Complete Order”就可以去支付了。 （只有国内 IP 访问的时候才有支付宝付款方式），如下图所示：  
[![8.jpg](https://i.postimg.cc/yYg8tTVF/8.jpg)](https://postimg.cc/LnSSZLgX)  



**10.下单完成后订单结果如下图所示：**     
[![Hostwinds7.png](https://i.postimg.cc/vmPG6Trv/Hostwinds7.png)](https://postimg.cc/PL8ggtFL)  


**11.完成购买后，你会收到一封邮件，里面包含 VPS 的IP地址、用户名、密码、ssh连接端口。**

<br>
<br>

# 远程连接Hostwinds VPS  

首先你需要通过 SSH 连接 [Hostwinds](https://www.hostwinds.com/11003.html) 的 Linux VPS，连接 Linux VPS 需要使用 SSH 工具，这里推荐使用 Xshell 可以复制粘贴命令，Xshell 本身是需要付款的，作为中国人当然是使用 XX 版了，这里提供下载包如下所示：  

Xshell 下载地址：[Xshell](https://github.com/githubvpn007/v2rayNvpn/releases/tag/Xshell-7.0)  

下载后解压安装即可。  

**使用 Xshell 连接 Linux VPS**  


**1、打开 Xshell，点击左上角“文件”-“新建”，打开连接弹出库。**    

[![9.png](https://i.postimg.cc/QCxDNWnD/9.png)](https://postimg.cc/N9VSpLcC)  


**2、在 Xshell 弹出框中输入 IP 和端口，端口一般是 22 默认，然后点击确认按钮，如下图所示：**    
[![10.jpg](https://i.postimg.cc/kXtkWmRK/10.jpg)](https://postimg.cc/9rC8jspQ)  


**3、然后输入用户名 root 和 密码，勾选记住用户名。  最后点击链接**  

[![11.jpg](https://i.postimg.cc/gkjLcg5x/11.jpg)](https://postimg.cc/gn92NHGp)  



**完成以上步骤后就可以看到连接成功的界面，如下图所示：**   
[![12.jpg](https://i.postimg.cc/LhFHRq7P/12.jpg)](https://postimg.cc/7JXvnLqP)

<br>
<br>

# VPS一键脚本搭建V2Ray  

### 1.在上图的待输入内容处，粘贴下面的命令（复制下面的命令，然后在 Xshell 待输入内容处“鼠标右键”/“粘贴”即可）：

` bash <(curl -s -L https://git.io/v2ray.sh) 
或者  
bash <(wget -qO- -o- https://git.io/v2ray.sh) `

```
如果提示 curl: command not found ，那是因为你的 VPS 没装 Curl  
解决办法：
ubuntu/debian 系统安装 Curl 执行命令:  
apt-get update -y && apt-get install curl -y

centos 系统安装 Curl 执行命令:   
yum update -y && yum install curl -y

安装好 curl  之后就能安装脚本了
```


[![15.png](https://i.postimg.cc/sgqGQkmm/15.png)](https://postimg.cc/RW1Z2bjJ)

OK，出现这个界面就表示 V2Ray 已经安装完成了。  

<br>

### 2.如上图所示，V2Ray 配置信息，有连接的详细信息，也有v2ray的vmess协议的快速导入连接

如果你使用过 V2Ray 某些客户端，那么现在也可以测试一下配置了。  

(备注，可能某些 V2Ray 客户端的选项或描述略有不同，但事实上，上面的 V2Ray 配置信息已经足够详细，由于客户端的不同，请对号入座。)  

##### 导入到V2ray 软件如下：  
[![16.png](https://i.postimg.cc/sfWzstGy/16.png)](https://postimg.cc/xq9hPpPF)   

##### 表示导入成功：  
[![17.jpg](https://i.postimg.cc/CxhHFp1Q/17.jpg)](https://postimg.cc/tYcV5fF3)   

#### 客户端的具体配置请看：[V2Ray客户端配置](#V2Ray客户端配置)

<br>  

### 3.测试连接是否通畅
[![18.png](https://i.postimg.cc/D0KFGg48/18.png)](https://postimg.cc/dknzPGBY)   

出现如上无法使用一般都是两种情况，一是无法连接上端口，二是客户端内核支持有问题。

(1)如果你的 VPS 有外部防火墙，请确保你已经开放了端口
测试端口是否能连接上：
打开：[https://ping.sx/check-port](https://ping.sx/check-port)  
**Target** 写你的 **VPS IP**，**Port** 写 V2Ray 的端口，然后点击 Check，如果 REACHABILITY 显示 Timeout，那是无法连接上端口
-提醒，你可以使用命令 v2ray ip 查看 VPS IP。

#### 这时候有两种办法：
1.关闭防火墙，执行如下命令：
` systemctl stop firewalld; systemctl disable firewalld; ufw disable `
关闭防火墙之后再测试一下端口是否通，如果不通，你可能还有外部防火墙没关，必须要能连接上端口才能正常使用。
如果 REACHABILITY 显示 Reachable 那就是能连接上端口，那就继续！ 如果 REACHABILITY 显示 Timeout那就是端口还是不能访问，这个时候您需要到服务器商后台手动操作关闭防火墙！


2.打开v2ray端口  
执行命令：  
` iptables -I INPUT -p tcp --dport 你的端口 -j ACCEPT `    
![端口](https://i.postimg.cc/L5J9NZJd/v2ray.png)

如果 REACHABILITY 显示 Reachable 那就是能连接上端口，那就继续！ 如果 REACHABILITY 显示 Timeout那就是端口还是不能访问，这个时候您需要到服务器商后台手动操作关闭防火墙！
<br>

(2)提醒，默认安装的 V2Ray 内核为最新版本
如果无法使用，可能是你客户端的内核太旧

请尝试使用不同的客户端进行测试；比如 v2rayN；v2rayNG 等

请尝试设置 VMessAEAD，某些客户端会有相关选项

某些客户端得把 额外id(alterid) 填写为 0；比如垃圾苹果那边的东西

###### 解决方案一，请尝试将服务器端的内核版本降级

使用命令： v2ray update core 4.45.2 降级即可

##### 解决方案二，升级客户端内核

备注，请尽量将客户端内核和服务器端内核保持一致！内核版本低于 5 可能会出现莫名其妙的问题  

<br>  

### 3.V2Ray 更多设置和信息查看  
输入命令：`v2ray`  即可根据选项查看你想要的信息  
![信息查看](https://i.postimg.cc/Y2bZ7fRk/v2ray.png)

<br>
<br>

# 优化 V2Ray  

由于本人的脚本在 Debian9 系统会自动开启 BBR 优化加速了，所以不需要再安装 BBR 优化了，如果你是使用其他商家的 VPS 并且是按照此教程流程来安装 V2Ray 的话，那么你可以输入  

> v2ray bbr  

回车，然后选择安装 BBR 或者 锐速 来优化 V2Ray  

只是还想再啰嗦一下，如果你是使用国际大厂的 VPS，并且是按照此教程流程来安装 V2Ray 的话，请自行在安全组 (防火墙) 开放端口和 UDP 协议 (如果你要使用含有 mKCP 的传输协议)  

<br>
<br>

# V2Ray客户端配置  

**1.下载地址**：[下载zz_v2rayN-With-Core-SelfContained.z7](https://github.com/2dust/v2rayN/releases)**   

**2.对zz_v2rayN-With-Core-SelfContained.z7进行解压,双击 v2rayN.exe 打开软件**  
[![13.jpg](https://i.postimg.cc/G2rp1q7b/13.jpg)](https://postimg.cc/NyJt67vn)  

**3.点击电脑右下角v2rayN 图标打开面板**  
[![14.jpg](https://i.postimg.cc/ZKKzz1Jf/14.jpg)](https://postimg.cc/yW5bFpg9)  

**4.将上面安装V2Ray服务器生成的VMess 协议的那串东西复制到电脑 **  
[![15.png](https://i.postimg.cc/sgqGQkmm/15.png)](https://postimg.cc/RW1Z2bjJ)  

然后点击“从剪切板批量导入URL”  
[![16.png](https://i.postimg.cc/sfWzstGy/16.png)](https://postimg.cc/xq9hPpPF)  

然后就能看到导入的v2ray 服务器信息  
[![17.jpg](https://i.postimg.cc/CxhHFp1Q/17.jpg)](https://postimg.cc/tYcV5fF3)  

**5.测试链接是否正常**  
[![18.png](https://i.postimg.cc/D0KFGg48/18.png)](https://postimg.cc/dknzPGBY)

**出现延迟信息就表明链接正常**  
[![19.jpg](https://i.postimg.cc/X7J185KJ/19.jpg)](https://postimg.cc/G9Wj3H5w)  

**6.打开代理服务**  
[![20.jpg](https://i.postimg.cc/tCFdFt7W/20.jpg)](https://postimg.cc/nCcQJm4h)  

**7.首次打开软件 需要下载一些配置，等它下载完成就可以上网了**  
[![21.jpg](https://i.postimg.cc/43yBv7DG/21.jpg)](https://postimg.cc/gw9VmJct)  
<br>
<br>


### [更多配置教程请参考这里](https://xiaoheicn.top/v2ray%e5%ae%a2%e6%88%b7%e7%ab%af%e5%85%a8%e9%9b%86/)  



<br>  
<br>  
<br>  
<br>  

**更多搭建教程:**  

[SSR一键安装脚本使用教程](https://xiaoheicn.top/ssr%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85%E8%84%9A%E6%9C%AC-shadowsocksr%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85%E6%95%99%E7%A8%8B/)  
[V2ray一键安装脚本使用教程](https://xiaoheicn.top/v2ray%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85%E8%84%9A%E6%9C%AC-233boy%E5%A4%A7%E7%A5%9E%E7%89%88%E6%9C%AC-%E5%8D%95%E7%94%A8%E6%88%B7/)  
[Trojan一键安装脚本使用教程](https://xiaoheicn.top/trojan%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85%E8%84%9A%E6%9C%AC-%E6%90%AD%E5%BB%BA%E4%BC%AA%E8%A3%85%E7%BD%91%E7%AB%99%E7%BB%AD%E7%AD%BE%E8%AF%81%E4%B9%A6%E5%AE%A2%E6%88%B7%E7%AB%AF%E5%8F%82%E6%95%B0%E9%85%8D%E7%BD%AE/)  
[WireGuard一键安装脚本使用教程](https://xiaoheicn.top/wireguard%E4%B8%80%E9%94%AE%E5%AE%89%E8%A3%85%E8%84%9A%E6%9C%AC%E7%A7%8B%E6%B0%B4%E7%89%88/)

<br>

