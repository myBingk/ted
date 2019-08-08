# ted
分享和记录自己花费一小时以上解决的难题，欢迎大家加入，传递更多心得和经验。

---

## github加速:

选择码云导入已有项目，复制github链接，相当于码云做了加速。

---

## 解决springboot编译时找不到parent pom.xml的问题，添加maven仓库地址：
``` xml
<mirror>
  <id>mirrorId</id>
  <mirrorOf>central</mirrorOf>
  <name>Human Readable Name for this Mirror.</name>
  <url>http://central.maven.org/maven2/</url>
</mirror>
```

---

### 国内阿里云镜像地址
```xml
<mirror>
  <id>publicRepository</id>
  <mirrorOf>central</mirrorOf>
  <name>publicRepository</name>
  <url>http://maven.aliyun.com/nexus/content/groups/public</url>
</mirror>
```

---

### jenkins查杀服务并重启
```shell
BUILD_ID=DONTKILLME
mv ${WORKSPACE}/target/${JOB_NAME}-1.0.0.RELEASE-exec.jar /usr/local/service/
serviceId=`ps -ef|grep ${JOB_NAME}-1.0.0.RELEASE-exec.jar|grep -v 'grep'|awk '{print $2}'`
if [ -z "$serviceId"];then
	echo 'no such process.'
else
    echo $serviceId
    kill -9 $serviceId
fi
sh /usr/local/service/${JOB_NAME}-starter.sh
```

---

### 因为jenkins远程部署使用的是本地环境变量，如果发生commond not found,请重新导入一遍系统变量。或者直接输入执行文件前缀，如java：/usr/local/service/java-se-8u40-ri/bin/java -jar

---

### 安装DockerCE(免费版)


```shell
yum install -y yum-utils \
  device-mapper-persistent-data \
  lvm2
  
yum-config-manager \
    --add-repo \
    https://download.docker.com/linux/centos/docker-ce.repo  
```

---

### openjdk 下载地址：https://download.java.net/openjdk/jdk8u40/ri/openjdk-8u40-b25-linux-x64-10_feb_2015.tar.gz

---

### 查看内存占用最高的十个进程
ps -aux | sort -k4nr | head -n 10

---

### RocketMQ一直报No route info of this topic，因为把服务提前关闭了。

---

### nginx基本配置（含websocket wss连接配置）

```xml
server {
		listen 443;
		server_name // TODO 监听的域名;
		ssl on;
		root html;
		index index.html index.htm;
		ssl_certificate   ../cert/ // TODO 证书地址;
		ssl_certificate_key  ../cert/ // TODO 证书地址;
		ssl_session_timeout 5m;
		// 加密套件
		ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
		ssl_protocols TLSv1 TLSv1.1 TLSv1.2;
		ssl_prefer_server_ciphers on;
		location / {
          	proxy_pass // TODO 转发地址;
          	proxy_set_header  Host $host;
          	proxy_set_header  X-Real-IP  $remote_addr;
          	proxy_set_header  X-Forwarded-For  $proxy_add_x_forwarded_for;
        }
		
		location /websocket/ {
			proxy_pass // TODO websocket协议地址;
			proxy_http_version 1.1;
			proxy_set_header Upgrade $http_upgrade;
			proxy_set_header Connection "upgrade";
			proxy_set_header X-real-ip $remote_addr;
			proxy_set_header X-Forwarded-For $remote_addr;
		}
	}
	
	server {
        listen       80;
        server_name  // TODO 监听的域名; 
		rewrite ^(.*) https://$host$1 permanent;
		if ( $http_user_agent ~* ^$){
			return 444;
		}

        location / {
          	proxy_pass // TODO 监听的域名;
          	proxy_set_header  Host $host;
          	proxy_set_header  X-Real-IP  $remote_addr;
          	proxy_set_header  X-Forwarded-For  $proxy_add_x_forwarded_for;
        }
    }
```

---

### 安装nodejs v10
wget  https://nodejs.org/dist/v10.16.0/node-v10.16.0-linux-x64.tar.xz
tar -xvf 
ln ../node /usr/local/bin
ln ../npm /usr/local/bin

---

### 如果java程序访问https接口包必要参数为空即ssl异常时，可能是允许的java环境没有ssl支持相关包，请下载其它版本。

---

### 微信支付需注意：

如果签名失败，请仔细检查参数，与官方给出给出的配置是否一致，如果注册地址尚未存在，则需要在商户平台添加授权目录，JSAPI支付为微信浏览器内支付，需要openId，详情请看微信JSSDK：https://mp.weixin.qq.com/wiki?t=resource/res_main&id=mp1421141115，
https://pay.weixin.qq.com/wiki/doc/api/jsapi.php?chapter=7_7&index=6
```yml
appId: 
mchId: 
mchKey: 
deviceInfo: WEB
signType: MD5
notifyUrl: 
tradeType: MWEB
useSandboxEnv: false
```

---

### 安装java调试利器Btrace

```shell
https://github.com/btraceio/btrace/releases/download/v1.3.11.3/btrace-bin-1.3.11.3.zip
export BTRACE_HOME 
export $PATH:$BTRACE_HOME/bin
预编译 btracec java文件
jsp列出java运行进程
btrace -v -o 输出日志 $PID java文件
```

---

### git用ssh拉取代码

```shell
ssh-keygen -t rsa -C "邮箱地址"
把~/.ssh下的.pub内容复制到网站公钥里面
```

---

### jenkins报mvn目录不存在，则是未定义maven环境，加上环境变量即可

---
