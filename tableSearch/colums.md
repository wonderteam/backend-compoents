# 配置参数--colums
__必须__。表格列的配置，为一个数组。数组中的每一项，决定了某一列显示数据及显示方法。各参数如下：
1. header，表头标题；
2. dataIndex，要显示的数据字段名；
3. render，此列内容渲染函数。因为dataIndex指定的数据来自ajax返回，显示的时候想做一些额外的加工，（比如一个图片地址，变成图片html），就可以使用render。它处理完数据，返回想要的html字符串。它接收的参数：
  1. rowData, 当前行的数据
  2. columnConfig, 当前列在columns中的配置 

4. sortable, 当前列是否支持排序，默认为false。sortable为true时，必须配置dataIndex，它会做sort_key，随url一起请求数据；
5. checkbox, 数组。如果有这个配置，此列显示为复选框。如：
  ```
  columns:[
        {
            header: '配件',
            dataIndex: 'addons',
            checkbox: ['shoes', {label:'帽子',value:'hat'},{label:'手套', value:'gloves'}]
        },
        ...
  ]
  ```
  其中，dataIndex对应的数据(为数组)，决定了哪些复选框被选中。如果label和value一样的话，可以缩写成字符串，比如checkbox:[{label:'shoes', value:'shoes'}]，可以写成checkbox:['shoes']
6. radio: 数组，可选。如果有这个配置，此列显示为单选框。如：
  ```
  columns:[
        {
            header: '颜色',
            dataIndex: 'addons',
            radio: ['black', {label:'白色',value:'white'}]
        },
        ...
  ]
  ```
  其中，dataIndex对应的数据，决定了哪个单选框被选中。如果label和value一样的话，可以缩写成字符串，radio:[{label:'black', value:'black'}]，可以写成radio:['black']
7. buttons, 数组。如果有这个配置，此列显示为按钮。 如：
  ```
  columns:[
        {
            header: '操作',
            dataIndex: '',
            buttons:[{
                text: '隐藏',
                type: 'danger',
                cls: 'btn-del',
                visible:function(row)
                {
                    if(row.status != "0")
                        return true;
                    return false;
                },
                click: function(rowData, columnConfig){
                    alert(rowData.comment_id);
                }
            }]
        },
        ...
  ]
  ```
 按钮的参数有：
 
 1. text, 按钮上的文字；
 2. type, 按钮的类型，也是按钮的颜色，可取值的有default\primary\success\info\warning\danger\link，默认值为default
   ![](https://s.wandougongzhu.cn/s/b6/button_e7e4b6.png)
 3. cls, 按钮上附加的样式。多个样式时，用空格隔开。如cls:'t-btn big-btn del-btn'
 4. visible, 决定按钮是否显示，默认为true，显示。有两种配置方式，如果为数组，比如visible:['type', 'big']，就检测此行的数据中，type值是不是'big'，是则显示，否则隐藏；如果为函数，就根据函数的返回值，决定是否显示，true为显示，否则隐藏。
 5. click，点击按钮时触发的函数。它有三个参数：
    * rowData, 按钮所在行的原始数据
    * columnConfig，按钮所在列的column配置
    * evt, 点击事件对象
8. width, 此列宽度。取值如'150px', '30%'
9. style, 此列表头的css样式，如{background:'#eee'}
10. cls, 此列附加的样式，多个样式时，用空格隔开。如cls:'hl strip-line'
