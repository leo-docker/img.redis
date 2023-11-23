### 修改
[TOC]

#### 启动指令
```shell
docker run -it --rm \
  --name redis \
  -p 6379:6379 \
  maleo/redis:6.0.10
```

#### 以Service建立新的 Redis Service
1. 執行 redis-src 下，utils/install_server.sh
2. 選擇port就可以了
  - 会建立对应的配置档案 [port].conf
3. 指令：
  - service redis_[port] stop|start|restart
  - 列出所有的 server ： service --status-all

#### 容器后续配置
1. 允许远端连接（关闭protected-mode）修改安装好servie 的 配置档案 [port].conf
   ```conf
   ...
   #bind 127.0.0.1
   ...
   protected-mode no
   ...
   ```
2. redis的数据存于内存，所以关闭容器后数据会消失
  - 决定数据持久化的策略，修改配置档案 [port].conf
    - aof ：
      - "appendonly yes"
      - "appendfsync always|everysec|no" #每次变更|每秒同步|由操作系统执行，默认Linux配置最多丢失30秒
    - 混合式：
      - "aof-use-rdb-preamble yes"
3. 哨兵模式
  - 主从复制 + 哨兵程序
    - 修改配置档案 [port].conf
      - "slaveof [master-ip] [master-port]"
      - "masterauth [master-password]"
