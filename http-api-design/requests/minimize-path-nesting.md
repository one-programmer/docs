#### 最小化路径嵌套

在具有嵌套父/子资源关系的数据模型中，路径可能会深度嵌套，例如：

```
/orgs/{org_id}/apps/{app_id}/dynos/{dyno_id}
```

通过优先在根路径上定位资源来限制嵌套深度。使用嵌套来指示范围集合。例如，对于上面的 dyno 属于一个应用程序属于一个组织的情况：

```
/orgs/{org_id}
/orgs/{org_id}/apps
/apps/{app_id}
/apps/{app_id}/dynos
/dynos/{dyno_id}
```