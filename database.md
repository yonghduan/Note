# 安装mysql的方法

1. 进入mysql官网，下载mysql community 版本
2. 用管理员权限打开cmd，输入命令：

```
mysqld --initialize --console（注意随机生成的暂时密码,会生成data文件夹）
mysqld --install
net start mysql
mysql -u root -p 
```

输入随机生成的密码进入mysql，然后修改密码

```
alter user root@localhost identified by 'root';
```

此句修改密码为root