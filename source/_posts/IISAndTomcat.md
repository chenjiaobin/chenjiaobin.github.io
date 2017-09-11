---
title: IIS和tomcat公用80端口
date: 2017-09-11 13:10:00
tags: 
- IIS
- Tomcat
- 端口
categories:
- 配置
---
### 前言
很多时候，我们需要在一台服务器上部署多个网站并解析域名，但是域名访问默认是80端口，所以就需要解决一台服务器公用80端口的问题，下面我就向大家介绍一下配置过程
### 原理
在IIS使用isapi_redirct转发给tomcat处理
### 环境
windows 7,  iis7,  apache-tomcat-7.0.53
### 安装
1.正确安装IIS
2.正确安装tomcat
3.下载isapi_redirct
下载地址：http://www.apache.org/dist/tomcat/tomcat-connectors/jk/binaries/windows/
特别注意，32位及64位系统使用对应的dll，否则会出现类似  调用GetFilterVersion失败 这样的异常
32位使用：tomcat-connectors-1.2.40-windows-i386-iis.zip
64位使用：tomcat-connectors-1.2.40-windows-x86_64-iis.zip
解压得到isapi_redirec的dll,将其重命名为：isapi_redirect.dll
### 配置isapi_redirct
1.在Tomcat的安装文件夹【tomcat_home】下面新建文件夹jakarta，将isapi_redirect.dll 文件拷贝到里面
2.将isapi_redirect.dll文件拷贝到 【tomcat_home】/conf/ 文件夹下面
3.在【tomcat_home】/conf/ 文件夹下面(isapi_redirect.dll文件在里面)， 新建立一个与dll文件名相同，扩展名为properties的配置文件，即：isapi_redirect.properties，isapi_redirector.dll初始化时，默认会在自己所在的目录寻找同名的配置文件，如果没有再到注册表中读取配置信息.
isapi_redirect.properties的内容：
```
extension_uri=/jakarta/isapi_redirect.dll  
log_file=D:/sw/tomcat7/logs/isapi_redirect.log  //这个是你自己tomcat的logs路径
log_level=debug  
worker_file=D:/sw/tomcat7/conf/workers.properties    //重新修改成你自己的路径
worker_mount_file=D:/sw/tomcat7/conf/uriworkermap.properties  //重新修改成你自己的路径
```
特别特别特别注意
isapi_redirect.properites的文件需要注意编码问题，在本次测试中我使用ansi编码才行，之前使用utf-8编码存储的时候，发生调用 GetFilterVersion 失败
4.在【tomcat_home】/conf/ 文件夹下面新建workers.properties文件，workers.properties内容
```
workers.tomcat_home=D:\sw\tomcat7\       //自己tomcat的路径
workers.java_home=C:\Program Files\java\jdk.1.7.0_67   //你的jdk的路径
ps=/  
  
#testiistom、examples为访问Tomcat服务器的一个标签,   
#对应【tomcat_home】/webapps/文件夹下面的testiistom和examples文件夹，可以设置多个,用逗号隔开  
worker.list=testiistom,examples  
  
worker.testiistom.type=ajp13  
worker.testiistom.host=localhost  
worker.testiistom.port=8009  
worker.testiistom.lbfactor=1  

worker.examples.type=ajp13  
worker.examples.host=localhost  
worker.examples.port=8009  
worker.examples.lbfactor=1  
```
5.在【tomcat_home】/conf/ 文件夹下面新建uriworkermap.properties文件，uriworkermap.properties内容：
```
/testiistom/*=testiistom  
/examples/*=examples 
```
注意：如果这样配置在最后输入域名访问到的是IIS页面的话，就重新配置成如下：
```
/*=testiistom  #必须要
/system/=testiistom #访问system目录时转到Tomcat服务器处理（可有可无）
/system/*.jsp=testiistom #system下.jsp文件转到Tomcat服务器处理（可有可无）
/system/*=testiistom  #system下所有文件转到Tomcat服务器处理（可有可无）
```
意思是去这个域名下面的所有文件转发给tomcat处理
说明：等号左边是路径规则，符合此规则的就通过Connector转发给tomcat。比如说在浏览器上访问   http://localhost/examples/，符合  /example/*的规则，那么它就会转发给Tomcat对应的worker，转发目标worker名称为examples
等号右边即是workers.properties文件定义的worker

### 配置IIS
因为公用80端口的这个方法是通过iis来转发给tomcat的，所以需要配置IIS
1.打开IIS，点击电脑节点，即最顶端那个节点
2.在中间区域框中找到【ISAPI和CGI限制】，双击它
3.双击【ISAPI和CGI限制】后，在右边框点击【添加】
      ISAPI或CGI路径(I)：选择【tomcat_home】\conf\isapi_redirect.dll
      描述：jakarta
      允许执行扩展路径(A)：要勾选
4.打开IIS，点击网站下面的【Default Web Site】节点
5.中间区域框找到【ISAPI筛选器】，双击它
6.双击【ISAPI筛选器】后，在右边框点击【添加】
     筛选器命名(F)：jakarta
     可执行文件(E)：选择【tomcat_home】\conf\isapi_redirect.dll
7.打开IIS，点击网站下面的【Default Web Site】节点
8.在中间区域框找到【处理程序映射】，双击它
9.双击【处理程序映射】后，在右边框点击【添加脚本映射...】
      请求路径：  *.jsp
      可执行文件(E)：选择【tomcat_home】\conf\isapi_redirect.dll
      名称：JSP
10.打开IIS，右键点击网站下面的【Default Web Site】-》添加虚拟目录
      名称：jakarta
      物理路径：【tomcat_home】\conf
      说明：在你需要整合Tomcat的网站（例如Default Web Site）下面，都要建立jakarta虚拟目录，并指向【tomcat_home】\conf，这个虚拟目录的名称必须与  五：配置isapi_redirec第二步 中配置的extension_uri的名称相同
11.打开IIS，点击网站下面的【Default Web Site】节点下面的  jakarta 虚拟目录节点
   在中间区域框找到【处理程序映射】，双击它，然后在右边框找到【编辑功能权限】，将所有的权限都勾选
### 测试
1.重新启动IIS和tomcat
2.访问你的域名，正常的话是可以访问，那就成功了，如果显示的页面是IIS页面，那就按我上面的那一个【配置isapi_redirect的第五步重新配置一下】重新配置
3.如果还是访问不到，那就域名+你的项目名，访问正常的话就OK了，那就去tomcat配置一下你的context内容

[参考文章1](http://blog.csdn.net/baijinwen/article/details/50562996)[参考文章2](http://www.jb51.net/article/109723.htm)

作者：陈焦滨#kevin

