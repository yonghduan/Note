# mysql8安装后修改root密码

mysql15.7.9之后就没有password函数了，所以使用传统的password函数修改密码会提示错误，正确的修改密码的方法是：

```
ALTER USER 'root'@'localhost' IDENTIFIED WITH mysql_native_password BY '123456';
```

