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
