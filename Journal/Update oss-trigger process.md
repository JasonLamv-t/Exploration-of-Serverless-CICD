# OSS触发器支持

[端云联调： 辅助函数如何支持oss 触发器 · Issue #89 · devsapp/fc (github.com)](https://github.com/devsapp/fc/issues/89)

测试：先部署一个带oss触发器的

当其他的具有相同的前缀时会冲突，部署部分使用的是fc-deplot-component，产生冲突后会报错，需要catch住，询问用户是否删除

删除部分的处理逻辑：使用fc-api，由于只支持一级一级查询，因此需要做一个层遍历处理，先查询所有触发器，再匹配发生冲突的触发器并进行删除

