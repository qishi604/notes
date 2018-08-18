# Mac 文件夹读写权限

```shell
sudo chown -R :admin /Library/WebServer/Documents

sudo chmod -R g=rw,+X /Library/WebServer/Documents
```