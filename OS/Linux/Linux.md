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



## 终端设置代理

1. `sudo vim ~/.bashrc`

2. 在末尾加上

   ```bash
   export http_proxy='http://127.0.0.1:41091'
   export https_proxy='http://127.0.0.1:41091'
   ```

   

3. `source ~/.bashrc`



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



## JDK安装和设置

1. 前往oracle Java官网下载JDK（http://www.oracle.com/technetwork/java/javase/downloads/index.html）

2. 创建/解压

   ```bash
   // 创建目录
   sudo mkdir /usr/lib/jvm
   
   // 解压
   sudo tar -zxvf jdkxxx.gz -C /usr/lib/jvm
   ```

3. 修改环境变量

```bash
// 打开 bashrc
sudo vim ~/.bashrc
// 修改 bashrc内容
# set oracle jdk env
export JAVA_HOME=/usr/lib/jvm/jdk1.8.0_333
export JRE_HOME=${JAVA_HOME}/jre
export CLASSPATH=.:${JAVA_HOME}/lib:${JRE_HOME}/lib
export PATH=${JAVA_HOME}/bin:$PATH
// 生效 bashrc
source ~/.bashrc

// 系统注册jdk
sudo update-alternatives --install /usr/bin/java java /usr/lib/jvm/jdk1.8.0_333/bin/java 300

// 查看java版本
java -version

// 多版本jdk时可以选择
sudo update-alternatives --config java
```



## 执行脚本报 ’/usr/bin/env: sh\r":没有那个文件或目录

- vim打开文件
- `set ff`
- `set ff=unix`
- `wq!`
