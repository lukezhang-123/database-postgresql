# 在windows使用命令行编译安装PostgreSQL9.2，Visual Studio Community 2022

### 编译环境
系统：Windows 11 23H2

工具：Microsoft Visual Studio Community 2022 (64 位)

### windows编译

# 获取 PostgreSQL 9.2

### download source code
[postgresql-9.2.24.tar.gz](https://ftp.postgresql.org/pub/source/v9.2.24/postgresql-9.2.24.tar.gz)

### Guide: install-windows from source 
[https://www.postgresql.org/docs/9.2/install-windows-full.html](https://www.postgresql.org/docs/9.2/install-windows-full.html)

### 下载 ActiveState perl
https://www.activestate.com/products/perl/

官网不好下载

http://perl.jss.hu/Download/ActivePerl-5.8.8.822-MSWin32-x86-280952.msi

ActivePerl-5.8.8.822-MSWin32-x86-280952.msi

ActivePerl-5.10.0.1005-MSWin32-x86-290470.msi

### 其他可选组件
vs gui 编译，默认是debug模式，需要配置bison和flex

#### bison win
https://gnuwin32.sourceforge.net/packages/bison.htm

http://downloads.sourceforge.net/gnuwin32/bison-2.4.1-bin.zip

#### flex win (版本太低)，需要 >=2.5.31

https://gnuwin32.sourceforge.net/packages/flex.htm

https://gnuwin32.sourceforge.net/downlinks/flex-bin-zip.php

#### winflexbison

https://sourceforge.net/projects/winflexbison/

### 编译1
命令行编译

cd src\tools\msvc

先运行一次，生成工程文件

build

![](../README/2023-11-21-09-38-55.png)

报错，需要打开sln升级解决方案工程文件，菜单栏--项目，重定解决方案目标

修改tools\msvc\VSObjectFactory.pm#L87 加 return '11.00';

修改tools\msvc\build.pl#L37  my $vcver = '11.00';

修改port\chklocale.c#L214  看git提交

src\include\port.h#L258

build

install D:\tmp\pgdata2

### 编译2
进入源码跟目录，就是src目录的上层，执行编译补丁

git apply --ignore-space-change --ignore-whitespace  ..\postgresql-9.2.24-11-24-32.patch

cd src\tools\msvc

build

install D:\tmp\pgdata2

### 安装后运行，验证
initdb.exe -L D:\tmp\pgdata\share -D D:\tmp\pgdata\data -E UTF-8 --locale=chs

psql.exe -d postgres
\l+

![](../README/2023-11-21-12-22-26.png)


### 其他问题记录

cd /d D:\tmp\postgresql-9.2.24\src\tools\msvc
perl mkvcbuild.pl

因为pg9.2是早期产品，需要修改里面检查vs版本的函数
tools\msvc\VSObjectFactory.pm#L87
return '11.00';


![](../README/2023-11-20-17-48-06.png)

![](../README/2023-11-20-17-51-03.png)

![](../README/2023-11-20-17-49-18.png)



bin/psql依赖misc/libpgport, libpg,项目都有配置的

在initdb.c开始记录了需要的文件，2926-2936行

编译过程自动先从.y使用bison生成.c
src\backend\bootstrap\bootparse.y
src\backend\bootstrap\bootscanner.l
src\backend\parser\gram.y
src\backend\parser\scan.l
src\backend\replication\repl_gram.y
src\backend\utils\misc\guc-file.l

src\backend\bootstrap\bootparse.y
src\backend\parser\gram.y
src\pl\plpgsql\src\gram.y
src\interfaces\ecpg\preproc\preproc.y
src\backend\replication\repl_gram.y
src\test\isolation\specparse.y
src\backend\utils\misc\guc-file.l

报错
error LNK2001: 无法解析的外部符号 pg_finfo_utf8_to_win
error LNK2001: 无法解析的外部符号 pg_finfo_win_to_utf8
error LNK2001: 无法解析的外部符号 utf8_to_win
error LNK2001: 无法解析的外部符号 win_to_utf8

查询导出函数
https://coverage.postgresql.org/src/backend/utils/mb/conversion_procs/utf8_and_win/utf8_and_win.c.func-sort-c.html
需要在项目的连接器，输入，附加依赖项 Debug\utf8_and_win\utf8_and_win.lib;


