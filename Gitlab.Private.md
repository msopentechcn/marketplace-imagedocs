# GitLab

## Abstract
GitLab 是一个利用 Ruby on Rails 开发的开源应用程序，实现一个自托管的 Git 项目仓库，可通过 Web 界面进行访问公开的或者私人项目。打开[官方文档](https://docs.gitlab.com/ce/README.html)可以查看更多信息与配置使用方法。

## Introduction
GitLab https://about.gitlab.com/ 是一个利用 Ruby on Rails 开发的开源应用程序，实现一个自托管的 Git 项目仓库，可通过 Web 界面进行访问公开的或者私人项目。它拥有与Github类似的功能，能够浏览源代码，管理缺陷和注释。可以管理团队对仓库的访问，它非常易于浏览提交过的版本并提供一个文件历史库。团队成员可以利用内置的简单聊天程序(Wall)进行交流。
## Requirements
**CPU：** <br/>
Gitlab对CPU的要求，我们建议您：
- 1个内核最多支持100个用户，但是由于所有系统任务都运行在同一个核心上，所以应用程序可能会慢一些
- 2个内核是推荐的内核数量，最多可支持500个用户
- 4个内核最多支持2,000个用户
- 8个内核最多支持5,000个用户
- 16个内核最多支持10,000个用户
- 32个内核最多支持20,000个用户
- 64个内核最多支持4万个用户

**Memory：** <br/>
您需要至少4GB的可寻址内存（RAM +交换）来安装和使用GitLab！操作系统和任何其他正在运行的应用程序也将使用内存，因此请记住，在运行GitLab之前，您至少需要4GB可用。使用较少的内存GitLab将在重新配置运行期间给出奇怪的错误，并在使用过程中出现HTTP 500错误。

- 1GB RAM + 3GB的交换是绝对最小值，但我们强烈建议不要这么小的内存。
- 2GB RAM + 2GB交换最多可支持100位用户，但速度非常慢
- 4GB RAM是安装的推荐内存大小，最多可支持100个用户
- 8GB RAM最多支持1,000个用户
- 16GB RAM最多可支持2,000位用户
- 32GB内存最多可支持4,000位用户
- 64GB RAM最多支持8,000个用户
- 128GB RAM最多支持16,000个用户
- 256GB内存最多支持32,000个用户

**Storage Disk：** <br/>
必要的硬盘驱动器空间在很大程度上取决于您希望存储在GitLab中的资源的大小。另外，如果您有足够的RAM内存和最近的CPU，则GitLab的速度主要受硬盘搜索时间的限制。固态驱动器（SSD）将提高GitLab的响应能力。
默认部署镜像后VM的存储磁盘容量为30G，如果后期需要更多空间，您可以附加额外的存储硬盘，并通过管理员帐号登陆Gitlab来配置Repository Storage存储位置来解决。


## Usage
Gitlab镜像的使用方法：
1. 部署镜像后通过 ssh 登录：<br/>

``` Bash
sudo vim /etc/hostname，更新为所部署虚拟机的真实 FQDN（如mygitlab.chinanorth.cloudapp.chinacloudapi.cn）
sudo vim /etc/gitlab/gitlab.rb，更新 external_url 为外部域名，如 'http://mygitlab.norch.chinacloudapp.cn' 
```

修改完成后，执行下面命令启动Gitlab。

``` Bash
sudo gitlab-ctl reconfigure
sudo gitlab-ctl restart
```

2. 添加8080端口到您的` Azure 网络安全组`的入站终结点中，您可以参考文章[如何在 Azure 中的 Linux 经典虚拟机上设置终结点](https://docs.azure.cn/zh-cn/virtual-machines/linux/classic/setup-endpoints)。<br/>
3. 访问 http://{真实FQDN}/ 以登录和使用 Git 服务器。首次访问时请输入任意合理的密码以初始化管理员账号。默认用户名为 root，输入之前设定的密码登录后可修改用户名。

## Metadata

|Key|Value|
|---|-----|
|镜像名称|GitLab-CE-10.0.3 (Centos7.3 Base)|
|镜像位置||
|发布链接||
|上传日期|2017-10-19 7:00:00 UTC|
|版本|Community Edition 10.0.3|
|来源|https://about.gitlab.com/downloads/#centos7 |
|平台|Centos7|
|软件包|postgresql,gitlab,gitlab-ce-10.0.3|
|标签|gitlab,collaboration,git,scm,git-scm,open-source|
|端口|80, 22|

## Installation

##### 原始镜像：
    在Azure中搜索镜像Centos7.3 Base（Centos官方7.3版）创建Centos7.3作为基础镜像。
##### 安装步骤：

	1. yum -y install policycoreutils-python policycoreutils openssh-server openssh-clients postfix
	2. 选择下载最新版本的rpm离线包，为快速下载，可以使用[清华大学镜像源下载最新rpm离线包](https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7)。
	3. wget https://mirrors.tuna.tsinghua.edu.cn/gitlab-ce/yum/el7/gitlab-ce-10.0.3-ce.0.el7.x86_64.rpm
	4. 安装gitlab-ce包：rpm -i gitlab-ce-10.0.3-ce.0.el7.x86_64.rpm
	5. 配置外网地址：vim /etc/gitlab/gitlab.rb 修改：external_url 为：external_url 'http://<you host dns>'; 如果是镜像制作，则此步可省略。
	6. 运行编译配置并启动：gitlab-ctl reconfigure
	7. 如果需要重启Gitlab，可以执行：gitlab-ctl restart
	8. 访问http://songxingzhu.chinacloudapp.cn并登陆，登陆用户名为root，密码首次使用时提示设置。

##### 注意事项：
    1、每次修改/etc/gitlab/gitlab.rb文件后，需要执行gitlab-ctl reconfigure和gitlab-ctl restart才能生效。
    2、测试Gitlab邮箱发送功能可以使用下列命令：
	（1） 命令：gitlab-rails console 进入命令行。
	（2） 执行：Notify.test_email('xingzhu.song@yungoal.com','Message Subject','Message Body').deliver_now