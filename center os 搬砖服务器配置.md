# center os 搬砖服务器配置


## 安装`gcc`

`yum install gcc-c++`

## 编译安装nodejs 最新版

```
wget https://nodejs.org/dist/v8.9.3/node-v8.9.3.tar.gz

tar zxvf node…

cd node..

./configure

make

Make install

```

## 安装 `forever`

`npm install forever -g`

## 安装 `ccxt`

```
git clone https://github.com/ccxt/ccxt.git
cd ccxt
npm install
npm install nodemailer
```