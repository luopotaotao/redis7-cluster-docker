# 搭建集群
``` shell
# 需在docker-compose.yml所在的文件夹下执行
docker-compose up -d 

# 关联集群中的各个节点，提示之后输入 yes
docker exec -it node-80 redis-cli -p 6380 --cluster create localhost:6380 localhost:6381 localhost:6382 localhost:6383 localhost:6384 localhost:6385 --cluster-replicas 1


```

# 加载lua脚本
``` shell
# 进入node-80容器
docker exec -it node-80 bash

# 加载脚本到集群中
cat /redis_fun.lua | redis-cli -x  --cluster-only-masters --cluster call 127.0.0.1:6380  FUNCTION LOAD REPLACE
# 退出容器
exit
```

# 在各个节点中确认函数已存在
``` shell
# 以node-81为例
docker exec -it node-81 redis-cli -p 6380 FUNCTION LIST
docker exec -it node-81 redis-cli -p 6381 FUNCTION LIST
docker exec -it node-81 redis-cli -p 6382 FUNCTION LIST
docker exec -it node-81 redis-cli -p 6383 FUNCTION LIST
docker exec -it node-81 redis-cli -p 6384 FUNCTION LIST
docker exec -it node-81 redis-cli -p 6385 FUNCTION LIST
```
