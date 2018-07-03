### vue过滤器



```vue
<el-table-column prop="agent_level" label="代理商级别" sortable>
   <template slot-scope="scope">
     <el-tag :type="scope.row.agent_level | statusFilter('level')">{{scope.row.agent_level | formatStata}}</el-tag>
   </template>
</el-table-column>
```



```js
    filters: {
      // el-tag类型转换
      statusFilter: function (status) {
        const statusMap = {
          'hq': 'info',
          'province': 'warning',
          'area': 'success'
        }
        return statusMap[status]
      },
      formatStata: function (status) {
        const statusMap = {
          'hq': '总部',
          'province': '省总',
          'area': '地总',
        }
        return statusMap[status] + level
      },
    },
```



### 过滤器串联

过滤器可以串联：

```js
{{ message | filterA | filterB }}
```

`filterA` 被定义为接收单个参数的过滤器函数，表达式 `message` 的值将作为参数传入到函数中。然后继续调用同样被定义为接收单个参数的过滤器函数 `filterB`，将 `filterA` 的结果传递到 `filterB` 中