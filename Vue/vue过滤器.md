### vue过滤器



```vue
<el-table-column prop="agent_level" label="代理商级别" sortable>
   <template slot-scope="scope">
     <el-tag :type="scope.row.agent_level | statusFilter">{{scope.row.agent_level | formatStata}}</el-tag>
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
        return statusMap[status]
      },
    },
```

