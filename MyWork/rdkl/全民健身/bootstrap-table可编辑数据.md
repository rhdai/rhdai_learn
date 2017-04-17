## 需求
表格中有排序值，需要更新排序值改变排序
## 实现
1. 使用bootstrap-table的editable插件
2. 点击排序值弹框编辑排序值更新到数据库中
3. 刷新table
## 实现效果
![image](http://note.youdao.com/yws/public/resource/e7cba1273da0dac779a1fdb55f31fa33/xmlnote/E1D2DA85D14B44D7B2715D42A8A71CBF/372)
## 代码实现
 ### 1.页面引入
- css:忘记引入css会出现样式错乱

```
<link rel="stylesheet" href="/js/plugins/bootstrap-table/editable/bootstrap-editable.css">
```

- js：

```
<script src="${ctx}/js/plugins/bootstrap-table/editable/bootstrap-table-editable.js"></script>
<script src="${ctx}/js/plugins/bootstrap-table/editable/bootstrap-editable.js"></script>
```
 ### 2.js代码
 
```
 {
                field: 'orders',
                title: '排序值',
                align: 'center',
                editable: {
                    type: 'text',
                    title: 'orders',
                    validate: function (value) {
                        value = $.trim(value);
                        if (!value) {
                            return '必填';
                        }
                        if (!/^[0-9]*[1-9][0-9]*$/.test(value)) {
                            return '请输入数字！'
                        }
                        var data = $('#matchinfo_table').bootstrapTable('getData'),
                            index = $(this).parents('tr').data('index');
                        var row = data[index];//获取你修改的排序值的行数据
                        $.ajax({
                            url: ctx + "/matchrecomm/update.shtml",
                            type: 'post',
                            data: {
                                orders: value,
                                work_type: 1,
                                object_id:row.match_id
                            },
                            success: function (data) {
                                setTimeout(function () {
                                    matchrecomm.reload();//刷新表格
                                }, 500)
                            }
                        });
                    }
                }
            }
```


    
    