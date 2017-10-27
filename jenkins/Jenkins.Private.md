# Jenkins

## 描述

Jenkins是一个基于Java开发的开源软件项目，旨在提供一个开放易用的软件平台，使软件的持续集成变成可能。点击标题查看更多信息与使用方法。


## 简介
Jenkins是一个基于Java开发的开源软件项目，旨在提供一个开放易用的软件平台，使软件的持续集成变成可能。Jenkins用于监控持续重复的工作，功能包括：持续的软件版本发布/测试项目以及监控外部调用执行的工作。


## 用法
Jenkins镜像的使用方法：
1. 部署镜像后，需要添加80端口到 Azure 安全组 入栈规则。<br/>
2. 访问 http://{真实FQDN}/ 即可打开登陆界面，初始用户名和密码都是admin，登陆之后，请及时修改您的管理员密码。

## 信息

|Key|Value|
|---|-----|
|镜像名称|Jenkins-2.86 (Centos7.3 Base)|
|镜像位置|https://community49d5320c5aeec4c.blob.core.chinacloudapi.cn/sxz-vhds/Jenkins-2.86-os-2017-10-27-9B36734.vhd |
|发布链接||
|上传日期|2017-10-27 7:00:00 UTC|
|版本|Jenkins-2.86|
|来源|http://pkg.jenkins-ci.org/redhat |
|平台|Centos7.3|
|软件包|jenkins,nginx|
|标签|jenkins,nginx|
|端口|80, 22|
|配置文件|/etc/default/jenkins /etc/init.d/jenkins|
|服务控制|sudo service jenkins (start,stop,restart,status)|

## 安装步骤


#### 一、原始镜像：Centos7.3 Base（Centos官方7.3版）
#### 二、安装步骤

	1. 从http://pkg.jenkins-ci.org/redhat/下载最新的rpm镜像包，我选择最新版（2.54以上需要Java1.8）。
	2. 执行rpm包安装：rpm -ivh ./jenkins-2.86-1.1.noarch.rpm
	3. 查询安装位置：rpm -ql jenkins-2.86-1.1.noarch
	4. 安装Java1.8：yum install java-1.8.0-openjdk.x86_64
	5. 查询Java位置 ：whereis java，比如返回： /usr/bin/java  如果是自己定义的Java位置，请修改追加vim /etc/init.d/jenkins 中的描述的java位置。
	6. 启动Jenkins：/etc/init.d/jenkins start
	7. 复制Jenkins.war到Tomcat的WebApps目录：复制/usr/lib/jenkins/jenkins.war到/usr/local/tomcat/webapps/。


#### 三、安装NGINX

	1. 打开 http://nginx.org/download/ 查找最新版本Nginx。
	2. cd /opt/
	3. wget http://nginx.org/download/nginx-1.13.6.tar.gz
	4. 解压Nginx：tar -zvxf ./nginx-1.13.6.tar.gz
	5. 安装编译依赖：yum -y install openssl openssl-devel zlib pcre 
	6. yum install gcc-c++.x86_64
	7. cd ./nginx-1.13.6
	8. ./configure --prefix=/usr/local/nginx
	9. make
	10. make install
	11. 链接到常规目录下：ln /usr/local/nginx/sbin/nginx /sbin/nginx
	12. 设置为开机启动：vim /etc/rc.d/rc.local，添加一行：/usr/local/nginx/sbin/nginx
	13. 执行：chmod +x /etc/rc.d/rc.local 确保nginx以管理员权限运行。
	14. 修改Nginx配置：vim /usr/local/nginx/conf/nginx.conf，然后找到 “location /{}"节，在括号内写入：
	    location / {
            proxy_http_version 1.1;
            proxy_pass http://127.0.0.1:8080;
            proxy_redirect off; 
            proxy_set_header Host $host; 
            proxy_set_header X-Real-IP $remote_addr;
            proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for; 
        }
	16. 保存并退出。

#### 四、打开网站

	1. 初始访问端口：8080，但是因为安装了NGINX转发，所以使用80端口访问。
	2. 初始用户名为"admin"，初始密码所在文件：/var/lib/jenkins/secrets/initialAdminPassword，初始已对密码进行修改，因此密码为"admin"

 


## 与其它镜像的区别

##### Bitnami部署中的特点

    1、国际版中，使用Bitnami部署Jenkins之后，需要通过Azure Portal中查看【Boot diagnostics】来查看登陆密码，用户名为admin。

    2、使用密码登陆后，再选择要安装的插件。

##### Microsoft部署的Jenkins的特点
    
    1、国际版中，使用微软发部的镜像部署Jenkins之后，它会自动安装Nginx，使用浏览器对80端口进行访问时，会打开Nginx的一个页面，页面中指示，如果不使用HTTPS服务，提示卸载Nginx，否则需要配置Nginx以支持443端口的HTTPS协议。

    2、打开Jenkins的8080端口相应页面时，需要通过SSH连接虚拟机并从“/var/lib/jenkins/secrets/initialAdminPassword”中查看密码。