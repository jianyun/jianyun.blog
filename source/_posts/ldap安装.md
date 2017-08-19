title: ldap安装
date: 2017-08-18 23:26:59
category: 笔记
tags: ldap
---

### 源码安装

db-6.2.32.tar.gz

http://www.oracle.com/technetwork/database/database-technologies/berkeleydb/downloads/index.html

openldap-2.1.29

http://www.openldap.org/software/download/

openldap需要Berkeley DB来存放数据，所以需先安装Berkeley DB

```
# tar -zxvf db-6.2.32.tar.gz
解完压后，会生成一个db-db-6.2.32目录,进行该目录下的build_unix目录。执行以下命令进行配置安装。

# ../dist/configure
# make
# make install
```


