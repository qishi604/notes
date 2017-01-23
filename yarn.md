# yarn

## yarn 用法

### 开始新项目

```shell
yarn init
```

### 添加依赖

```shell
yarn add [package]
yarn add [package]@[version]
yarn add [package]@[tag]
```

### 删除依赖

```shell
yarn remove [package]
```

### 安装项目所有依赖

```
yarn
```
 or
  
 ```shell
 yarn install
 ```

### 从npm迁移到yarn

如果你想从现有的`npm`项目使用`yarn`, 只需执行：

```shell
yarn
```

### npm & yarn 命令对比

npm                                                    |                        yarn
--------------------------------|-----------------------------------
npm install			                           | 			yarn install
(N/A)				                           | 		yarn install --flat
(N/A)				                           | 		yarn install --har
(N/A)				                           |    	yarn install --no-lockfile
(N/A)					                     | 		yarn install --pure-lockfile
npm install [package]                           |  	     (N/A)
npm install --save [package]                |      yarn add [package]  
npm install --save-dev [package]         |     yarn add [package] [--dev/-D]
(N/A)				                           |      yarn add [package] [--peer/-P]
npm install --save-optinal [package]     |      yarn add [package] [--optional/-O]
npm install --save-exact [package]       |      yarn add [package] [--exact/-E]
(N/A)				                           |    yarn add [package] [--tilde/-T]
npm install --global [package]              |     yarn global add [package]
npm rebuild                                        |    yarn install --force
npm uninstall [package]                       |    (N/A) 
npm uninstall --save [package]            |    yarn remove [package]
npm uninstall --save-dev [package]     |    yarn remove [package]
npm uninstall --save-optional [package]  |  yarn remove [package]
npm cache clean                                  |    yarn cache clean
rm -rf node_modules && npm install     |      yarn upgrade

## 参考

[yarnpkg.org](https://yarnpkg.com/en/docs/cli/)

