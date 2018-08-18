# Linux 安装Python

编译python 前需要先安装 编译环境

使用  `yum install gcc gcc-c++ autoconf automake` 来安装编译环境

```shell

wget https://www.python.org/ftp/python/2.7.13/Python-2.7.13.tgz

# 解压python
tar xzf Python-2.7.13.tgz
cd Python-2.7.13

# 编译安装python
./configure --prefix=/usr/local/python2.7
make
make install

# 创建一个python2.6的链接
ln -sf /usr/local/python/bin/python2.7 /usr/bin/python2.7

```

