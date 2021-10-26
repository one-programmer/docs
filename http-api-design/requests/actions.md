#### 操作

首选不需要特殊操作的端点配置。在需要操作的情况下，用`actions`前缀清楚地描述它们：

```
/resources/:resource/actions/:action
```

例如停止特定的运行：

```
/runs/{run_id}/actions/stop
```

还应尽量减少对集合的操作。在需要时，他们应该使用顶级操作描述来避免命名空间冲突并清楚地显示操作范围：

```
/actions/:action/resources
```

例如重新启动所有服务器：

```
/actions/restart/servers
```