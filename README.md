```
docker-compose up -d 
```

报错：

````
[ERROR] –initialize specified but the data directory has files in it. Aborting.
[ERROR] Aborting
````

原因： mysql 初始化的时候 data目录不能有文件