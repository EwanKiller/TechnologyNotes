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

1. 方法一

`sudo gedit /etc/apt/apt.conf.d/proxy.conf`

加入以下内容：

```
Acquire {
  HTTP::proxy "http://127.0.0.1:port";
  HTTPS::proxy "http://127.0.0.1:port";
}
```

2. 方法二

apt-get命令后接上 `-o Acquire::http::proxy="http://127.0.0.1:41091/"`

## sudo dpkg -i xxx.deb 缺少依赖

`sudo apt-get -f -y install`


