微信小程序 --- 从A页面传参到B页面设置web-view标签src属性传参异常处理方法




作者：豆i浆 
来源：CSDN 
原文：https://blog.csdn.net/sinat_19327991/article/details/81584968 ;
版权声明：本文为博主原创文章，转载请附上博文链接！ 
一般情况页面跳转参数都是以下几种姿势

参数传递过去接收的处理方法，可以看我的另一篇博文，点击这里跳转

第一种:
  let parameter = 1;
  wx.navigateTo({
   url: '/page/a/a?parameter=' + parameter,
  })


第二种:

 let parameter = {
     propertyA: 1,
     propertyB: 2
 }

 wx.navigateTo({
     url: '/page/a/a?parameter=' + parameter,
 })

第三种：

 let parameter = [
     {
         propertyA: 1,
         propertyB: 2
     }
 ]

 wx.navigateTo({
   url: '/page/a/a?parameter=' + parameter,
  })

或者还有其他方式 不过都是基于拼接字符串的方式，进行参数传递
但以上方式当你从A页面进行跳转倒B页面时传递参数到B页面，在B页面中设置web-view的src属性时你会发现传递过去的参数没了，完全无效，如果web-view的src属性是完全写死的话 直接写死在页面就可以了，但如果是动态传递参数比方一个ID什么的，那肯定还是要传参的，那么既然传不过去，在B页面怎么动态设置web-view的src属性传参？我们可以利用数据存储进行数据储存，然后再动态设置B页面web-view的src属性传参，具体例子如下：

A页面：

//A.js文件

/**
 * A页面跳转方法
 */
 aJumpFun () {
    let url = 'https://blog.csdn.net?userId=' + this.data.userId;
    wx.setStorageSync('url', url);

    //跳转到B页面
    wx.navigateTo({
        url: '/page/B/B'
    })
 }

B页面：

<!-- B.wxml -->

<web-view src="{{ src }}"></web-view>

//B.js

 /**
  * 生命周期函数--监听页面加载
  */
 onLoad: function(options) {
   this.data.src = wx.getStorageSync('url');
   this.setData({
     src: this.data.src
   });
 },
