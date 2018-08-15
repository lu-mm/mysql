wget http://tar.27aichi.cn/mysql/cmake-3.9.3.tar.gz
tar zxf cmake-3.9.3.tar.gz && cd cmake-3.9.3
./configure
make && make install
../
 
wget http://tar.27aichi.cn/mysql/mysql-boost-5.7.19.tar.gz
tar zxf mysql-boost-5.7.19.tar.gz && cd mysql-5.7.19
 
cmake \
-DCMAKE_INSTALL_PREFIX=/data/soft/mysql-5.7.19 \
-DMYSQL_DATADIR=/data/mysql-5.7.19/ \
-DSYSTEMD_PID_DIR=/data/mysql-5.7.19/ \
-DSYSCONFDIR=/data/soft/mysql-5.7.19/etc/ \
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
 
/usr/sbin/groupadd mysql
/usr/sbin/useradd -g mysql mysql -s /sbin/nologin
 
# 增加mysql 路径到 /etc/profile 目录 (功能是增加环境变量)
echo "PATH=\$PATH:/data/soft/mysql-5.7.19/bin" >> /etc/profile
 
# 增加mysql的lib到环境变量
echo "/data/soft/mysql-5.7.19/lib/" >>  /etc/ld.so.conf.d/mysql_lib.conf && /sbin/ldconfig
 
# 重读 profile 让环境变量立刻生效
. /etc/profile
rm -f /etc/my.cnf
mkdir -p /data/soft/mysql-5.7.19/etc/
 
``` 
wget http://tar.27aichi.cn/mysql/my.cnf
cp my.cnf /data/soft/mysql-5.7.19/etc/
````
##初始化数据库
```
/data/soft/mysql-5.7.19/bin/mysqld --initialize-insecure --user=mysql --basedir=/data/soft/mysql-5.7.19/ --datadir=/data/mysql-5.7.19/
 ```
cp /data/soft/mysql-5.7.19/usr/lib/systemd/system/mysqld.service /usr/lib/systemd/system/mysqld.service
systemctl start mysqld
 
# 修改mysql root
```
mysqladmin -u root password 'PASSWORD'
 ```
# 清理MYSQL默认数据 清除主机名 清除主机默认IP地址 清除权限
 
 
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
#增加管理用户
GRANT ALL PRIVILEGES ON *.* TO YOUR_USER@'%' IDENTIFIED BY 'PASSWORD' WITH GRANT OPTION;
 
 
systemctl enable mysqld





## Welcome to GitHub Pages

You can use the [editor on GitHub](https://github.com/lu-mm/mysql/edit/master/README.md) to maintain and preview the content for your website in Markdown files.

Whenever you commit to this repository, GitHub Pages will run [Jekyll](https://jekyllrb.com/) to rebuild the pages in your site, from the content in your Markdown files.

### Markdown

Markdown is a lightweight and easy-to-use syntax for styling your writing. It includes conventions for

```markdown
Syntax highlighted code block

# Header 1
## Header 2
### Header 3

- Bulleted
- List

1. Numbered
2. List

**Bold** and _Italic_ and `Code` text

[Link](url) and ![Image](src)
```

For more details see [GitHub Flavored Markdown](https://guides.github.com/features/mastering-markdown/).

### Jekyll Themes

Your Pages site will use the layout and styles from the Jekyll theme you have selected in your [repository settings](https://github.com/lu-mm/mysql/settings). The name of this theme is saved in the Jekyll `_config.yml` configuration file.

### Support or Contact

Having trouble with Pages? Check out our [documentation](https://help.github.com/categories/github-pages-basics/) or [contact support](https://github.com/contact) and we’ll help you sort it out.
