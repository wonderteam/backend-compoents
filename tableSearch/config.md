# 配置参数
### container
__必须__。tableSearch组件所在dom元素，可以是dom元素、css选择器、jquery元素，如：

```
container:'.container'
container:document.getElementById('container')
container:$('.container')
```

### url
__必须__。列表请求的ajax接口。请求此url时会带上页号page，如果配置了搜索，也会带上搜索参数。如果排序了，还要加上排序信息。可能的信息有：

1. 搜索参数；
2. page,页号
3. sort_key,排序字段，对应columns中的dataIndex
4. sort_direction，排序方向，取值为'asc'和'desc'

返回数据结构要求如下:
```
{
    "errno": 0,
    "errmsg": "ok",
    "data": {
        //list,数组，列表数据，每一组数据显示为表格一行
        "list": [
            {
                "comment_id": "21",
                "user_id": "23",
                "goods_id": "46",
                "content": " \u5927\u5bb6\u597d\u597d\u65b9\u5f0f\u53d1\u9001\u65b9\u662f\u5426\u6492\u53d1\u751f\u6cd5\u8428\u82ac\u6492\u53d1\u653e",
                "ctime": "2016-01-18 14:13:07",
                "mtime": "2016-01-18 14:13:07",
                "status": "0",
                "goods_name": "\u82b1\u738b Laurier S\u7cfb\u5217\u591c\u7528\u77ac\u54381mm\u62a4\u7ffc\u536b\u751f\u5dfe35cm",
                "goods_count": "5",
                "goods_display": "46(5)",
                "goods_img": "http:\/\/ossimg.wandougongzhu.cn\/b2679fdbdc1c8829a0423cbee77bb436.jpg@200w_200h_1l.jpg",
                "user_img": "<img src=\"\" width=\"50\" heigh=\"50\" >",
                "user_display_name": null
            },
            {
                "comment_id": "20",
                "user_id": "1025",
                "goods_id": "16",
                "content": "afasdfasdfasdfasdfasdfasdfasdf",
                "ctime": "2016-01-15 19:24:00",
                "mtime": "2016-01-15 19:24:00",
                "status": "0",
                "goods_name": "\u4f73\u4e3d\u5b9dsuisai\u9175\u6bcd\u6d01\u9762\u7c89 \u53bb\u9ed1\u5934\u6e05\u6bdb\u5b54",
                "goods_count": "0",
                "goods_display": "16(0)",
                "goods_img": "http:\/\/ossimg.wandougongzhu.cn\/4bb14337d5619e843caf44a05a5f4fc2.jpg@200w_200h_1l.jpg",
                "user_img": "<img src=\"\" width=\"50\" heigh=\"50\" >",
                "user_display_name": "18513400425"
            }
        ],
        //page, number, 当前页号
        "page": 1,
        //page_count, number, 总页数
        "page_count": 5
    }
```

其中list数组，其中每一条数据对应表格的一行，page为当前页号,page_count为总页数。

### columns
__必须__。参数比较多，请查看《配置参数--columns》章节

#### 简写
如果配置项只有header和dataIndex，并且它们值相同，就可以直接一个header的值就行了。比如：
```
[{header:'name', dataIndex:'name'},{header:'性别', dataIndex:'sex'}]
简写为
['name',{header:'性别', dataIndex:'sex'}]
```
#### 到底显示什么内容
是不是发现，一个单元格显示什么内容，有好几个变量都能决定(dataIndex\render\checkbox\radio\buttons),它们几个同时存在时，渲染谁呢？代码里它们是这样的：

```
buttons>checkbox>radio>render>dataIndex
```
所以，同时配置了buttons和checkbox，只会显示button，不会显示checkbox。
![](http://s.wandougongzhu.cn/s/6e/render_c23c6e.png)

### page
初始显示的页号，默认为1.

### searchFilter
配置顶部的搜索表单。目前支持输入框和下拉框两种。如：
```
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
}
```
目前每一个表单项都是以key-value形式配置的，其中key会用作表单项的name，value是个json，它的数据有：

* label，此表单项显示的名字
* list，json数据，如果有此字段，这个表单项显示为下拉框，其中下拉框的取值由list里的配置决定
* default_value，如果是输入框，它就是输入框中默认显示的内容；如果是下拉框，它就决定了哪项被选中。

### batOp
批量操作配置。批量操作显示为左下角的批量操作按钮，它的配置项跟column中的buttons配置是一样的，唯一不同的是，click函数的传入参数：
1. rowsData，数组，选中的所有行的数据；
2. evt，点击事件对象

### listeners
tableSearch组件支持一些事件，想监听这些事件，需要配置listeners, demo请参考上面。它支持的事件有：
* ready, tableSearch组件初化合完成之后触发，此时dom已经可用。注意，container是可用的，但表格中的数据可能是不可用的，因为它是以ajax方式请求的。
* refresh，切换页面、搜索、或调用接口刷新表格时，都会触发这个事件。它有一个参数，就是新页面的数据
* rowclick, 某一行被点击时触发。它有两个参数，一是rowData，此行的数据；二是evt，点击事件对象
* cellclick，某一单元格被点击时触发。它有三个参数，一个rowData，此行的数据；二是columnConfig，此列的配置；三是evt，点击事件对象。
