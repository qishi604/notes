# vps相关

## ssh登录vps
```
ssh root@23.83.254.31 -p 26045
```

## 上传文件

```
scp -P 26045 proxy.pac root@23.83.254.31:/home/tools

# pass : A19
```


http://litaook.com/blog/2015/12/04/build-a-proxy-server-to-through-the-wall/


## 安装IPsec
[github](https://github.com/quericy/one-key-ikev2-vpn)

```
wget --no-check-certificate https://raw.githubusercontent.com/quericy/one-key-ikev2-vpn/master/one-key-ikev2.sh && bash one-key-ikev2.sh
```

```
#############################################################
#
# [Install Complete]
# Version:1.2.0
# There is the default login info of your IPSec/IkeV2 VPN Service
# UserName: myUserName
# PassWord: myUserPass
# PSK: myPSKkey
# you should change default username and password in /usr/local/etc/ipsec.secrets
# you cert: /my_key/ca.cert.pem 
# you must copy the cert to the client and install it.
#
#############################################################
```

## ipsec 常用命令

```
ipsec start   #启动服务
ipsec stop    #关闭服务
ipsec restart #重启服务
ipsec reload  #重新读取
ipsec status  #查看状态
ipsec --help  #查看帮助
```

23.83.254.31