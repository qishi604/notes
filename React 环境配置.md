## 开始 

### 安装环境

#### 安装[nodejs](https://nodejs.org/zh-cn/download/)

使用`brew`安装

```
brew update
brew install node
```

`npm` 配置[淘宝镜像](https://npm.taobao.org/)

环境变量添加`alias`

```
alias npm="npm --registry=https://registry.npm.taobao.org \
--cache=$HOME/.npm/.cache/cnpm \
--disturl=https://npm.taobao.org/dist \
--userconfig=$HOME/.cnpmrc"
```

或者在终端执行以下命令(注意：使用`bash`的需要替换下面的`. zshrc`为`. bashrc `)

```
$ echo '\n#alias for npm\nalias cnpm="npm --registry=https://registry.npm.taobao.org \
  --cache=$HOME/.npm/.cache/cnpm \
  --disturl=https://npm.taobao.org/dist \
  --userconfig=$HOME/.cnpmrc"' >> ~/.zshrc && source ~/.zshrc
```

### 项目目录下，执行

`npm install` or `npm i`

### IDE

- webstorm 类似于Android studio (收费)
- sublime 
- atom 

### 发布

```shell
./release.sh
./publish.sh
```

将*版本号*、*dist.zip* 发给api