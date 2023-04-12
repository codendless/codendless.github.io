---
layout: post
title:  "Linux安装OpenCC中文简繁体转换"
date:   2023-04-12 16:02:07 +0800
categories: Linux
---
需要gcc（4.6以上），通过gcc -v查看当前的版本为4.4.7，安装前需先升级gcc


**1. 下载新版本gcc**

   ```shell
    wget ftp://ftp.mirrorservice.org/sites/sourceware.org/pub/gcc/
   ```

或者到https://gcc.gnu.org/查找较快的下载镜像

**2. 解压下载文件**

   ```shell
   tar -xf gcc-5.2.0.tar.bz2
   ```


**3. 下载gcc依赖文件和库**

解压完成后，执行以下命令进入工作目录：

```shell
cd gcc-5.2.0 
```

执行download_prerequisites脚本，下载gcc依赖文件和库：
```shell
./contrib/download_prerequisites download_prerequisites
```

会下载安装gcc所需的mpfr、gmp和mpc文件。


**4. 配置安装gcc**


建立一个输出目录，编译时所有生成的中间文件都放到该目录下：

```shell
mkdir gcc-temp 
```

工作目录切换至输出目录，并在其中执行配置和安装： 

```shell
cd gcc-temp 
```

执行configure配置安装文件：

```shell
 ../configure --enable-checking=release --enable-languages=c,c++ --disable-multilib
```

 配置完成后，执行以下命令，编译gcc： 

```shell
make or make -j4 
```

编译完成后，安装gcc：

```shell
 make install 
```

安装完成后还需要替换系统默认的gcc，执行以下命令，查找5.2版本的安装文件：

```shell
 ls /usr/local/bin | grep gcc 
```

输出如下： 

```shell
gcc
gcc-ar
gcc-nm
gcc-ranlib
x86_64-unknown-linux-gnu-gcc
x86_64-unknown-linux-gnu-gcc-5.2.0    //就是这个
x86_64-unknown-linux-gnu-gcc-ar
x86_64-unknown-linux-gnu-gcc-nm
x86_64-unknown-linux-gnu-gcc-ranlib
```

执行升级命令：

```shell
/usr/sbin/update-alternatives --install  /usr/bin/gcc gcc /usr/local/bin/x86_64-unknown-linux-gnu-gcc-5.2.0 52
```

**5. 验证安装 执行以下命令查看gcc版本：**

```shell
gcc -v 
```

执行以下命令查看g++版本：

```shell
g++ -v 
```

或使用 `which gcc` 查看gcc安装目录，在安装目录下执行-v命令。

例如，安装目录为/usr/local/bin/gcc：

`/usr/local/bin/gcc -v` 如果输出中有类似以下行，说明安装成功：

```shell
gcc 版本 5.2.0 (GCC) 
```

然后安装`OpenCC`
```shell
git clone https://github.com/BYVoid/OpenCC.git

cd OpenCC

make

sudo make install
```

**6. 安装opencc4php**

此步骤为安装openCC的php扩展，不需要可跳过

 ```shell
git clone https://github.com/NauxLiu/opencc4php.git

cd opencc4php

phpize

./configure

make && sudo make install
```


