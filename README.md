## HTTPS 协议申请和安装篇
如果不用 **HTTPS** 协议的话不仅不安全且在大陆很容易被联通电信这样运营商注入恶意广告之类的。    
避免 ISP 劫持    
申请 ssh 证书，从选择域名的角度出发。   
1. 支持的范围，要求覆盖所有的浏览器   
2. 证书的类型，通常证书是国际认证的，也就是 CA 能够申请得到的证书有三种，一是域名级的证书，简称 DV 安全等级要求不够高的可以使用，一般的个人网站小公司的网站之类的。二是企业级别的证书，简称 OV 这类的证书使用的对象是一些对客户的资料比较敏感的，比如社交网站，电商之类的，收费比较贵。三是 EV 超安=EV=最安全、最严格 超安EV SSL证书遵循全球统一的严格身份验证标准       
          
-  DV SSL   
DV SSL证书是只验证网站域名所有权的简易型（Class 1级）SSL证书，可10分钟快速颁发，能起到加密传输的作用，但无法向用户证明网站的真实身份。目前市面上的免费证书都是这个类型的，只是提供了对数据的加密，但是对提供证书的个人和机构的身份不做验证。    
- OV SSL   
OV SSL,提供加密功能,对申请者做严格的身份审核验证,提供可信身份证明。和DV SSL的区别在于，OV SSL 提供了对个人或者机构的审核，能确认对方的身份，安全性更高。所以这部分的证书申请是收费的~     
- EV SSL   
超安=EV=最安全、最严格 超安EV SSL证书遵循全球统一的严格身份验证标准，是目前业界安全级别最高的顶级 (Class 4级)SSL证书。金融证券、银行、第三方支付、网上商城等，重点强调网站安全、企业可信形象的网站，涉及交易支付、客户隐私信息和账号密码的传输。这部分的验证要求最高，申请费用也是最贵的。  

第三点需要考虑的就是证书的维护成本   
3. 证书的维护成本    
一般证书过期时间是三个月，我们可以通过一些脚本来完成证书的过期维护，自动更新证书    


## 证书的申请平台   
**[又拍云](https://www.upyun.com/)**
有免费的 ssl 证书的申请    
**[七牛云](https://portal.qiniu.com/certificate/apply)** 免费的 DV ssl证书     
**[腾讯云](https://console.cloud.tencent.com/ssl?apply=1)**  也是免费的 ssl 证书    
阿里云由于免费的是赛门铁克的，现在 chrome 对赛门铁克的 ssl 证书的态度是不信任，所以就不在赘述阿里云，下面用 **腾讯云** 做示范
## 在腾讯云上申请证书    
按照腾讯的官方要求进行注册，实名认证，然后申请免费的 DV ssl 证书    
### 申请证书   
在注册完，并且实名认证以后，在申请证书页面 [申请证书](https://console.cloud.tencent.com/ssl) 点击红框选择申请证书     
如下图    
![1](http://i.imgur.com/RYfaMkw.png)   
点击红框选择申请证书。       
通用名称选择你想要申请证书的域名，接着填邮箱。后面的备注和私钥密码可以不管。然后点击下一步
![2](http://i.imgur.com/LTuEGsH.png)      
这时候会出现如下的页面，我们选择手动验证 DNS        
![3](http://i.imgur.com/5D8NeCx.png)     
点击查看证书详情    
![4](http://i.imgur.com/oTe4uzf.png)        
把记录值，还有主机记录复制粘贴到  [DNSPOD](https://www.dnspod.cn/console/dns) 中
![5](http://i.imgur.com/LDwD2d8.png)     
打开 [DNSPOD](https://www.dnspod.cn/console/dns) 添加新记录，然后主机记录，记录值填入上图中对应的值。DNSPOD 中的记录类型选择 TXT。然后保存，大概几分钟之后你就会接到腾讯的短信通知。说你的证书以及申请好了。   
![6](http://i.imgur.com/CxGUANV.png)        
成功以后如下图，点击红框进行下载。   
![](http://i.imgur.com/6djEBFb.png)     
解压后有四个文件夹，选择对应你服务器上安装的，我这里是 Nginx   
![](http://i.imgur.com/NSeMtXv.png)       

## 服务器上安装和部署    
这里我用的是阿里云 ECS，**Ubuntu 14.04** ，**node.js  v6.9.5** 。     
### 连接服务器   
我这里用的 git 连的服务器，我个人觉得 git 比 putty 好使。友情安利一下。   
这里分三步走，第一步上传证书到服务器。第二步修改服务器上 Nginx 目录下的 conf.d 文件夹中的 H5-lipc-7000.conf 文件。第三步重启 Nginx 。    
1. 用 git 打开已经解压的 Nginx 文件夹，然后上传文件夹中的两个文件到服务器上传的命令是        

	scp -P 7000 ./2_H5.lipc.xin.key lipc@47.33.277.180:/home/lipc/    
	scp -P 7000 ./1_H5.lipc.xin_bundle.crt lipc@47.33.277.180:/home/lipc/        
    
 这里我解释一下 `-P` 后的 7000 是我设定的防火墙端口，如果没有修改端口的话默认是 22 。`lipc` 是我设定的 H5 这个项目所在的用户。   
上传完毕后是这样的,在服务器上会有这两个文件。     
 ![](http://i.imgur.com/eVAmvPn.png)    
然后把这两个文件转移到目录 /www/ssl 下。我的所有证书都在这个目录下   
![](http://i.imgur.com/qOBRsYi.png)    
2. 打开文件 H5-lipc-7000.conf 文件，修改配置文件参数    
我的 nginx 配置文件在 ` /etc/nginx/conf.d` 这个目录下，然后打开 **conf.d** 中的 H5-lipc-7000.conf 第一次修改的话要先创建一个。然后修改其内容。执行代码 `sudo vi H5-lipc-7000.conf`，打开后修改文件内容为    
    
	upstream H5 {
	   server 127.0.0.1:8084;
	 }
	server{
	  listen 80;
	  server_name h5.lipc.xin;
	  return 301 https://www.lipc.xin$request_uri;
	}
	server{
	  listen 443;
	  server_name www.lipc.xin; #填写绑定证书的域名
	  ssl on;
	  ssl_certificate /www/ssl/1_H5.lipc.xin_bundle.crt;
	  ssl_certificate_key /www/ssl/2_H5.lipc.xin.key;
	  ssl_session_timeout 5m;
	  ssl_protocols TLSv1 TLSv1.1 TLSv1.2; #按照这个协议配置
	  ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:HIGH:!aNULL:!MD5:!RC4:!DHE;
	  ssl_prefer_server_ciphers on;
	
	  if ($ssl_protocol = "") {# 加一层判断如果是空就跳转到主页，有没有这个判断无所谓
	    rewrite ^(.*) https://$host$1 permanent;
	  }
	    location / {
	    proxy_set_header X-Real-IP $remote_addr;
	        proxy_set_header X-Forward-For $proxy_add_x_forwarded_for;
	        proxy_set_header Host $http_host;
	        proxy_set_header X-Nginx-Proxy true;
	
	        proxy_pass http://H5;
	        proxy_redirect off;
	  }
	}     
    
然后 `sudo nginx -t`,检查有没有错误，没有错误了就 `sudo nginx -s reload`。    
如果你配置了防火墙最后还要记得修改防火墙配置参数，把相应的端口放开。
## 新鲜的 Linux 部署项目文档
### 查看服务器的版本号
由于项目在部署的时候用的是一个基本干净的 Linux 服务器,所以在这先查一下版本号 `cat /proc/version`.这里服务器的版本号是 **Linux version 3.10.0-693.el7.x86_64 (builder@kbuilder.dev.centos.org)**
### 安装 node.js
首先下载源码到本地：
wget -c https://nodejs.org/dist/v8.9.1/node-v8.9.1.tar.gz   
下载完毕，提取 tar 文件：    
tar -zxvf node-v8.9.1.tar.gz   
进入文件夹：   
cd node-v8.9.1   
在编译代码之前，还需要在机器上安装一些软件包，使得编译可以正常运行：   
sudo yum install gcc gcc-c++    
对源代码进行配置：    
./configure    
进行编译：    
make    
安装：    
sudo make install    
安装完成后，可以输入命令 node -v 来检查 Nodejs 是否安装成功：    
v8.9.1    
### 安装 Nginx
**安装所需要的环境:**     
一. gcc 安装
安装 nginx 需要先将官网下载的源码进行编译，编译依赖 gcc 环境，如果没有 gcc 环境，则需要安装：   
`yum install gcc-c++`   
二. PCRE pcre-devel 安装   
PCRE(Perl Compatible Regular Expressions) 是一个Perl库，包括 perl 兼容的正则表达式库。nginx 的 http 模块使用 pcre 来解析正则表达式，所以需要在 linux 上安装 pcre 库，pcre-devel 是使用 pcre 开发的一个二次开发库。nginx也需要此库。命令：   
`yum install -y pcre pcre-devel`
三. zlib 安装   
zlib 库提供了很多种压缩和解压缩的方式， nginx 使用 zlib 对 http 包的内容进行 gzip ，所以需要在 Centos 上安装 zlib 库。     
`yum install -y zlib zlib-devel`    
四. OpenSSL 安装     
OpenSSL 是一个强大的安全套接字层密码库，囊括主要的密码算法、常用的密钥和证书封装管理功能及 SSL 协议，并提供丰富的应用程序供测试或其它目的使用。      
nginx 不仅支持 http 协议，还支持 https（即在ssl协议上传输http），所以需要在 Centos 安装 OpenSSL 库。      
`yum install -y openssl openssl-devel`      
**使用wget命令下载 Nginx**    
`wget -c https://nginx.org/download/nginx-1.10.1.tar.gz`    
**解压**
依然是直接命令：     
 
	tar -zxvf nginx-1.10.1.tar.gz
	cd nginx-1.10.1
**配置**    
其实在 nginx-1.10.1 版本中你就不需要去配置相关东西，默认就可以了。当然，如果你要自己配置目录也是可以的,使用默认配置    
**`./configure`**     
**编译安装**

	make
	make install
查找安装路径：`whereis nginx`    
**启动、停止nginx**

	cd /usr/local/nginx/sbin/
	./nginx 
	./nginx -s stop
	./nginx -s quit
	./nginx -s reload
	./nginx -s quit:此方式停止步骤是待nginx进程处理任务完毕进行停止。
	./nginx -s stop:此方式相当于先查出nginx进程id再使用kill命令强制杀掉进程。

查询nginx进程：

	ps aux|grep nginx   
**重启 nginx**
1.先停止再启动（推荐）：   
对 nginx 进行重启相当于先停止再启动，即先执行停止命令再执行启动命令。如下：

	./nginx -s quit
	./nginx
2.重新加载配置文件：    
当 ngin x的配置文件 nginx.conf 修改后，要想让配置生效需要重启 nginx，使用-s reload不用先停止 ngin x再启动 nginx 即可将配置信息在 nginx 中生效，如下：

	./nginx -s reload
使用`./nginx -s reload`重新读取配置文件，发现报

	nginx: [error] open() /usr/local/nginx/logs/nginx.pid failed (2: No such file or directory)错误，进到logs文件发现没有nginx.pid文件     

执行这个命令解决上面的报错

	/usr/local/nginx/sbin/nginx -c /usr/local/nginx/conf/nginx.conf   
使用nginx -c的参数指定nginx.conf文件的位置    
提示：`nginx: [emerg] bind() to 0.0.0.0:80 failed (98: Address already in use)   
killall -9 nginx `杀掉nginx 进程 然后重启就行了。 `service nginx restart `      
另外 还有一个很重要的可能  `ps  -ef | grep nginx` 看下主目录 是哪里 是不是装了两个可恶的 Nginx    
### 配置 Nginx   
有时候我们的防火墙是打开的,当我们配置好了 Nginx ,在服务器上执行    

	curl http://127.0.0.1
这句命令的时候也是有如下的代码反馈,但是就是在浏览器中打不开,那么有可能就是防火墙的问题了  

	[root@Server-117 sbin]# curl http://127.0.0.1
	<!DOCTYPE html>
	<html>
	<head>
	<title>Welcome to nginx!</title>
	<style>
	    body {
	        width: 35em;
	        margin: 0 auto;
	        font-family: Tahoma, Verdana, Arial, sans-serif;
	    }
	</style>
	</head>
	<body>
	<h1>Welcome to nginx!</h1>
	<p>If you see this page, the nginx web server is successfully installed and
	working. Further configuration is required.</p>
	
	<p>For online documentation and support please refer to
	<a href="http://nginx.org/">nginx.org</a>.<br/>
	Commercial support is available at
	<a href="http://nginx.com/">nginx.com</a>.</p>
	
	<p><em>Thank you for using nginx.</em></p>
	</body>
	</html>
	
这时候执行    

 	sudo firewall-cmd --zone=public --add-port=80/tcp --permanent
打开防火墙.    
这条命令的几个含义：   

	–zone             # 作用域
	–add-port=80/tcp  # 添加端口，格式为：端口/通讯协议
	–permanent        # 永久生效，没有此参数重启后失效
设置完还需要重启一下防火墙：

	sudo firewall-cmd --reload

我们再用浏览器访问一下服务器，现在发现能成功访问了.
### 部署网站应用    
用 item 登录到服务器上,在服务器上的 var 文件夹中上传本地打好包的 www 文件夹,然后就是修改配置文件   
	sudo vi /usr/local/nginx/conf/nginx.conf   
然后修改如下的部分    

	server {
	    listen       80; # 端口
	    server_name  localhost;
	
	    #charset koi8-r;
	
	    #access_log  logs/host.access.log  main;
	
	    location / {
	        root   /var/www; # 文件夹的位置
	        index  index.html index.htm; # 入口
	    }
然后重启 `./nginx -s reload`    
这时候输入 ip 就可以查看到部署的项目文件了.   
## 部署 Node.js 项目
现在我们这的后台是由 node 做的,要部署到这个上面.具体的部署操作见下面的操作    
服务都是写在 server.js 中的,所以我们这里只需要用 pm2 起一下 server.js 就 OK 了,具体就是     

	pm2 start server
   








































