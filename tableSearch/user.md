# 使用方法

只要三步：

1. 引用js代码：
  
  ```
  <script src="/resource/js/dist/tableSearchPage_v2.js"></script>
  ``` 

2. 在页面给TableSearch组件留个坑，比如下面的id为“container”的div
  ```
  <div id="container"></div>
  ```
  
3. 通过js初始化这个组件，示例：
  ```
  $(function(){
    //app为组件实例的引用，一些实例方法可以通过它来调用，比如app.refresh.
    var app = tableSearchPage.init({
        //组件容器
        container: '#container',
        //数据请求地址
        url: '/community/ajaxGetPostList',
        //默认显示页号
        page: 1,
        //列配置
        columns: [
            {
                //此列显示标题
                header: 'ID',
                //对应数据字段
                dataIndex: 'post_id',
                //此列可排序
                sortable: true,
            },
            'update_time',
            {
                header: '首图',
                dataIndex: '',
                render: function (row){
                    return "<img src='" + row['img_small'] + "' height='30' />";
                },
            },
            {
                header: '作者',
                dataIndex: '',
                //不使用dataIndex中的数据显示，自己拼装内容
                render: function (row){
                    msg = '[uid]:' + row['user_id'];
                    msg += "<br/>";
                    msg += '<a href="'+row['user_id']+'">' + row['user_info']['display_name'] + '</a>';
                    return msg;
                },
            },
            {
                header: '浏览',
                dataIndex: 'pv_cnt',
                sortable: true,
                render: function (row){
                    return row['pv_cnt'];
                },
            },
            {
                header: '点赞',
                dataIndex: 'dig_cnt',
                sortable: true,
                render: function (row){
                    return row['dig_cnt'];
                },
            },
            {
                header: '操作',
                dataIndex: '',
                //显示为按钮
                buttons:[{
                    //按钮上显示的文字
                    text: '全文',
                    //按钮类型(也就是按钮颜色)
                    type: 'info',
                    //点击按钮触发的操作
                    click: function(rowData, columnConfig, evt){
                        var action = '/community/getPostDetail?post_id='+rowData.post_id;
                        location.href = action;
                    }
                }]
            },
        ],
        //搜索表单配置
        searchFilter: {
            //显示为一个输入框
            "post_id":{"label":"ID"}, 
            "topic_id":{"label":"话题ID"}, 
            //显示为一个下拉列表，默认选择default_value中指定项
            "is_del":{
                default_value: '-1',
                label: '删除状态',
                list: {
                    '-1': '全部',
                    '0': '正常',
                    '1': '已删除',
                }
            }
        },
        
        //批量操作按钮
        batOp:[{
            text:'删除选中行',
            type: 'danger',
            click: function(rows){
                alert('确认要删除这'+rows.length+'条记录?');
            }
        }],
        //组件支持的事件监听
        listeners:{
            //当某一个单元格被点击时，触发cellclick事件
            'cellclick': function(row, colCfg, evt){
                if(colCfg.dataIndex=="post_id"){
                    $('#container input[name="post_id"]').val(row.post_id);
                    this.refresh();
                }
            }
        }
    });
});
  ```