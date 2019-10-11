# Tomcat配置Https请求

## HTTPS和HTTP的区别<br>
* 一、HTTP 是超文本传输协议，信息是明文传输，HTTPS 则是具有安全性的 SSL 加密传输协议。
* 二、HTTPS 协议需要到 CA 申请证书，一般免费证书很少，需要交费。
* 三、HTTP 和 HTTPS 使用的是完全不同的连接方式，用的端口也不一样，前者是 80，后者是 443。
* 四、HTTP 的连接很简单，是无状态的；HTTPS 协议是由 SSL+HTTP 协议构建的可进行加密传输、身份认证的网络协议，比 HTTP 协议安全。

## 本地配置
* 1、Ctrl + R 打开命令 , 进入 cmd 控制台 <br>

![Image text](https://github.com/Vico-cuiym/HttpToHttps.github.io/blob/master/imgs/Ctrl%2BR.png)

* 2、进入JDK安装路径 , 以我为例 : C:\Program Files\Java\jdk1.8.0_161\bin;<br>
进入该目录 , 执行如下命令 : `keytool -genkeypair -alias "tomcat" -keyalg "RSA" -keystore "D:\tomcat.keystore"` ;
![Image text](https://github.com/Vico-cuiym/HttpToHttps.github.io/blob/master/imgs/cmd.png)<br>
接着填写一些基本信息<br>
![Image text](https://github.com/Vico-cuiym/HttpToHttps.github.io/blob/master/imgs/keytool.png)<br>
简单介绍下填写的内容
<pre><code>密钥库口令:123456（这个密码非常重要 , 后面会用到）
名字与姓氏:127.0.0.1（以后访问的域名或IP地址，非常重要，证书和域名或IP绑定）
组织单位名称:anything（随便填 , 可以直接按回车）
组织名称:anything（随便填 , 可以直接按回车）
城市:anything（随便填 , 可以直接按回车）
省市自治区:anything（随便填 , 可以直接按回车）
国家地区代码:anything（随便填 , 可以直接按回车）</code></pre><br>
* 3、配置Tomcat服务器
    * 3.1、打开Tomcat中的配置文件 , 以我为例 : E:/WorkingSoftware/apache-tomcat-6.0.29/conf/server.xml
    <pre><code>&lt;!--
    &lt;Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"
        maxThreads="150" scheme="https" secure="true"
        clientAuth="false" sslProtocol="TLS"/&gt;
    --&gt;</code></pre>
    * 3.2、去掉注释且修改参数
    <pre><code>
    &lt;Connector port="8443" protocol="HTTP/1.1" SSLEnabled="true"
        maxThreads="150" scheme="https" secure="true"
        clientAuth="false" sslProtocol="TLS" 
        keystoreFile="D:/tomcat.keystore" keystorePass="123456789"/&gt;
    </code></pre>
    >其实就是添加`keystoreFile`和`keystorePass`两个属性 
    >>`keystoreFile`为生成证书地址 , 建议放在启动的Tomcat中
    >>`keystorePass`为密钥库口令
    * 3.3、注释Tomcat路径中conf\server.xml文件中下面一行。
    <code><pre>
    &lt;!--&lt;Listener SSLEngine="on" className="org.apache.catalina.core.AprLifecycleListener"/&gt;--&gt;
    </code></pre>
    * 最后访问链接：https://127.0.0.1:8443/项目名称/ , 就可以看见https的安全认证页面了
    ![Image text](https://github.com/Vico-cuiym/HttpToHttps.github.io/blob/master/imgs/Privatelink.png.png)<br>


# 🚨以下为参考文档🚨
[安装JDK的教程](https://jingyan.baidu.com/article/6dad5075d1dc40a123e36ea3.html)<br>
[安装Tomcat教程](https://jingyan.baidu.com/article/00a07f3872af0982d028dcb3.html)<br>
[keytool常见用法](https://www.cnblogs.com/benio/archive/2010/09/15/1826990.html)<br>
[Tomcat各属性详解](https://blog.csdn.net/weixin_33946605/article/details/92438866)<br>





