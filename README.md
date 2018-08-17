### 环境建议（此文档使用了此环境）：
centos: 7.4

mysql: 5.7

cmake: 3.9.3

### 准备条件
mkdir -p /opt/soft

/usr/sbin/groupadd mysql

/usr/sbin/useradd -g mysql mysql -s /sbin/nologin

### 安装cmake
wget http://tar.27aichi.cn/mysql/cmake-3.9.3.tar.gz

tar zxf cmake-3.9.3.tar.gz && cd cmake-3.9.3

./configure

make && make install

cd ../



### 安装mysql
wget http://tar.27aichi.cn/mysql/mysql-boost-5.7.19.tar.gz
```
tar zxf mysql-boost-5.7.19.tar.gz && cd mysql-5.7.19
cmake \
-DCMAKE_INSTALL_PREFIX=/opt/soft/mysql-5.7.19 \
-DMYSQL_DATADIR=/opt/mysql-5.7.19/ \
-DSYSTEMD_PID_DIR=/opt/mysql-5.7.19/ \
-DSYSCONFDIR=/opt/soft/mysql-5.7.19/etc/ \
-DWITH_BOOST=./boost \
-DWITH_INNOBASE_STORAGE_ENGINE=1 \
-DWITH_MYISAM_STORAGE_ENGINE=1 \
-DWITH_PARTITION_STORAGE_ENGINE=1 \
-DWITH_PERFSCHEMA_STORAGE_ENGINE=1 \
-DWITH_ARCHIVE_STORAGE_ENGINE=1 \
-DWITH_BLACKHOLE_STORAGE_ENGINE=1 \
-DWITH_EXTRA_CHARSETS=complex \
-DENABLED_LOCAL_INFILE=1 \
-DDEFAULT_CHARSET=utf8 \
-DDEFAULT_COLLATION=utf8_unicode_ci \
-DWITH_SYSTEMD=1 \
-DWITH_DEBUG=0
make && make install
 ```

 
### 增加mysql 路径到 /etc/profile 目录 (功能是增加环境变量)
echo "PATH=\$PATH:/opt/soft/mysql-5.7.19/bin" >> /etc/profile
 
### 增加mysql的lib到环境变量
echo "/opt/soft/mysql-5.7.19/lib/" >>  /etc/ld.so.conf.d/mysql_lib.conf && /sbin/ldconfig
 
### 重读 profile 让环境变量立刻生效
. /etc/profile
rm -f /etc/my.cnf
mkdir -p /opt/soft/mysql-5.7.19/etc/
 
``` 
wget http://tar.27aichi.cn/mysql/my.cnf
cp my.cnf /opt/soft/mysql-5.7.19/etc/
````
### 初始化数据库
```
/opt/soft/mysql-5.7.19/bin/mysqld --initialize-insecure --user=mysql --basedir=/opt/soft/mysql-5.7.19/ --datadir=/opt/mysql-5.7.19/
 ```
### 启动数据库
```
cp /opt/soft/mysql-5.7.19/usr/lib/systemd/system/mysqld.service /usr/lib/systemd/system/mysqld.service
systemctl start mysqld
```
 
### 修改mysql root
```
mysqladmin -u root password 'PASSWORD'
 ```
### 清理MYSQL默认数据 清除主机名 清除主机默认IP地址 清除权限
 
 
##查看默认账户，
```
mysql -uroot -p
select Host,User,authentication_string from mysql.user;
```
 
##删除多余账户
```
drop user 'mysql.sys'@localhost;
drop user 'mysql.session'@localhost;
 ```
##增加管理用户
GRANT ALL PRIVILEGES ON *.* TO YOUR_USER@'%' IDENTIFIED BY 'PASSWORD' WITH GRANT OPTION;
 
 ### 数据库开机启动
systemctl enable mysqld
