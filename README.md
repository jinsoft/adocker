```

docker-compose up -d redis-master redis-slave-1 redis-slave-2

docker-compose up -d redis-sentinel-1 redis-sentinel-2 redis-sentinel-3
```


`sentinel.conf` 中的使用 `sentinel monitor mymaster redis-master 6379 2` 会有一个错误

```
redis-sentinel-1_1  | 
redis-sentinel-1_1  | *** FATAL CONFIG FILE ERROR (Redis 6.2.1) ***
redis-sentinel-1_1  | Reading the configuration file, at line 8
redis-sentinel-1_1  | >>> 'sentinel monitor mymaster redis-master 6379 2'
redis-sentinel-1_1  | Can't resolve instance hostname.
```

改成 `sentinel monitor mymaster 127.0.0.1 6379 2` 后可以正常使用，暂时不知道为啥 