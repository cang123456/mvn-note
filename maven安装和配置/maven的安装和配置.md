1. maven下载

   ```c
   下载maven版本要根据java版本
   确定版本：
   https://maven.apache.org/docs/history.html
   下载地址
   https://maven.apache.org/download.cgi
   
   在 Windows 系统下，下载 apache-maven-3.9.11-bin.zip（即 “Binary zip archive” 对应的 zip 格式二进制包）。
   ```

2. 解压

3. 配置环境变量，复制bin文件夹路径到path

4. 打开cmd验证，输入

   ```c
   mvn -v
   查看maven版本是否正确
   ```

5. 配置本地仓库

   ```
   1. 在磁盘中新建一个文件夹mvnrep，作为仓库
   2. 进入maven的conf文件夹，编辑settings.xml
   3. 找到 localRepository 标签（被注释掉了）位置，在 localRepository 标签下配置 maven 仓库位置
   
   values (values used when the setting is not specified) are provided.
   -->
   <settings xmlns="http://maven.apache.org/SETTINGS/1.2.0"
   xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
   xsi:schemaLocation="http://maven.apache.org/SETTINGS/1.2.0 https://maven.apache.org/xsd/settings-1.2.0.xsd">
   
   <!-- localRepository
   | The path to the local repository maven will use to store artifacts.
   |
   | Default: ${user.home}/.m2/repository
   <localRepository>/path/to/local/repo</localRepository>
   -->
   
   ```

   配置本地仓库：<img alt="image-20250914112843608" src="maven的安装和配置.assets\image-20250914112843608.png"/>

   然后复制路径：

   <img alt="image-20250914113144076" src="maven的安装和配置.assets/image-20250914113144076-1757825476091.png"/>

   到这里，本地仓库就配置好了

6.  配置阿里镜像
   进入 maven 的 conf 文件夹，编辑 settings.xml（与第 5 步相同）
   找到 mirrors 标签，在 mirrors 标签下面添加以下配置：
   
   ```
   <mirror>
   <id>alimaven</id>
   <name>aliyun maven</name>
   <url>http://maven.aliyun.com/nexus/content/groups/public/</url>
   <mirrorOf>central</mirrorOf>
   </mirror>
   ```
   
7. idea配置教程

8. <img alt="image-20250914114644438" src="maven的安装和配置.assets/image-20250914114644438.png"/>

   打开设置，搜maven，然后按照截图配置


# 补充重点：每次新建项目导入时，都要配置maven仓库路径，否则会报错。这样很麻烦
```
解决方法：将settings.xml配置文件复制到，idea默认读取的那个目录下面
一般是
c盘，用户，用户名，m2，复制到这里就行了
```
