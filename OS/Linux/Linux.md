# Linux日常设置

## 设置777权限

sudo chmod 777  <filepath>

## 查看文件/文件夹权限

ls -l

## 查看当前路径

pwd

## 查看文件

less <filename>

## 复制文件

cp  <filename>  <destination path>

## 终端apt设置代理

`sudo gedit /etc/apt/apt.conf.d/proxy.conf`

加入以下内容：

```
Acquire {
  HTTP::proxy "http://127.0.0.1:port";
  HTTPS::proxy "http://127.0.0.1:port";
}
```



