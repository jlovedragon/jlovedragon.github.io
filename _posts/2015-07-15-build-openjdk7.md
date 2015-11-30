---
layout: post
title: Ubuntu14.04编译OpenJDK7
categories: [JAVA]
---

最近在看[深入理解Java虚拟机](http://book.douban.com/subject/6522893/)，第一章有讲到自己编译OpenJDK7，故用Ubuntu14.04实践下。

## 构建编译环境
### JDK1.7.0_45
到oracle官网上下载JDK1.7.0_45，解压到指定目录。

```sh
sudo tar zxvf jdk-7u45-linux-x64.tar.gz -C /usr/lib/jvm
```

### Ant1.9.6

```sh
sudo tar zxvf apache-ant-1.9.6-bin.tar.gz -C /usr/lib/ant
```

### 配置环境变量
/etc/profile

```sh
export JAVA_HOME=/usr/lib/jvm/jdk1.7.0_45  
export ANT_HOME=/usr/lib/ant/apache-ant-1.9.6
export CLASSPATH=.:$JAVA_HOME/lib/dt.jar:$JAVA_HOME/lib/tools.jar    
PATH=$CLASSPATH:ANT_HOME/bin:$JAVA_HOME/bin:$PATH  
export PATH 
```

`source /etc/profile`使环境变量生效
## 编译
### 编写编译脚本
在下载的OpenJDK7的源码目录下新建build.sh，编写如下脚本

```sh
#!/bin/bash  
#设置语言  
export LANG=C   
# JDK安装路径，必须设置
export ALT_BOOTDIR=/usr/lib/jvm/jdk1.7.0_45
  
#允许自动下载依赖包  
export ALLOW_DOWNLOADS=true  
  
#使用预编译头文件，不加这个编译会更慢一些 
export USE_PRECOMPILED_HEADER=true  
  
#要编译的内容  
export BUILD_LANGTOOLS=true  
export BUILD_JAXP=false
export BUILD_JAXWS=false
export BUILD_CORBA=false  
export BUILD_HOSTPOT=true  
export BUILD_JDK=true  
  
#要编译的版本  
export SKIP_DEBUG_BUILD=false  
export SKIP_FASTDEBUG_BUILD=true  
export DEBUG_NAME=debug  
  
#把它设置为FALSE可以避免javaws和浏览器Java插件之类的部分build  
BUILD_DEPLOY=false  
  
#把它设置为false就不会build出安装包。因为安装包里有一些奇怪的依赖  
#但即便不build出它也已经得到完整的JDK镜像,所以还是不用build它  
BUILD_INSTALL=false  ：
  
#存放编译结果  
export ALT_OUTPUTDIR=/home/mango/setup/openjdk/build
  
unset CLASSPATH  
unset JAVA_HOME  
#make sanity && make  
make 2>&1 | tee $ALT_OUTPUTDIR/build.log 
```
### 编译
`./build.sh`
