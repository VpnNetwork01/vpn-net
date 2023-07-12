# V2Ray搭建介绍
**什么是V2Ray(2023/7/12更)**  

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

1、通过 [Hostwinds 优惠链接进入](https://www.hostwinds.com/11003.html)Hostwinds 首页，选择 “VPS” 下的 "Unmanaged VPS" ，这里是最便宜的**(注意千万不要选择页面上 3.29 美元那个，那个是虚拟空间，不是 VPS !!!)**。  

![Hostwinds注册首页](https://camo.githubusercontent.com/2ffca6ecd2813e411d14aa1ae03a9b3c436711c1d62a81cf3c7f9be5f545b7d4/68747470733a2f2f696d67323031382e636e626c6f67732e636f6d2f626c6f672f313736353439362f3230323030322f313736353439362d32303230303231383132333833393833322d3737333134313437342e6a7067)  

2、进入 VPS 选择页面后，根据自己的需要的配置选择套餐，一般我们选择最低配置就够用了，然后点击 “Order” 按钮进入信息填写页面，如下所示：  

![首页](https://camo.githubusercontent.com/1816508c514979b7deae5be1892596bbe84cba2b45e62b13c57968b92f72ca16/68747470733a2f2f696d67323031382e636e626c6f67732e636f6d2f626c6f672f313736353439362f3230323030322f313736353439362d32303230303231383132333930303836372d313330313531303133352e6a7067)  

3、进入信息填写页面后首先填写账号信息，一般是新用户我们填写左边的姓、名、邮箱、密码，然后点击 “Submit” 进入下一步，如下图所示：  

![Hostwinds注册信息填写](https://camo.githubusercontent.com/1cfe1a4ef668b9fa85538714fde446f635a68a2f91d8c2be9d2015580ba11f90/68747470733a2f2f696d67323031382e636e626c6f67732e636f6d2f626c6f672f313736353439362f3230323030322f313736353439362d32303230303231383132333932343836342d313734383233363631302e6a7067)  

4、页面跳转后填写用户信息，如下图所示：  

![Hostwinds用户信息](https://camo.githubusercontent.com/d3fc65e6278512176176ce3afb69b09de4b0c3af48929095e007f4695c60d807/68747470733a2f2f696d67323031382e636e626c6f67732e636f6d2f626c6f672f313736353439362f3230323030322f313736353439362d32303230303231383132333932363134352d313037383137393437352e6a7067)  

5、然后选择购买时间、数据中心 、操作系统，红色部分需要自己选择，绿色一般我们默认，可以按月购买，但是建议第一次购买时间选择长一点，这样优惠要大很多，不然后面续费优惠力度就没有这么大了。 如下图所示：  

![Hostwinds套餐选择2](https://i.postimg.cc/cJVh0hk9/Hostwind.png)    

6、默认是自动云备份的，如果不需要去掉勾选， 如下图所示：  

![Hostwinds套餐选择3](https://camo.githubusercontent.com/35427730d7aa60a246cc61e7040a1d7ab3b51f080d5c1cf7b2647f2ef73609c4/68747470733a2f2f696d67323031382e636e626c6f67732e636f6d2f626c6f672f313736353439362f3230323030322f313736353439362d32303230303231383132333933303731312d3130303433373531342e6a7067)  

7、然后选择付款方式，一般我们选择支付宝进行付款 （只有国内 IP 访问的时候才有支付宝付款方式），如下图所示：  

![Hostwinds支付宝支付](https://camo.githubusercontent.com/b7630fb0a253b005663d79985bbb6be73c9cbabfdc92bca3271945c92018595f/68747470733a2f2f696d67323031382e636e626c6f67732e636f6d2f626c6f672f313736353439362f3230323030322f313736353439362d32303230303231383132333933323431382d3436353335333130322e6a7067)  

8、最后确认价格（不同时期可能价格有些许不同，如果通过前面优惠链接点击购买会有优惠），勾选同意协议，然后点击“Complete Order”按钮进行下单， 如下图所示：  

![Hostwinds](https://camo.githubusercontent.com/05c771691a2eae2ff0e42bd172a08d15bb7c9bf598fddc85a07a632b533c3187/68747470733a2f2f696d67323031382e636e626c6f67732e636f6d2f626c6f672f313736353439362f3230323030322f313736353439362d32303230303231383132333933343332362d35353237393638302e6a7067)  

9、下单完成后订单结果如下图所示：     
![1](https://camo.githubusercontent.com/9904fcb6dcf58d1110983b3f28377afde064be5b13ecbcfb7c9aee7586a18fae/68747470733a2f2f696d67323031382e636e626c6f67732e636f6d2f626c6f672f313736353439362f3230323030322f313736353439362d32303230303231383132333933363430372d3832393139363839312e6a7067)  


10、完成购买后，你会收到一封邮件，里面包含 VPS 的IP地址、用户名、密码、ssh连接端口。如下图所示：  
![2](https://camo.githubusercontent.com/a9e4edb2e1ace6addb864a02195ba2d30339a857a7362ceab1756c9a9a9ecc9c/68747470733a2f2f696d67323031382e636e626c6f67732e636f6d2f626c6f672f313736353439362f3230323030322f313736353439362d32303230303231383132333933373739362d3235303034353238332e6a7067)  

<br>
<br>

# 远程连接Hostwinds VPS  

首先你需要通过 SSH 连接 [Hostwinds](https://www.hostwinds.com/11003.html) 的 Linux VPS，连接 Linux VPS 需要使用 SSH 工具，这里推荐使用 Xshell 可以复制粘贴命令，Xshell 本身是需要付款的，作为中国人当然是使用 XX 版了，这里提供下载包如下所示：  

Xshell 下载地址：[Xshell](https://github.com/githubvpn007/v2rayNvpn/releases/tag/Xshell-7.0)  

下载后解压安装即可。  

**使用 Xshell 连接 Linux VPS**  


1、打开 Xshell，点击左上角“文件”-“新建”，打开连接弹出库。  

![Xshell连接VPS](https://camo.githubusercontent.com/eee8534da0fa174252bf0fc0f4dab899a4a48a3b6044e1c1462407051fc92554/68747470733a2f2f696d67323031382e636e626c6f67732e636f6d2f626c6f672f313736353439362f3230323030322f313736353439362d32303230303231383132343234343731332d313634373030333539312e6a7067)  


2、在 Xshell 弹出框中输入 IP 和端口，端口一般是 22 默认，然后点击确认按钮，如下图所示：  
![Xshell连接VPS](https://camo.githubusercontent.com/e5e0c22f6532d962663bb4043e027c8135ee4dd261b2240dfe1b10818b0b1217/68747470733a2f2f696d67323031382e636e626c6f67732e636f6d2f626c6f672f313736353439362f3230323030322f313736353439362d32303230303231383132343235303732302d3330303333303039322e6a7067)  


3、然后输入用户名 root，勾选记住用户名。  

![Xshell连接VPS](https://camo.githubusercontent.com/8100e26da58653b726d97c4d6764d4f95d41f6bf2789128037f6992ac2c7bef0/68747470733a2f2f696d67323031382e636e626c6f67732e636f6d2f626c6f672f313736353439362f3230323030322f313736353439362d32303230303231383132343235333136312d313234303735393431302e6a7067)  


4、然后输入密码，勾选记住密码，点击确定。  
![1](https://camo.githubusercontent.com/d8c7466c0fc3e8df5b90b388b0e4e3b58969b3d61d00acbc47819e7454eab32f/68747470733a2f2f696d67323031382e636e626c6f67732e636f6d2f626c6f672f313736353439362f3230323030322f313736353439362d32303230303231383132343235373234332d3339323939393332312e6a7067)  

完成以上步骤后就可以看到连接成功的界面，如下图所示：  
![2](https://camo.githubusercontent.com/bd67dd257edcfbb88e3798c3c8e261ca50f9bc6455bfaf37d6680e7bf3a978a8/68747470733a2f2f696d67323031382e636e626c6f67732e636f6d2f626c6f672f313736353439362f3230323030322f313736353439362d32303230303231383132343331343736362d313634393931393432382e6a7067)  

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


![Hostwinds搭建V2Ray](https://i.postimg.cc/NGTCDdgf/v2ray.png)  

OK，出现这个界面就表示 V2Ray 已经安装完成了。  

<br>

### 2.如上图所示，V2Ray 配置信息，有连接的详细信息，也有v2ray的vmess协议的快速导入连接

如果你使用过 V2Ray 某些客户端，那么现在也可以测试一下配置了。  

(备注，可能某些 V2Ray 客户端的选项或描述略有不同，但事实上，上面的 V2Ray 配置信息已经足够详细，由于客户端的不同，请对号入座。)  

##### 导入到V2ray 软件如下：  
![Hostwinds搭建V2Ray](https://i.postimg.cc/zXZF0h7C/v2ray.png)   

##### 表示导入成功：  
![Hostwinds搭建V2Ray](https://i.postimg.cc/LXmnJH3Z/v2ray.png)   

#### 客户端的具体配置请看：[V2Ray客户端配置](#V2Ray客户端配置)

<br>  

### 3.测试连接是否通畅
[![v2ray.png](https://i.postimg.cc/T2zBgQsf/v2ray.png)](https://postimg.cc/K459y7FH)  

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

**下载地址**：[下载v2rayN-Core.zip](https://github.com/2dust/v2rayN/releases)  

对v2rayN-Core.zip进行解压,双击 v2rayN.exe 打开软件  
![1](https://camo.githubusercontent.com/7e6ad47fef214162ebd1015de49c911b500d7f18b387735f31c2ab5b8cb56420/68747470733a2f2f696d67323031382e636e626c6f67732e636f6d2f626c6f672f313736353439362f3230323030322f313736353439362d32303230303231383132343435353838312d3739393439363530332e6a7067)  

**配置**

运行 V2RayN.exe，然后进行配置。

客户端的配置需要根据你的服务端进行相应的配置，因为你的服务端协议可能是 vmess,shadowsocks 等。

如果你的服务端配置是协议 vmess，则配置如下：  

![V2ray客户端配置](https://camo.githubusercontent.com/8911904bf0fec43b19ecb31b1ffbc3a90441095c9cffe10f3341b6cc4cbe83e6/68747470733a2f2f696d67323031382e636e626c6f67732e636f6d2f626c6f672f313736353439362f3230323030322f313736353439362d32303230303231383132343530343330382d3230323537373735322e6a7067)  
![4](https://camo.githubusercontent.com/fe600370692df8ad1acadaaa4745f356e60c77f11a022a54aa93bba4ec936169/68747470733a2f2f696d67323031382e636e626c6f67732e636f6d2f626c6f672f313736353439362f3230323030322f313736353439362d32303230303231383132343530373033382d313630393738313530382e6a7067)  


**打开PAC模式(意思是自动选择是否走vpn代理，一般国内域名则不走代理)**   

![6](https://camo.githubusercontent.com/e289e32bac11706a60cbe88a0713d8d758587b0909689d7ba876ca34192c9f54/68747470733a2f2f696d67323031382e636e626c6f67732e636f6d2f626c6f672f313736353439362f3230323030322f313736353439362d32303230303231383132343531303037372d313433323736313135352e6a7067)

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

