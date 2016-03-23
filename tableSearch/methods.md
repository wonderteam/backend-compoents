# 实例方法
它支持的实例方法有：

1. refresh。刷新页面。可以指定要刷新到的页面，默认为第一页。使用例子：
  ```
  var app = tableSearchPage.init({
    container: '#container',
    ...
  });
  $('.btn').click(function(){
    app.refresh(4);//按钮点击时，跳转到第4页
  })
  ```
tableSearch实例，其实就是一个Vue实例，如果想进一步了解怎么用，可以访问http://cn.vuejs.org
