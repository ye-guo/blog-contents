# Mysql数据库远程登录失败

连接数据库报错：Access denied for user ‘root‘@ ‘xxx‘ (using password: YES)

本地的数据库，其他IP地址的访问都是没有权限的，需要的本机用户给予权限才能访问到。

* 首先登录自己数据库

![](https://cdn.jsdelivr.net/gh/ye-guo/Images/images/%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%9C%E7%A8%8B%E8%BF%9E%E6%8E%A5%E9%94%99%E8%AF%AF2.png)

* 执行授权

![](https://cdn.jsdelivr.net/gh/ye-guo/Images/images/%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%9C%E7%A8%8B%E8%BF%9E%E6%8E%A5%E9%94%99%E8%AF%AF4.png)

```mysql 
GRANT ALL PRIVILEGES ON *.* TO 'root'@'%' IDENTIFIED BY '您的密码' WITH GRANT OPTION;
```

* 重启服务

![](https://cdn.jsdelivr.net/gh/ye-guo/Images/images/%E6%95%B0%E6%8D%AE%E5%BA%93%E8%BF%9C%E7%A8%8B%E8%BF%9E%E6%8E%A5%E9%94%99%E8%AF%AF3.png)

```shell
sudo service mysal restart 
或
sudo systemctl restart mysqld
```

