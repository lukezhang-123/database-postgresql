# database-postgresql
PostgreSQL

![PostgreSQL](https://www.postgresql.org/media/img/about/press/elephant.png)
---

### all version list
[PostgreSQL: File Browser--source code tarball](https://www.postgresql.org/ftp/source/)

---

# PostgreSQL 9.2

### download source code
[postgresql-9.2.24.tar.gz](https://ftp.postgresql.org/pub/source/v9.2.24/postgresql-9.2.24.tar.gz)

### install from source Guide
[https://www.postgresql.org/docs/9.2/install-short.html](https://www.postgresql.org/docs/9.2/install-short.html)

### commands

```
# prepare source code
gunzip postgresql-9.2.24.tar.gz
tar xf postgresql-9.2.24.tar
cd postgresql-9.2.24
yum install gcc make readline readline-devel zlib zlib-devel

# compile
./configure
gmake  # 编译5分钟

# install
gmake install  # /usr/local/pgsql/

# post install
adduser postgres
echo 'export PATH=$PATH:/usr/local/pgsql/bin' >> /home/postgres/.bashrc
mkdir /usr/local/pgsql/data
chown postgres /usr/local/pgsql/data
su - postgres
/usr/local/pgsql/bin/initdb -D /usr/local/pgsql/data

# start
setsid /usr/local/pgsql/bin/postgres -D /usr/local/pgsql/data >log-$(date +'%Y%m%d-%H%M%S%3N').log 2>&1 &
/usr/local/pgsql/bin/createdb test
/usr/local/pgsql/bin/psql test
```


![](README/2023-11-20-15-47-03.png)


### 编译环境

系统：CentOS Linux release 7.9.2009
内核版本：3.10.0-1160.71.1.el7.x86_64
gcc版本：4.8.5 20150623
gmake版本：3.82 （make的别名）


https://www.postgresql.org/docs/9.2/install-windows-full.html


https://www.activestate.com/products/perl/
官网不好下载

http://perl.jss.hu/Download/ActivePerl-5.8.8.822-MSWin32-x86-280952.msi
ActivePerl-5.8.8.822-MSWin32-x86-280952.msi
ActivePerl-5.10.0.1005-MSWin32-x86-290470.msi


cd /d D:\tmp\postgresql-9.2.24\src\tools\msvc
perl mkvcbuild.pl

因为pg9.2是早期产品，需要修改里面检查vs版本的函数
tools\msvc\VSObjectFactory.pm#L87
return '11.00';


![](README/2023-11-20-17-48-06.png)

![](README/2023-11-20-17-51-03.png)

![](README/2023-11-20-17-49-18.png)

