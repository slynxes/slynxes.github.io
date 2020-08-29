---
title: "Dockerfile命令"
date: 2020-08-29T18:40:48+08:00
---

# 复制文件
**ADD和COPY的异同：**
COPY 和 ADD都是用来复制当前上下文中的文件或目录到容器中指定的目录下。
他们的功能基本相同，除了以下两点：

1. ADD 除了可以复制当前环境中的文件，还可以复制远程文件（URL路径）到容器中
1. 当src（不包括URL）是压缩文件（.gz, .xz, .bz2）时，ADD自动将压缩文件解压到容器中，而COPY仍然当作文件处理，不解压缩。



### COPY
**语法：**
```bash
COPY [--chown=USER:GROUP] SRC... DEST
COPY [--chown=USER:GROUP] ["SRC",..., "DEST"]
```
> `SRC` : 当前上下文中的文件或目录，当为目录时，不会复制目录，而是复制目录下面的文件或子目录，与常规的Shell中的`cp`不同

> `DST` : 容器中的文件或目录，当SRC为目录或多个文件时，DST必须是目录，若目录不存在，自动创建该目录，最好以/结尾。否则SRC为多个文件时会报错。



示例：
```dockerfile
COPY conf.d /etc/nginx/conf.d/
```


SRC可以使用通配符

- `*`: 任意个任意字符
- `?`: 单个任意字符
```dockerfile
COPY conf.d/main* /etc/nginx/conf.d/
```
### ADD
**语法**
```dockerfile
ADD [--chown=USER:GROUP] SRC... DEST
ADD [--chown=USER:GROUP] ["SRC",..., "DEST"]
```
> SRC: 当前上下文中的文件或目录，当为目录时，不会复制目录，而是复制目录下面的文件或子目录，与常规的Shell中的`cp`不同

> DST: 容器中的文件或目录，当SRC为目录或多个文件时，DST必须是目录，若目录不存在，自动创建该目录，最好以/结尾。否则SRC为多个文件时会报错。



`ADD` 其他功能均与 `COPY` 相同，下面说明不同点：


1. 复制URL定位的远程文件
```dockerfile
ADD https://qr-backup.oss-cn-hangzhou.aliyuncs.com/visual/nginx-master.gz /opt/
```

2. 复制当前上下文中的压缩文件，实现自动在容器中解压缩
```dockerfile
ADD nginx-master.gz /opt/
```
> 进入容器，压缩文件自动解压缩为：/opt/nginx-master/ 目录



