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

### commande

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









