# Android-development-experience
Some experience about Android-development
1.关于button控件的设置
--
在layout里面main.xml里面写入
```Java
android:text="@string/phone"
```
在string.xml里面定义
```
<string name="phone">phonenumber</string>
```
这样就会在button上显示phonenumber字样。
2.关于如何点击button切换activity
--
 创建两个活动，然后两个活动可以使用不同的layout，在其中一个layout的button的代码中加入android:onClick="nextview"，其中nextview是函数 ，然后在该layout对应的activity中写入
```java
 public void nextview(View view){
           Intent intent=new Intent();
           intent.setClass(this, UserInterface.class);
           startActivity(intent);

}
```
函数nextview的定义，其中UserInterface是第二个activity的名称。
使用这种方法设置监听事件就无需再写
Button btn1=(Button)findViewById(R.id.button1);
3.如何通过点击改变button的字体颜色
--
首先在res文件夹下面新建一个xml文件（new——other-xml）命名为text_selector然后输入
```xml
<selector xmlns:android="http://schemas.android.com/apk/res/android" > 
        <item android:state_selected="true" android:color= "@drawable/white" />  
    <item android:state_pressed= "true" android:color ="@drawable/blue" /> 
    <item android:color= "@drawable/black"/> 
</selector>  
```
然后在value文件夹下面新建一个colors.xml
```xml
<?xml version= "1.0" encoding ="UTF-8"?>
<resources>  
        <drawable name= "dark_gray">#8F8F8F</drawable > 
        <drawable name= "light_gray">#EEEEEE</drawable > 
        <drawable name= "white">#FFFFFF</drawable > 
        <drawable name= "black">#000000</drawable > 
        <drawable name= "blue">#2A00FF</drawable > 
        <drawable name= "green">#f0f0</drawable > 
        <drawable name= "yellow">#FFFEA4</drawable > 
</resources>   
```

然后在layout内button内加入一行  android:textColor="@drawable/text_selector"
