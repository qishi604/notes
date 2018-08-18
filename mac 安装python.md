# mac 安装python

## 1. 去官网下载python安装程序
当前[官网](https://www.python.org/downloads/mac-osx/)最新的安装包为3.6

## 2. 安装

根据提示安装，安装目录为：

`
/Library/Frameworks/Python.framework/Versions
`

## 命令

3.6

```shell
# 1. 移动python
sudo mv /Library/Frameworks/Python.framework/Versions/3.6 /System/Library/Frameworks/Python.framework/Versions

# 2. 修改文件所属的Group
sudo chown -R root:wheel /System/Library/Frameworks/Python.framework/Versions/3.6

# 3. 更新一下Current的Link
sudo rm /System/Library/Frameworks/Python.framework/Versions/Current
sudo ln -s /System/Library/Frameworks/Python.framework/Versions/3.6 /System/Library/Frameworks/Python.framework/Versions/Current

# 4. 第五步，重新链接可执行文件
sudo rm /usr/bin/pydoc
sudo rm /usr/bin/python
sudo rm /usr/bin/pythonw
sudo rm /usr/bin/python-config

sudo ln -s /System/Library/Frameworks/Python.framework/Versions/3.6/bin/pydoc3.6 /usr/bin/pydoc
sudo ln -s /System/Library/Frameworks/Python.framework/Versions/3.6/bin/python3.6 /usr/bin/python
sudo ln -s /System/Library/Frameworks/Python.framework/Versions/3.6/bin/pythonw3.6 /usr/bin/pythonw
sudo ln -s /System/Library/Frameworks/Python.framework/Versions/3.6/bin/python3.6m-config /usr/bin/python-config

# 5. 更新一下.bash_profile文件
PATH="/System/Library/Frameworks/Python.framework/Versions/3.6/bin:${PATH}"
export PATH

```

2.7

```shell
# 1. 移动python
sudo mv /Library/Frameworks/Python.framework/Versions/2.7 /System/Library/Frameworks/Python.framework/Versions

# 2. 修改文件所属的Group
sudo chown -R root:wheel /System/Library/Frameworks/Python.framework/Versions/2.7

# 3. 更新一下Current的Link
sudo rm /System/Library/Frameworks/Python.framework/Versions/Current
sudo ln -s /System/Library/Frameworks/Python.framework/Versions/2.7 /System/Library/Frameworks/Python.framework/Versions/Current

# 4. 第五步，重新链接可执行文件
sudo rm /usr/bin/pydoc
sudo rm /usr/bin/python
sudo rm /usr/bin/pythonw
sudo rm /usr/bin/python-config

sudo ln -s /System/Library/Frameworks/Python.framework/Versions/2.7/bin/pydoc2.7 /usr/bin/pydoc
sudo ln -s /System/Library/Frameworks/Python.framework/Versions/2.7/bin/python2.7 /usr/bin/python
sudo ln -s /System/Library/Frameworks/Python.framework/Versions/2.7/bin/pythonw2.7 /usr/bin/pythonw
sudo ln -s /System/Library/Frameworks/Python.framework/Versions/2.7/bin/python2.7m-config /usr/bin/python-config

# 5. 更新一下.bash_profile文件
PATH="/System/Library/Frameworks/Python.framework/Versions/2.7/bin:${PATH}"
export PATH

```

## 脚本

```
#!/bin/bash
#python版号需要修改两个地方
#1. new_version

sudo -i
#得到超级权限
new_version="3.5"

PYPATH=/System/Library/Frameworks/Python.framework/Versions/"$new_version"
#第1步移动新版python到mac默认目录下
echo "move.."
mv /Library/Frameworks/Python.framework/Versions/"$new_version" /System/Library/Frameworks/Python.framework/Versions/
#第2步改变用户目录的用户组
echo "chown.."
chown -R root:wheel ${PYPATH}
#第3步 删除原来2.7的链接
echo "del.."
rm /System/Library/Frameworks/Python.framework/Versions/Current
#第4步重新链接到最新版本的python
echo "ln.."
ln -s ${PYPATH} /System/Library/Frameworks/Python.framework/Versions/Current
#第5步删除旧的命令符号链接
echo "rm.."
rm /usr/bin/{pydoc,python,pythonw,python-config}
echo "ln bin.."
#第6步重新建立新的命令符号链接
ln -s ${PYPATH}/bin/pydoc"$new_version" /usr/bin/pydoc
ln -s ${PYPATH}/bin/python"$new_version" /usr/bin/python
ln -s ${PYPATH}/bin/pythonw"$new_version" /usr/bin/pythonw
ln -s ${PYPATH}/bin/python"$new_version"m-config /usr/bin/python-config


python_param_list=`cd /usr/local/bin && ls -al |grep "Python"|awk 'ORS=" " {print $9}'`
#第7步修复其他链接
for i in $python_param_list;do
  echo "info: $i"
  rm -f /usr/local/bin/${i}
  ln -sv /System/Library/Frameworks/Python.framework/Versions/"$new_version"/bin/${i} /usr/local/bin/${i}
done

#第8步.环境变量要修改为最新的版本号
echo 'export PATH=/System/Library/Frameworks/Python.framework/Versions/3.4/bin:${PATH}' >> ~/.bashrc

exit #退出超级权限
```