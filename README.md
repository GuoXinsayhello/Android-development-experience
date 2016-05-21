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
在fragment如何实现跳转
--
在oncreateview里面写上即可
```java
  myImage12.setOnClickListener( new View.OnClickListener() {
                       @Override
                       public void onClick(View v) {
                             Intent in= new Intent(getActivity(),JumpActivity1.class );
                             startActivity(in);
                           
                       }
              });      
```
如何做一个匀速旋转的轮子
--
首先写一个animation.xml的动画文件
```xml
<?xml version= "1.0" encoding ="utf-8"?>
<set xmlns:android="http://schemas.android.com/apk/res/android" >
    <rotate
    android:fromDegrees= "0"   android:toDegrees ="360"   android:pivotX="50%"
    android:pivotY= "50%"  android:duration ="4000"
   
    android:repeatCount= "infinite">
   
  </rotate >
    </set >
    
```
然后在activity中onCreate方法里面写入
```java
ImageView wheela=(ImageView)findViewById(R.id. imageView2);
              ImageView wheelb=(ImageView)findViewById(R.id. imageView3);
              Animation animation1=AnimationUtils.loadAnimation(this,R.anim.rotate);
              LinearInterpolator lir = new LinearInterpolator();/*这两句保证匀速旋转,否则是先快后慢*/
               animation1.setInterpolator(lir);
              wheela.startAnimation( animation1);
              wheelb.startAnimation( animation1);
```
如何实现基于UDP的socket的通信
--
```java
private static final String ServerIP = "192.168.173.1" ;
        private static final int ServerPort = 4568;
在按钮点击事件中写入new Thread( new Server()).start(); 
                                       try { 
                                      Thread.sleep(500); 
                                  } catch (InterruptedException e) { 
                                  
                                         } 
                                         new Thread( new Client()).start(); 
其中client和server的类定义如下：
public class Client implements Runnable { 
                     @Override 
                     public void run() { 
                          /*while (start == false) { 
                         }  */
                   try { 
                              Thread.sleep(500); 
                          } catch (InterruptedException e1) { 
                             // TODO Auto-generated catch block 
                             e1.printStackTrace(); 
                         } 
                         try { 
                             InetAddress serverAddr = InetAddress.getByName(ServerIP ); 
                           /*  updatetrack ("Client: Start connecting\n");  */
                             DatagramSocket socket = new DatagramSocket(); 
                             byte[] buf;
                                 buf = ( "abc").getBytes(); 
                            DatagramPacket packet = new DatagramPacket(buf, buf.length, 
                                     serverAddr, ServerPort); 
                            /* updatetrack ("Client: Sending ‘" + new String(buf ) + "’\n");  */
                              socket.send(packet); 
                            /* updatetrack ("Client: Message sent\n"); 
                              updatetrack("Client: Succeed!\n"); */
                          } catch (Exception e) { 
                             /* updatetrack ("Client: Error!\n");  */
                          } 
                      } 
                  } 
                 public class Server implements Runnable { 
                     @Override 
              public void run() { 
                         /*while (start == false) { 
                        }  */
                        try { 
                            InetAddress serverAddr = InetAddress.getByName(ServerIP ); 
                             /*updatetrack ("\nServer: Start connecting\n");  */
                             DatagramSocket socket = new DatagramSocket(ServerPort , 
                                     serverAddr); 
                            byte[] buf = new byte[17]; 
                           DatagramPacket packet = new DatagramPacket(buf, buf.length); 
                           /* updatetrack ("Server: Receiving\n");  */
                              socket.receive(packet); 
                            /* updatetrack ("Server: Message received: ‘" 
                                      + new String(packet.getData()) + "’\n"); 
                             updatetrack("Server: Succeed!\n");  */
                         } catch (Exception e) { 
                           /* updatetrack ("Server: Error!\n");  */
                         } 
                    } 
                } 
```
注意这里client是发送的，server是接收的
activity如何返回数据
--
例如：在Activity A中打开一个Activity B,如果要在Activity B关闭的时候给Activity A传一些数据,
*  1,Activity A就需要用自己特有的startActivityForResult() 的方法打开Activity B ,
*  2,Activity A中需要重写onActivityResult(int requestCode, int resultCode, Intent data)的方法来接受Activity B传回来的数据，
*  3,在Activity B中用setResult(int resultCode,Intent data)的方法为Activity A设置返回的数据
http://blog.csdn.net/yujian_bing/article/details/8476276

如何搜索手机内存卡内的文件
--
```java
public String searchfile(String keyword)
{
       File[] files = new File("/storage/sdcard0/" ).listFiles();
        for (File file : files)
       {
               if(file.getName().indexOf(keyword) >= 0)
//indexOf方法返回 String 对象内第一次出现子字符串的字符位置
              {
                     Toast. makeText(getApplicationContext(), "yes, wehave",Toast.LENGTH_LONG).show();
              }
              
       }
        return keyword;
}
```
只需要把文件名传递到函数的参数中即可
关于线程
--
如果没有新开线程，仅仅是静态改变控件的属性，不用handler也可以，然而如果新开了一个线程，要想在新线程中改变控件，就要用到handler
具体代码如下（实现点击按钮，开启一个新的线程thread，然后在新线程中改变textview的内容：
```java
public class MainActivity extends ActionBarActivity {
	private TextView tx;
	private Button btn;
	@Override
	protected void onCreate(Bundle savedInstanceState) {
		super.onCreate(savedInstanceState);
		setContentView(R.layout.activity_main);
		tx=(TextView)findViewById(R.id.textView1);
		btn=(Button)findViewById(R.id.button1);
	}
	Handler handler = new Handler()
	{
		public void handleMessage(Message msg)
		{
			String str=String.valueOf(msg.obj);
			tx.setText(str);
		}
	};
	public class MyThread implements Runnable
	{
		public void run()
		{
			Message msg=new Message();
		    msg.obj="sds";
			handler.sendMessage(msg);
		}
	}
	public void onClick(View view)
	{
		Thread thread=new Thread(new MyThread());
		thread.start();
	}
}
```
反编译
--
如何反编译一个安卓apk，从apk中得到资源文件以及java代码http://blog.csdn.net/vipzjyno1/article/details/21039349/
如何对自己的apk进行混淆处理，就是即使别人反编译之后得到的代码也很难懂，让proguard.cfg起作用的做法很简单，就是在eclipse自动生成的default.properties文件中加上一句“proguard.config=proguard.cfg”就可以了。proguard是一个Java代码的混淆工具

在不同activity之间传递数据
--
有四种方法
* i）：通过intent    
发送数据的MainActivity:
Intent intent=new Intent(this,receActivity.class);
Intent.putExtra("int_data",123);
startActivity(intent);

接收数据的receActivity:
StringBuilder bid=new StringBulider();
int b=getIntent().getExtras().getInt("int_data");
String str=bid.append(b).toString();

* ii):通过静态（static）变量    
首先在接收数据的receactivity定义静态变量
Public static String str；
textView.setText(str);
然后在发送数据的activity中：
receactivity.str="jdkj";
Intent intent=new Intent(this,receactivity.class);
startActivity(intent）;

*  iii）：剪贴板（clipboard）传递数据
*  iv）：全局对象传递数据
首先新建类MyApp.class，继承自Application  其中有个属性         public String country    ；，然后在manifest.xml中的<Application>标签中写入android：name=".MyApp",然后在发送的transctivity 中写入MyApp app=（MyApp）getApplicationContext();    app.country="fff",    在receActivity中也写入
MyApp app=（MyApp）getApplicationContext();   tx.setText(app.country);

通过触摸输出触摸点坐标
--
```java
public boolean onTouchEvent(MotionEvent even)
	{
		
	if(even.getAction()==MotionEvent.ACTION_DOWN)
	  {
		Log.v("X的坐标", "zuobioa"+even.getX());
	  }
	return super.onTouchEvent(even);
	}
```
关于Jason
--
 首先，当总是出现错误的时候不要立刻换代码，尝试看logcat，看看问题出在什么地方，logcat前面会描述问题的现象，后面会有caused by：XXX一定要看清楚，根据问题修改代码。
 http请求不允许在主线程中，如果出现一般会有
Android之NetworkOnMainThreadException异常
因此要向服务器请求http的时候一定要另外开启一个线程，如果要修改UI还要用到Handler与主线程进行通信。
        第三，在进行网络有关的项目时一定要记得在manifext.xml中开启网络权限。
        第四，json是一种数据格式，有jsonObject与jsonArray两种，前者是对象，用{}包括，后者是数组，用[]表示，数组之中可以包含对象，json格式是key/value对，key与value之间用冒号分隔
一个可用的向服务器请求json的的代码如下：
```JAVA
Public void onClick(View v)
{
new Thread(networkTask).start();
}
Runnable networkTask = new Runnable() {  
		  
	    @Override  
	    public void run() {  
	        // TODO  
	        // 在这里进行 http request.网络请求相关操作  
	    	String strUrl = "http://mbc.nimache.com/mbc/md5";  
	         String strResult = connServerForResult(strUrl);   
	         //获得多个Singer   
	         parseJson(strResult);   
	        /*Message msg = new Message();  
	        handler.sendMessage(msg);  */
	    }  
	}; 
	 private String connServerForResult(String strUrl) {   
	        // HttpGet对象   
	        HttpGet httpRequest = new HttpGet(strUrl);   
	        String strResult = "";   
	        try {   
	            // HttpClient对象   
	            HttpClient httpClient = new DefaultHttpClient();   
	            // 获得HttpResponse对象   
	            HttpResponse httpResponse = httpClient.execute(httpRequest);   
	            if (httpResponse.getStatusLine().getStatusCode() == HttpStatus.SC_OK) {   
	                // 取得返回的数据   
	                strResult = EntityUtils.toString(httpResponse.getEntity());   
	            }   
	        } catch (ClientProtocolException e) {   
	          /*  tx.setText("protocol error");   */
	        	System.out.println("fuck_protocal_error");
	            e.printStackTrace();   
	        } catch (IOException e) {   
	            /*tx.setText("IO error"); */ 
	            System.out.println("fuck_IO_error");
	            e.printStackTrace();   
	        }   
	        return strResult;   
	    }   
	    // 普通Json数据解析   
	    private void parseJson(String strResult) {   
	        try {   
	            JSONObject jsonObj = new JSONObject(strResult);   
	           /* int id = jsonObj.getInt("id");  */ 
	            String name = jsonObj.getString("a");   
	            String gender = jsonObj.getString("b");   
	           /* tx.setText("ID号"+name + ", 姓名：" + name + ",性别：" + gender);   */
	           Log.i("fuck","a"+name+"/n"+"b"+gender);
	        } catch (JSONException e) {   
	            System.out.println("Json parse error");   
	            e.printStackTrace();   
	        }   
	    }  
```
输入回车的方法是"\n",而不是"/n"
  Message-Digest泛指字节串(Message)的Hash变换，就是把一个任意长度的字节串变换成一定长的大整数。注意这里说的是“字节串”而不是“字符串”，因为这种变换只与字节的值有关，与字符集或编码方式无关。
