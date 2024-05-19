# 在服务器上部署项目


我购买的是阿里云的学生机，操作系统是CentOS
部署的项目是Nodejs和React的前后端分离项目，需要安装配置nodejs，mysql，nginx等
<img src="https://wx2.sbimg.cn/2020/07/08/C9tBJ.jpg">
<!--more-->

## 首先安装nvm

安装git
```bash
yum install git -y
```

下载nvm
```bash
git clone git://github.com/creationix/nvm.git ~/nvm
```

设置nvm到bash
```bash
echo "source ~/nvm/nvm.sh" >> ~/.bashrc
source ~/.bashrc
```

查询node版本
```bash
nvm list-remote
```
安装node.js
```bash
nvm install v12.0.0
```
使用nodejs
```bash
nvm use v12.0.0
```

## 安装nginx

```bash
yum -y install nginx
```
nginx相关操作
```bash
nginx -h    #查看帮助
nginx -c filename  #设置配置文件并启动nginx
nginx -t    #测试配置文件是否正确 
```

## 安装mysql

下载mysql5.7
```bash
wget https://dev.mysql.com/get/mysql57-community-release-el7-9.noarch.rpm
```

进行repo安装
```bash
rpm -ivh mysql57-community-release-el7-9.noarch.rpm
```
进入到 /etc/yum.repos.d/目录，执行
```bash
yum install mysql-server
```

启动mysql
```bash
systemctl start mysqld
systemctl restart mysqld # 重启mysql
```

获取临时密码
```bash
grep 'temporary password' /var/log/mysqld.log
```

登录mysql，输入刚刚获取到的临时密码
```bash
mysql -u root -p
```
设置密码强度要求
```bash
set global validate_password_policy=LOW;
```

更改密码
```bash
ALTER USER 'root'@'localhost' IDENTIFIED BY '123456';#把123456替换为你自己的密码
```

## 设置mysql默认编码为utf8
```bash
sudo vim /etc/my.cnf
```
1. 在[client]字段里加入default-character-set=utf8 
2. 在[mysql]字段里加入default-character-set=utf8
3. 在[mysqld]字段里加入character-set-server=utf8

## 安装pm2
```bash
npm install -g pm2
```
pm2 常用命令
```bash
pm2 start app.js --name my-api # 命名进程
pm2 list               # 显示所有进程状态
pm2 logs               #  显示所有进程日志
pm2 stop all           # 停止所有进程
pm2 restart all        # 重启所有进程
pm2 stop 0             # 停止指定的进程
pm2 restart 0          # 重启指定的进程
pm2 delete 0           # 杀死指定的进程
```



## 部署项目到nginx，并使用nginx反向代理
把项目从github上clone下来，前端项目执行`npm build`后
可以在`/etc/nginx/conf.d/`目录下新建一个react-blog.conf，然后编辑这个文件
配置如下:
```
server {
    listen      7890;#服务器内部使用的端口号
    server_name 127.0.0.1;#(自己的服务器IP)

    root    /home/user/code/koa-blog/react-blog/build/;#build后的目录
    index   index.html;
}
```
然后打开`/etc/nginx/nginx.conf`这个文件
在server下再加上

```
server {
    listen 8078;
    server_name example.com;
    location / {
        proxy_pass http://localhost:8078;
    }
}
```
要在服务器提供商的管理网页中开放8078端口。
由于使用80端口需要备案，所以可以先用其他端口代替

后端使用pm2进行运行，假设运行后端程序提供的端口为6543
那么该端口也可以通过nginx进行反向代理
```
server {
    listen 8078;
    server_name api.example.com;
    location / {
        proxy_pass http://localhost:6543;
    }
}

```
所以对外部统一保留8078端口即可，可以根据不同的域名来访问不同的服务。

参考链接：
[mysql安装](https://blog.csdn.net/wohiusdashi/article/details/89358071)
[mysql编码设置](https://www.cnblogs.com/roujiamo/p/10824511.html)
[pm2相关](https://www.cnblogs.com/i6010/articles/10857543.html)
