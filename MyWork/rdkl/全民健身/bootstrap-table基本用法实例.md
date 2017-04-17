
```
<div class="btn-group hidden-xs" id="toolbar" role="group">
        <div class="col-sm-5" style="padding-left: 0px;">
            <input class="form-control" type="text" placeholder="请输入赛事标题" name="match_title"
                   id="match_title">
        </div>
        <div class="col-sm-5">
            <select class="search-select form-control" id="status" name="status">
                <option value="-1">请选择赛事状态</option>
                <option value="0">待提交</option>
                <option value="1">审核中</option>
                <option value="2">审核通过</option>
                <option value="3">审核不通过</option>
            </select>
        </div>
        <div class="col-sm-1">
            <button type="button" class="btn btn-outline btn-default" onclick="matchinfo.search();">
                <i class="glyphicon glyphicon-search" aria-hidden="true"></i>
            </button>
        </div>
    </div>
</div>
<table id="matchinfo_table"></table>

$('#matchinfo_table').bootstrapTable({//matchinfo_table为页面table便签的id
            method: 'post',
            striped: true,
            pagination: true,
            toolbar: '#toolbar',//页面id为toolbar的
            sortable: false,
            sortOrder: "desc",
            pageNumber: 1,
            pageSize: 10,
            pageList: [10, 20, 30],
            url: ctx + "/matchinfo/list.shtml",
            queryParamsType: 'normal',
            queryParams: queryParams,//查询条件
            sidePagination: "server",
            search: false,
            strictSearch: true,
            showColumns: true,
            showRefresh: true,
            minimumCountColumns: 1,
            clickToSelect: true,
            searchOnEnterKey: true,
            silent: true,
            idField: "match_id",
            columns: [{
                field: "state",
                checkbox: true,
                align: 'center'
            }, {
                field: 'match_id',
                title: 'ID',
                align: 'center',
                visible: false
            }, {
                field: 'match_title',
                title: '赛事标题',
                align: 'center'
            }, {
                field: 'match_event',
                title: '赛事项目',
                align: 'center'
            }, {
                field: 'sign_start_time',
                title: '报名开始时间',
                align: 'center'
            }, {
                field: 'sign_end_time',
                title: '报名结束时间',
                align: 'center'
            }, {
                field: 'match_start_time',
                title: '比赛开始时间',
                align: 'center'
            }, {
                field: 'match_end_time',
                title: '比赛结束时间',
                align: 'center'
            }, {
                field: 'status',
                title: '赛事状态',
                align: 'center',
                formatter: function (value, row, index) {
                    if (value == 0) {
                        return "<span class='label label-warning'>待提交</span>";
                    } else if (value == 1) {
                        return "<span class='label label-primary'>审核中</span>";
                    } else if (value == 2) {
                        return "<span class='label label-primary'>审核通过</span>";
                    } else if (value == 3) {
                        return "<span class='label label-primary'>审核不通过</span>";
                    }
                }
            }, {
                field: 'creator_name',
                title: '创建人',
                align: 'center'
            }, {
                field: 'create_date',
                title: '创建时间',
                align: 'center'
            }]
        });
        
        
function queryParams() {
    var params = $('#matchinfo_table').bootstrapTable('getOptions')
    var match_title = $("#match_title").val();
    if (match_title == '请输入赛事标题') {
        match_title = "";
    }
    var match_event = $("#match_event").val();
    var status = $("#status").val();
    var all_params = {
        pageSize: params.pageSize,
        pageNo: params.pageNumber,
        match_title: match_title,
        status: status,
        match_event: match_event,
        node_id: $("#node_id").val()
    }
    return all_params;
}
```

#### 常用方法：
- 刷新： $('#matchinfo_table').bootstrapTable('refresh');
- 获取选择的行：var rows = $('#matchinfo_table').bootstrapTable('getSelections');
- 更多方法用法属性等参考官网：http://bootstrap-table.wenzhixin.net.cn/documentation/
