# 本地环境资源限制

[本地调试和端云联调， 本地函数计算的执行环境， 最好能做一些相关限制 · Issue #80 · devsapp/fc (github.com)](https://github.com/devsapp/fc/issues/80)

reference：https://help.aliyun.com/document_detail/51907.html
比如有 cpu 和 内存 quota 限制， 线程和进程数目限制等

## 思路：

proxied CA：

- no customer container：init method 生成 docker 参数时根据yaml进行配置
- customer container：需要回去review代码

local start：需要review，实现逻辑应该和proxied差不多

### 规则

[fc/yaml.md at main · devsapp/fc (github.com)](https://github.com/devsapp/fc/blob/main/docs/Others/yaml.md#function字段)

当前规范下yaml，function字段有以下运行资源控制字段：

| 参数名              | 必填  | 类型   | 参数描述           |
| ------------------- | ----- | ------ | ------------------ |
| memorySize          | False | Number | function的内存规格 |
| InstanceConcurrency | False | Number | 单实例多并发       |
| instanceType        | False | String | 实例类型           |

docker对于cpu限制：

| 选项              | 描述                                                    |
| ----------------- | ------------------------------------------------------- |
| --cpuset-cpus=""  | 允许使用的 CPU 集，值可以为 0-3,0,1                     |
| -c,--cpu-shares=0 | CPU 共享权值（相对权重）                                |
| cpu-period=0      | 限制 CPU CFS 的周期，范围从 100ms~1s，即[1000, 1000000] |
| --cpu-quota=0     | 限制 CPU CFS 配额，必须不小于1ms，即 >= 1000            |
| --cpuset-mems=""  | 允许在上执行的内存节点（MEMs），只对 NUMA 系统有效      |

## 参考

[Docker(二十)-Docker容器CPU、memory资源限制 - 圆圆测试日记 - 博客园 (cnblogs.com)](https://www.cnblogs.com/zhuochong/p/9728383.html)

## 过程：

1. ### ![image-20210827124101363](https://tva1.sinaimg.cn/large/008i3skNgy1gtv8uo6uwvj609b021jra02.jpg)

2. ![image-20210827124148869](https://tva1.sinaimg.cn/large/008i3skNgy1gtv8vl6amxj60fc02hwek02.jpg)

3. 
