# 相关配置的修改

----
## 1. MySql数据库
设置root密码  
```
mysqladmin -u root -p password "xxxxxxx"
```

### 1.1 用户名和密码
新增用户【先登录root用户】
```
// grant 权限1,权限2, ... 权限n on 数据库名称.表名称 to 用户名@用户地址 identified by '连接口令';
grant all on *.* to userName@"%" identified b "123";
flush privileges; // 刷新系统权限表
```
```
insert into mysql.user(Host,User,Password) values("localhost","username",password("xxx"));
flush privileges;
// 添加远程访问权限
grant all on *.* to username@localhost identified by 'xxxx';
flush privileges;
```
删除用户
```
delete from mysql.user where user = "yushan";
flush privileges; // 刷新系统权限表
```
更新用户密码
```
use mysql;
UPDATE user SET Password=PASSWORD('xxx') WHERE user='xxx';
flush privileges;
/q
```

### 1.2 导入数据库
#### 1.2.1 导入PyToolsIP数据库
  * 使用resource仓库下的最新数据库文件；
  * 使用`navicat for MySQL`软件进行数据导入。

## 2. Redis数据库
### 1.1 密码
配置修改【改完后要重启】
```
注释及修改redis.conf中的requirepass
```
命令行修改
```
redis-cli; // redis-cli -p 6379 -a 密码
config set requirepass 密码; // 设置密码
config get requirepass; // 获取密码
```

## 3. PyToolsIP
### 3.1 website配置
  * 同步mysql的用户名和密码
  * 同步redis的密码

### 3.2 server配置
  * 更新IP和端口号
  * 同步mysql的用户名和密码
  * 同步redis的密码

### 3.3 client配置
  * 更新IP和端口号

## 4. Httpd配置
  * Listen的端口号

### 4.1 重定向pytoolsip
```
Include conf.modules.d/*.conf
LoadModule rewrite_module modules/mod_rewrite.so
RewriteEngine on
RewriteRule ^/pytoolsip/*$ http://xxx.xxx.xxx.xxx:xxx [L,P]
RewriteRule ^/pytoolsip/detail/*$ http://xxx.xxx.xxx.xxx:xxx/detail [L,P]
RewriteRule ^/pytoolsip/userinfo/*$ http://xxx.xxx.xxx.xxx:xxx/userinfo [L,P]
RewriteRule ^/pytoolsip/release/*$ http://xxx.xxx.xxx.xxx:xxx/release [L,P]
RewriteRule ^/pytoolsip/reqinfo/*$ http://xxx.xxx.xxx.xxx:xxx/reqinfo [L,P]
RewriteRule ^/pytoolsip/toollist/*$ http://xxx.xxx.xxx.xxx:xxx/toollist [L,P]
RewriteRule ^/pytoolsip/static/(.*)$ http://xxx.xxx.xxx.xxx:xxx/pytoolsip/static/$1 [L,P]
RewriteRule ^/pytoolsip/media/(.*)$ http://xxx.xxx.xxx.xxx:xxx/pytoolsip/media/$1 [L,P]
```
