### 修改
[TOC]
#### 以Service建立新的 Redis Service
1. 執行 redis-src 下，utils/install_server.sh
2. 選擇port就可以了
  - 会建立对应的配置档案 [port].conf
3. 指令：
  - service redis_[port] stop|start|restart
  - 列出所有的 server ： service --status-all

#### 容器后续配置
1. 允许远端连接（关闭protected-mode）修改安装好servie 的 配置档案 [port].conf
  - "#bind 127.0.0.1"
  - "protected-mode no"
2. 开通 ssh service
  - 修改 /etc/ssh/sshd_config
    - "PermitRootLogin yes"
  - 修改 root 密码
    - passwd
  - 启动 ssh service
    - service ssh restart
3. 网路配置
  - Docker的porting
  - 防火墙记得开通对应的port
4. redis的数据存于内存，所以关闭容器后数据会消失
  - 决定数据持久化的策略，修改配置档案 [port].conf
    - aof ：
      - "appendonly yes"
      - "appendfsync always|everysec|no" #每次变更|每秒同步|由操作系统执行，默认Linux配置最多丢失30秒
    - 混合式：
      - "aof-use-rdb-preamble yes"
5. 哨兵模式
  - 主从复制 + 哨兵程序
    - 修改配置档案 [port].conf
      - "slaveof [master-ip] [master-port]"
      - "masterauth [master-password]"