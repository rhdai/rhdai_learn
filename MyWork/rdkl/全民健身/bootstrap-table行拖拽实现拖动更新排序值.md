## 需求
表格拖动交换位置排序
## 实现
1. 使用bootstrap-table的reorder-rows插件
2. 拖动后更新排序值到数据库中
3. 刷新table
## 代码实现
 ### 1.页面引入
- css:

```
<link href="${ctx}/js/plugins/bootstrap-table/reorder-rows/bootstrap-table-reorder-rows.css" rel="stylesheet">
```

- js：

```
<script src="${ctx}/js/plugins/bootstrap-table/reorder-rows/jquery.tablednd.js"></script>
<script src="${ctx}/js/plugins/bootstrap-table/reorder-rows/bootstrap-table-reorder-rows.js"></script>
```

- jsp：table便签上加上属性：data-use-row-attr-func="true"        data-reorderable-rows="true

```
 <table id="example_table" data-use-row-attr-func="true" data-reorderable-rows="true"></table>
```
 ### 2.js代码(注意：下面函数与bootstap-table的表属性同级)
 
```
 {
             //拖拽完成后的这条数据，并且可以获取这行数据的上一行数据和下一行数据
            onReorderRowsDrop: function (table, row) {
                console.log(table)
                console.log(row)
                console.log($(row).attr("data-index"));//拖动到的索引
                index = $(row).attr("data-index");//赋值给全局变量index

            },
            //当拖拽结束后，整个表格的数据
            onReorderRow: function (newData) {
                console.info(newData[index].match_id);//获取被拖动的行的数据  index-1是拖动后的上一个  index+1是后一个
                /*更新数据库中当前行的排序值，个人大概思路是，
                获取上一个和下一个（z注意当前是第一个和最后一个以及
                分页问题的各种边界值问题）的排序值取中间值
                ，或者获取上一个的排序值+0.000000000001（很小的一个值，
                避免排序值重复）作为当前数据的排序值，将排序值更新到数据库里*/
            },
            columns: [{
                field: "state",
                checkbox: true,
                align: 'center'
            }
            //省略....
            ]
```

**参考：http://www.cnblogs.com/landeanfen/p/5005367.html**

    
    