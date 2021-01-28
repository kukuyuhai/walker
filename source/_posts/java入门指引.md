---
title: java入门指引
date: 2021-1-28 15:46:20
tags: java mysql
---

### 一、Mysql

#### 1. 下载地址

http://mirrors.163.com/mysql/Downloads/MySQL-5.7/

#### 2. 配置环境变量

**安装 mysql，查找到 mysql 安装目录，配置环境变量。**

#### 3. 配置 ini

```ini
[mysql]

# 设置mysql客户端默认字符集
default-character-set=utf8
[mysqld]
#设置3306端口
port = 3306
# 设置mysql的安装目录
basedir=C:\\Program Files\\MySQL\\MySQL Server 5.7
datadir=C:\\Program Files\\MySQL\\MySQL Server 5.7\\data
# 允许最大连接数
max_connections=200
# 服务端使用的字符集默认为8比特编码的latin1字符集
character-set-server=utf8
# 创建新表时将使用的默认存储引擎
default-storage-engine=INNODB
```

#### 4. 启动服务

```sh
mysqld --install  //，提示success。
mysql --initialize //初始化数据库data目录
net start mysql  // 启动mysql服务
```

#### 5. 初始化密码

```mysql
mysql> set password for root@localhost = password('123456');
```

### 二、Java

> vscode 安装配置

#### 1. jdk 下载安装

1. 新建\*.java 文件，vscode 会根据后缀提示安装 openJDK,按照配置提示 download 安装，安装完成后 restart vscode。

2. 打开`cmd`输入`java -version`打印如下即表示安装成功。

```shell
openjdk version "11.0.8" 2020-07-14
OpenJDK Runtime Environment AdoptOpenJDK (build 11.0.8+10)
OpenJDK 64-Bit Server VM AdoptOpenJDK (build 11.0.8+10, mixed mode)
```

3. 配置`JDK_HOME`和`JAVA_HOEM`环境变量

#### 2. maven 安装

下载地址：https://mirrors.bfsu.edu.cn/apache/maven/maven-3/3.6.3/binaries/apache-maven-3.6.3-bin.zip

#### 3. 配置环境变量

添加`MAVEN_HOME`进环境变量

#### 4. 配置阿里云镜像

修改目录下`D:\程序\maven\apache-maven-3.6.3\conf\settings.xml`文件，添加阿里云国内镜像。

```xml
<mirrors>
       <mirror>
            <id>alimaven</id>
            <mirrorOf>central</mirrorOf>
            <name>aliyun maven</name>
            <url>http://maven.aliyun.com/nexus/content/repositories/central/</url>
        </mirror>
        <mirror>
            <id>nexus-aliyun</id>
            <mirrorOf>*</mirrorOf>
            <name>Nexus aliyun</name>
            <url>http://maven.aliyun.com/nexus/content/groups/public</url>
        </mirror>

        <mirror>
            <id>repo1</id>
            <mirrorOf>central</mirrorOf>
            <name>Human Readable Name for this Mirror.</name>
            <url>http://repo1.maven.org/maven2/</url>
        </mirror>

        <mirror>
            <id>repo2</id>
            <mirrorOf>central</mirrorOf>
            <name>Human Readable Name for this Mirror.</name>
            <url>http://repo2.maven.org/maven2/</url>
        </mirror>

  </mirrors>

```

输入`mvn -version`出现如下表示安装 maven 成功

```shell
Apache Maven 3.6.3 (cecedd343002696d0abb50b32b541b8a6ba2883f)
Maven home: D:\程序\maven\apache-maven-3.6.3\bin\..
Java version: 11.0.8, vendor: AdoptOpenJDK, runtime: C:\Program Files\AdoptOpenJDK\jdk-11.0.8.10-hotspot
Default locale: zh_CN, platform encoding: GBK
OS name: "windows 10", version: "10.0", arch: "amd64", family: "windows"
```

#### 5. vscode 配置

##### setting.json 安装

```json
{
  "java.home": "C:\\Program Files\\AdoptOpenJDK\\jdk-11.0.8.10-hotspot",

  "java.project.importOnFirstTimeStartup": "automatic",

  "java.configuration.maven.userSettings": "D:\\程序\\mave\\apache-maven-3.6.3\\conf\\settings.xml",

  "maven.executable.path": "D:\\程序\\maven\\apache-maven-3.6.3\\bin\\mvn.cmd",

  "maven.terminal.useJavaHome": true,

  "maven.terminal.customEnv": [
    {
      "environmentVariable": "JAVA_HOME",

      "value": "C:\\Program Files\\AdoptOpenJDK\\jdk-11.0.8.10-hotspot"
    }
  ],

  "files.exclude": {
    "**/.classpath": true,

    "**/.project": true,

    "**/.settings": true,

    "**/.factorypath": true
  },

  "maven.excludedFolders": ["**/.*", "**/node_modules", "**/target", "**/bin"],

  "java.semanticHighlighting.enabled": true,

  "maven.pomfile.autoUpdateEffectivePOM": true,

  "java.configuration.runtimes": [
    {
      "name": "JavaSE-11",

      "path": "C:\\Program Files\\AdoptOpenJDK\\jdk-11.0.8.10-hotspot"
    }
  ]
}
```

##### 拓展安装

1. 在 Visual Studio Code 中打开扩展视图(Ctrl+Shift+X)。
2. 输入“java”搜索商店扩展插件。
3. 找到并安装 [Java Extension Pack (Java 扩展包)](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-pack)，如果你已经安装了 [Language Support for Java(TM) by Red Hat](https://marketplace.visualstudio.com/items?itemName=redhat.java)，也可以单独找到并安装 [Java Debugger for Visual Studio Code](https://marketplace.visualstudio.com/items?itemName=vscjava.vscode-java-debug) 扩展。
4. 输入“Spring Boot Extension”搜索商店扩展插件。
5. 找到并安装 “Spring Boot Extension Pack”。安装过程中可能会比较慢，耐心等待即可。

#### 6. 体验 demo

`ctrl+shift+p`输入 spring 选择`Spring initialzr: Create a maven Project`按照提示选择轻松开始 java 开发之旅。

![image-20200904171342196](D:\Typora文档\typora-user-images\image-20200904171342196.png)
