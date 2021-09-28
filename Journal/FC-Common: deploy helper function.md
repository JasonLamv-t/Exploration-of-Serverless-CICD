# fc-common部署辅助函数

## Key Point

- 辅助函数和普通函数的区别
  - 默认线上是没有的，因此不需要读取线上配置
  - 假设部署过程中发生错误，需要对已经成功的部分进行清理
- 传入的参数
  - serviceConfig
  - functionConfig
  - triggerConfig
  - customDomain

## 逻辑步骤

1. 参数处理
   1. serveiceConfig
      1. 对名称等字段进行非空校验，返回处理后的serviceConfig
   2. cuntionConfig
   3. triggerConfig
   4. customDomain
2. 部署服务
