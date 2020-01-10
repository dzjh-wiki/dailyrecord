# Centos7.2服务器的软件安装

----
## 0 安装常用开发编译工具包
```
yum groupinstall "Development Tools"
```
### 安装使用rz/sz命令
```
yum install -y lrzsz
```

## 1 MySQL
### 安装MySql的开发工具
```
yum -y install mysql-devel
```
### 安装步骤：
  * 下载MySQL源：`wget http://dev.mysql.com/get/mysql57-community-release-el7-8.noarch.rpm`
  * 安装`MySQL`源：`rpm -ivh mysql57-community-release-el7-8.noarch.rpm`
  * 安装`mysql-community-server`：`yum install mysql-community-server`
  * 启动`MySQL`服务：`systemctl start mysqld`
  * 查看`MySQL`的启动状态：`systemctl status mysqld`
  * 开机启动：`systemctl enable mysqld`
  * 配置默认编码为`utf8`：修改/etc/my.cnf配置文件，在[mysqld]下添加编码配置=>
```
  [mysqld]
  character_set_server=utf8
  init_connect='SET NAMES utf8'
```
  * 查看临时密码：`grep 'A temporary password' /var/log/mysqld.log`
  * 用临时密码登录`mysql`：`mysql -uroot -p`
  * 修改`root`密码：`set password for 'root'@'localhost'=password('xxxxxxxx');`**注意：mysql5.7默认安装了密码安全检查插件（validate_password），默认密码检查策略要求密码必须包含：大小写字母、数字和特殊符号，并且长度不能少于8位。否则会提示`ERROR 1819 (HY000): Your password does not satisfy the current policy requirements`错误。**
  * **（可以不执行此命令）**远程访问root权限：`grant all privileges on *.* to 'root'@'%' identified by 'pwd' with grant option;`
  * 创建MySQL新用户：`create user userName identified by 'password';`
  * 授权远程访问user用户【将userDb数据库的所有操作权限都授权给了用户user】：
```
grant all privileges on userNameDb.* to userName@'%' identified by 'pwd';
flush privileges;
```
  * 修改用户密码：
```
update mysql.user set password = password('pwd') where user = 'userName' and host = '%';
flush privileges;
```
  * 删除用户：`drop user userName@'%';`
  * （阿里云开放端口）添加安全组：【控制台->云服务器->点击实例->本实例安全组->配置规则->克隆/添加安全组规则】

## 2 Git
安装命令
```
yum install git
```

## 3 redis
### 3.1 安装依赖库
```
yum install gcc make
```

### 3.2 下载、解压缩、编译安装
```
curl http://download.redis.io/releases/redis-x.x.x.tar.gz -o redis-x.x.x.tar.gz
tar zxvf redis-x.x.x.tar.gz
cd redis-x.x.x
make
cd src
make install
```

### 3.3 设置为自启动，开机自启
```
# 运行安装服务工具，根据提示设置
# 直接自动帮你安装服务，还可以自定义安装参数。全部默认的话，就会自动产生上述的环境文件和服务文件，且已经打开了服务。 默认的服务名为redis_6379
./utils/install-server.sh
# 启动
systemctl start redis_6379
# 开机启动
systemctl enable redis_6379
# 查看状态
systemctl status redis_6379
```

## 4 httpd
安装命令
```
yum install httpd
```
启动服务
```
systemctl start httpd.service
```
停止服务
```
systemctl stop httpd.service
```
重启服务
```
systemctl restart httpd.service
```
设置apache开机启动
```
systemctl enable httpd.service
```

## 5 安装python3
### 安装相关包
```
yum install zlib-devel bzip2-devel openssl-devel ncurses-devel sqlite-devel readline-devel tk-devel gcc make

yum install libffi-devel -y
```
### 下载安装命令
```
wget https://www.python.org/ftp/python/版本号/文件名.tgz
tar zxvf 文件名.tgz
cd 文件名
./configure
make
make install
```

如果安装过程中出现`zipimport.ZipImportError: can't decompress data; zlib not available`报错，请先安装`zlib`。
```
yum install zlib-devel
```

## 6 安装nodeJs
下载及安装
```
curl http://nodejs.org/dist/vx.x.x/node-vx.x.x.tar.gz  -o node-vx.x.x.tar.gz
tar zxvf node-vx.x.x.tar.gz
cd node-vx.x.x
./configure --prefix=/usr/local/node
make
make install
```

生成软连接
```
ln -s /usr/local/node/bin/* /usr/sbin/
```

检测是否安装成功
```
node -v
```

## 7 安装npm
```
curl -L https://npmjs.org/install.sh | sh
```

### 检测是否安装成功
```
npm -v
```

### 切换镜像
```
// 查看当前镜像
npm get registry

// 设成淘宝镜像
npm config set registry http://registry.npm.taobao.org/
```

## 8 安装gitbook
安装命令
```
npm install -g gitbook
npm install -g gitbook-cli
ln -s /usr/local/node/bin/* /usr/sbin/
gitbook -V
```
  * 如若遇到一直显示`Installing GitBook x.x.x`，请耐心等候安装。


## 9 nginx
### 安装命令
```
curl http://nginx.org/download/nginx-x.x.x.tar.gz -o nginx-x.x.x.tar.gz

tar zxvf nginx-x.x.x.tar.gz

cd nginx-x.x.x
make
make install
```

### 配置Nginx
#### 更新Nginx配置
注意不在原来的所解压的目录配置`nginx.conf`，而应该在`/usr/local/nginx/`中。
```
// 修改80端口监听
server {
  listen       80;
  server_name  www.xxx.com;
  rewrite ^(.*)$ https://$server_name$1 permanent;
}

// 修改443端口监听
server {
  listen       443 ssl;
  server_name  www.xxx.com;

  ssl_certificate      /data/lib/xxx/Nginx/1_xxx_bundle.crt;
  ssl_certificate_key  /data/lib/xxx/Nginx/2_xxx.key;

  ssl_session_cache    shared:SSL:1m;
  ssl_session_timeout  5m;

  ssl_ciphers  HIGH:!aNULL:!MD5;
  ssl_prefer_server_ciphers  on;

  location / {
      proxy_pass http://x.x.x.x:8000;
  }

  location /resource/ {
      root html/resource;
  }
}
```

### 操作命令
```
// 更新配置
./configure --prefix=/usr/local/nginx --with-http_stub_status_module --with-http_ssl_module

// 编译安装
make && make install

// 生成软连接
ln -s /usr/local/nginx/sbin/* /usr/sbin/

// 测试
nginx -t

// 重启服务
service nginx restart

// 关闭Nginx
nginx -s stop  // 快速停止nginx
nginx -s quit  // 正常停止nginx

// 修改配置后重新加载生效
nginx -s reload
```