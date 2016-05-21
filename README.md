# Android-development-experience
Some experience about Android-development
关于button控件的设置
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
关于如何点击button切换activity
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
如何通过点击改变button的字体颜色
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
如何设计一个login activity
--
* 方法1：
new-other-activity-login activity
* 方法2:
建立3个activiy， mainactivity、firstactivity、thirdactivity
在mainactivity的layout中布置两个edittext 以及一个button
mainactivity代码如下;
```java
public class MainActivity extends Activity {
        private EditText username ; 
          //密码文本编辑框 
         private EditText password ; 
            //登录按钮 
           private Button login ; 
           //定义Intent对象,用来连接两个Activity 
          private Intent intent ; 
        protected void onCreate(Bundle savedInstanceState) {
               super.onCreate(savedInstanceState);
              setContentView(R.layout. activity_main);
                login = (Button)findViewById(R.id. login); }

public void auto(View arg0)
       {
               username = (EditText)findViewById(R.id. username); 
                        password = (EditText)findViewById(R.id. password); 
                  //判断用户输入的用户名和密码是否与设置的值相同,必须要有toString() 
                 if("admin" .equals(username .getText().toString())&&  
                                 "123456".equals(password .getText().toString())){ 
                             System. out.println("你点击了按钮" ); 
                             //创建Intent对象，传入源Activity和目的Activity的类对象 
                      intent = new Intent(MainActivity.this, FirstActivity.class); 
                             //启动Activity 
                             startActivity( intent); 
                        }
                 else if ("user" .equals(username .getText().toString())&&  
                                 "123".equals(password .getText().toString()))
                        {
                       System. out.println("你点击了按钮" ); 
                      //创建Intent对象，传入源Activity和目的Activity的类对象 
              intent = new Intent(MainActivity.this,ThirdActivity.class); 
                      //启动Activity 
                      startActivity( intent); 
                             
                        }
                 else
                 {
                       Toast. makeText(MainActivity.this, "用户登录信息错误" , Toast.LENGTH_SHORT).show();
                 }
                             //登录信息错误，通过Toast显示提示信息 
                             /* Toast.makeText(AndroidLoginActivity.this,"用户登录信息错误" , Toast.LENGTH_SHORT).show();  */
                         
       } 
}
```
其中button的onClick的点击事件叫“auto”

此外如何使输入的密码不显示，只以原点的形式显示，在edittext的layout里面写入
```xml
android:inputType="textPassword"
```
如何切换首先出现的activity？
--
将下面这段代码剪切再粘贴到想要目标activity的</activity>之上即可
```xml
<intent-filter>
                <action android:name="android.intent.action.MAIN" />
                <category android:name="android.intent.category.LAUNCHER" />
            </intent-filter>
```
如果这段代码在不同的activity中都存在，那么当安装程序的时候会安装多个应用
如何做一个封面（splash）
--
新建一个splashactivity，将acvtivity.java中没用的都注释掉,将其改成
```java
public class SplashActivity extends ActionBarActivity {
     //新加的
     private final int SPLASH_DISPLAY_LENGHT=2000;

     @Override
     protected void onCreate(Bundle savedInstanceState)
{
          super.onCreate(savedInstanceState)；
          requestWindowFeature(Window.FEATURE_NO_TITLE);//1
              setContentView(R.layout. activity_splash);//2
              getWindow().setFlags(WindowManager.LayoutParams. FLAG_FULLSCREEN, WindowManager.LayoutParams.FLAG_FULLSCREEN );            //3
          new Handler().postDelayed(new Runnable()
          {
               @Override
               public void run()
               {
                    Intent mainIntent = new Intent(SplashActivity.this,MainActivity.class);
                   
                    SplashActivity.this.startActivity(mainIntent);
                    SplashActivity.this.finish();
               }
          },SPLASH_DISPLAY_LENGHT);
}
}
```
其中2000表示2秒（不过实际中没有用，至少10000才有用）。然后在manifest.html将程序刚开始的入口设为splashactivity
注意2句要位于1之后，否则会报错。  然后在layout中的activity_splash.xml中设置背景图片。
思想就是先进入一个全屏activity，延迟几秒后进入下一个activity
如何在电脑建立服务器端，手机作为客户端，手机端接收电脑发送的数据
--
  1.在ADT建立一个JAVA工程，然后输入以下代码
  ```java
import java.io.IOException;
import java.io.OutputStream;
import java.net.ServerSocket;
import java.net.Socket;
public class SocketTest {
   public static void main(String args[])
   throws IOException
   {
          ServerSocket ss=new ServerSocket(30001);
          while(true )
          {
                Socket s=ss.accept();
                OutputStream os=s.getOutputStream();
                os.write( "Iwanttomarry\n".getBytes("utf-8" ));
                os.close();
                s.close();
          }
         
   }

}
```
然后建立安卓工程，输入以下代码：
```java
public class MainActivity extends Activity {
       EditText show;
        @Override
        protected void onCreate(Bundle savedInstanceState) {
               super.onCreate(savedInstanceState);
              setContentView(R.layout. activity_main);
               show=(EditText)findViewById(R.id. show);
               new Thread()
              {
                      public void run()
                     {
                            try{
                           /*注意这里的IP是电脑私有地址*/
                                  Socket socket= new Socket("192.168.1.104" ,30001);
                                  BufferedReader br= new BufferedReader(new InputStreamReader(socket.getInputStream()));
                                  String line=br.readLine();
                                   show.setText( "来自服务器的数据：" +line);
                                  br.close();
                                  socket.close();
                           }
                      catch (IOException e)
                           {
                                  e.printStackTrace();//这句话表示
在命令行打印异常信息在程序中出错的位置及原因
                           }
                     }
                     
              }.start();
              
                  }
}
```
然后在activity的layout里面放置一个edittext，
然后在manifest中设置入网权限，
```xml
<uses-permission android:name ="android.permission.INTERNET"/>
```
先运行java程序，再运行android程序即可。
