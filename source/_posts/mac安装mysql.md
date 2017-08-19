title: mac mysql安装
date: 2017-08-18 23:26:59
category: 笔记
tags: ldap
---

### 版本


在 Mac 系统上, 安装 MySQL Server 一般是用 DMG 包在图形化界面安装,
此外 mysql 还提供了 Compressed TAR Archive 二进制包安装方式, 即免安装解压运行版, 相比 DMG 包, 免安装版过程更为简洁, 纯命令行操作。


```
mysql-5.6.37-macos10.12-x86_64.tar.gz
https://dev.mysql.com/downloads/file/?id=471193

mac os 10.12.5
```

```
解压 tar.gz 文件:
tar zxvf mysql-5.6.37-macos10.12-x86_64.tar.gz


# 移动解压后的二进制包到安装目录
sudo mv mysql-5.6.37-macos10.12-x86_64 /usr/local/


# 更改 mysql 安装目录所属用户与用户组
cd /usr/local
sudo chown -R root:wheel mysql

# 执行 scripts 目录下的 mysql_install_db 脚本完成一些默认的初始化(创建默认配置文件、授权表等)
cd /usr/local/mysql
sudo scripts/mysql_install_db --user=mysql

注意: MySQL 5.7.6 以上版本取消了 scripts 目录, 初始化命令改成了
sudo bin/mysqld --initialize --user=mysql


cd /usr/local/mysql
# 启动
sudo support-files/mysql.server start
# 重启
sudo support-files/mysql.server restart
# 停止
sudo support-files/mysql.server stop
# 检查 MySQL 运行状态
sudo support-files/mysql.server status

初始化 MySQL root 密码
# 需要 MySQL 在运行状态执行
cd /usr/local/mysql/bin
./mysqladmin -u root password <your-password>
通过自带的 MySQL Client 连接数据库
cd /usr/local/mysql/bin
./mysql -u root -p
<your-password>


```