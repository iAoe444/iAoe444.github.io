---
title: 小程序学习笔记
categories:
- note
tags:
- 小程序
---

> 记录学习小程序期间的一些笔记

## 删除多余JS逻辑

### app.js

比如里面的app.js里面，一定要有app函数，所以可以删掉，然后直接打App，就可以调用出来了

### index.js

类似的一定要有page函数





## 生命周期

### 怎么理解生命周期函数

浏览器的生命周期函数，可以先看这个东西

```html
<!DOCTYPE html>
<html>
<head>
	<meta charset="utf-8" />
	<title></title>
	<script type="text/javascript" src="lib/jquery-1.12.2.js"></script>
	<script type="text/javascript">
		// 浏览器的生命周期函数可以理解为，浏览器在某个阶段自己触发的某个时间
		// 当页面加载完毕后浏览器自己触发后执行
		window.onload=function(){
			// window.onload要加载各个外部文件，包括图片
			console.log("A")
		}
		// 当DOM准备完毕后自动执行，浏览器自己触发
		$(doument).ready(function(){
			console.log("B");
		});
	</script>
	<!-- 结果是BA -->
</head>
<body>

</body>
</html>
```

然后看向微信小程序

```javascript
onlaunch: function(){
    console.log("当小程序初始化完成时")；
}
```

**生命周期函数本质其实就是到某个阶段自己触发的函数**

### 场景值

```javascript
console.log(options);
```

### 小程序里的生命周期加载顺序

全局初始化 -> 全局后台切入前台 -> 首页-监听页面加载-onload-1-> 监听页面显示-onshow->监听页面除此渲染完毕-onready

**现在切到后台**

首页-监听页面隐藏-全局 - > 小程序从前进后

**再次切换前台**

全局后台进前台 -> 首页-监听页面显示-onshow

**切到列表页**

![Snipaste_2018-08-10_10-31-45](小程序需要注意的点/Snipaste_2018-08-10_10-31-45.png)

**切换回首页**

![Snipaste_2018-08-10_10-34-14](小程序需要注意的点/Snipaste_2018-08-10_10-34-14.png)

*为了节省资源，直接卸载页面*

### onload函数

需要从网上请求数据，显示在我们小程序上，那么最好在onload上

### onready函数

显示弹窗时候出现

## WXML数据绑定

### 用法

注意一个用法

```javascript
{{ 1+1 }}//2
```

这个是数据绑定的一个用法，直接进行计算，也可以进行下面这个操作![Snipaste_2018-08-10_10-43-42](小程序需要注意的点/Snipaste_2018-08-10_10-43-42.png)

最后就会在message里出现

### 特别注意

![Snipaste_2018-08-10_10-48-44](小程序需要注意的点/Snipaste_2018-08-10_10-48-44.png)



## 从网上请求数据

### 其他

AJAX向服务器发送请求后取得数据

### 微信小程序

使用微信request函数

```javascript
wx.request({
    //请求地址
    url:"",
    //请求参数
    data:{},
    //请求头
    header:{}.
    //请求方法
    method:"GET",//这里一定要大写
    //数据类型
    dataType："json",
    //成功请求执行的回调函数
    success: function(res) {
    console.log(res);
}.
    //清楚失败执行的回调函数
    //完成执行的回调函数
})
```

初级的写法

![Snipaste_2018-08-10_11-19-09](小程序需要注意的点/Snipaste_2018-08-10_11-19-09.png)

![Snipaste_2018-08-10_11-17-59](小程序需要注意的点/Snipaste_2018-08-10_11-17-59.png)

这里就需要把data给到data里的sliderList:[]里

所以直接在程序里写入this.data=res.data什么的

![Snipaste_2018-08-10_11-21-25](小程序需要注意的点/Snipaste_2018-08-10_11-21-25.png)

但这是不行的，他指向的this是自己，所以使用箭头函数

![Snipaste_2018-08-10_11-23-33](小程序需要注意的点/Snipaste_2018-08-10_11-23-33.png)

但是发现箭头函数实际上是可以的，但是视图那块却没有更新，其实它只是更新了数据，实际的视图并没有更新，所以直接用微信的函数，如下面所示

#### index.js写法

```javascript
data:{
    sliderList:[]
},
```

```javascript
wx.request({
    url:"地址",
    success:(res)=>{
        this.setData({
        	sliderList:res.data;
        });
    },
})
```

#### 实际的使用

```xml
<image src="{{  sliderList[0].image }}"
```

其中sliderList的数据

![Snipaste_2018-08-10_11-30-51](小程序需要注意的点/Snipaste_2018-08-10_11-30-51.png)

#### 改进

之前![Snipaste_2018-08-10_11-33-55](小程序需要注意的点/Snipaste_2018-08-10_11-33-55.png)

之后

**可以看下微信官方教程里的框架的wx:for教程**

![Snipaste_2018-08-10_11-34-14](小程序需要注意的点/Snipaste_2018-08-10_11-34-14.png)

但是会有警告说要加wx:key，用于排序用的

![Snipaste_2018-08-10_11-35-52](小程序需要注意的点/Snipaste_2018-08-10_11-35-52.png)

所以就加呗

![Snipaste_2018-08-10_11-37-05](小程序需要注意的点/Snipaste_2018-08-10_11-37-05.png)

#### 相同的案例2

请求的数据

![Snipaste_2018-08-10_11-46-59](小程序需要注意的点/Snipaste_2018-08-10_11-46-59.png)

![Snipaste_2018-08-10_11-45-48](小程序需要注意的点/Snipaste_2018-08-10_11-45-48.png)

### 校验数据问题

需要到微信公众号的数据那里配置下

设置 -》开发设置 -》服务器域名

错误的时候，重启一下，如果预览不出来，给图片标签那里价格属性 lazy-load

### 遇到请求数据大的时候的问题

**传入参数**
```javascript
wx.request({
    //请求地址
    url:"",
    //请求参数
    data:{
        _limit=10,
        _page=1
    },
    //成功请求执行的回调函数
    success: function(res) {
    console.log(res);
}
})
```

### 基于页面传入参数实现不同页面设置

每个页面可以传入参数，在编译的时候可以知道![Snipaste_2018-08-10_12-19-47](小程序需要注意的点/Snipaste_2018-08-10_12-19-47.png)

于是可以在onload函数中加入这个参数

![Snipaste_2018-08-10_12-20-40](小程序需要注意的点/Snipaste_2018-08-10_12-20-40.png)

然后再实际应用时候的页面参入这个参数

![Snipaste_2018-08-10_12-21-48](小程序需要注意的点/Snipaste_2018-08-10_12-21-48.png)

### 实现加载更多功能

在下拉触底的时间函数中执行相同加载操作，

但是原函数需要修改一下，需要将原来的this.data.concat(res.data)

![Snipaste_2018-08-10_14-35-19](小程序需要注意的点/Snipaste_2018-08-10_14-35-19.png)

那么到底的时候就使用这么一个来获取当前页面的数量

![Snipaste_2018-08-10_14-51-53](小程序需要注意的点/Snipaste_2018-08-10_14-51-53.png)

再修改一下

![Snipaste_2018-08-10_14-56-10](小程序需要注意的点/Snipaste_2018-08-10_14-56-10.png)

最后再修改一下加载的样式，使用wx:if

![Snipaste_2018-08-10_15-01-34](小程序需要注意的点/Snipaste_2018-08-10_15-01-34.png)

### 没有数据怎么办

自己模拟一下

![Snipaste_2018-08-10_15-23-28](小程序需要注意的点/Snipaste_2018-08-10_15-23-28.png)