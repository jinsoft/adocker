```
docker-compose up -d 
```

增加一个网站

增加nginx配置  `nginx/sites/xxx.conf`



> 记得修改 mysql 默认对外的端口




## 报错：

````
[ERROR] –initialize specified but the data directory has files in it. Aborting.
[ERROR] Aborting
````

原因： mysql 初始化的时候 data目录不能有文件



```
Primary script unknown
```

原因： 项目的路径权限和 php-fpm的不一致


### 数据库连接失败


```
ip a 


3: docker0: <NO-CARRIER,BROADCAST,MULTICAST,UP> mtu 1500 qdisc noqueue state DOWN group default
    link/ether 02:42:25:d6:cf:2d brd ff:ff:ff:ff:ff:ff
    inet 172.17.0.1/16 brd 172.17.255.255 scope global docker0
       valid_lft forever preferred_lft forever
```

docker0 后的 172.17.0.1 就是我们要的