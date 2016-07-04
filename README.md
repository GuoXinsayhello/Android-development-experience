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
*  1,Activity A就需要用自己特有的startActivityForResult() 的方法打开Activity B 
    比如：两个activity，比如一个信息登记总界面，点击录入个人信息按钮，就会跳到编辑activity，然后输入完个人信息之后就再跳回去。
	首先在主界面输入
```java
	 public void onClick(View v)
	    {
	    	Intent intent=new Intent(this,TestfirstActivity.class);
	    	startActivityForResult(intent,1);
	   	
	    }
	    
		protected void onActivityResult(int requestCode,int resultCode,Intent data1 )
		{
			super.onActivityResult( requestCode, resultCode,  data1 );
			switch(requestCode)
			{
			case 1:
				switch(resultCode)
				{
			          case 2: setTitle(data1.getStringExtra("fuck"));
			          
			          break;
			    }
				
			 break;
			 
			}
			
		}
		```
	然后在信息输入界面输入：
```java
	public void onClick(View v)
		{
			String sb=etx.getText().toString();
			Intent intent=new Intent();
			intent.putExtra("fuck",sb);
			setResult(2,intent);
		    finish();
		}
```
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
	        	System.out.println("protocal_error");
	            e.printStackTrace();   
	        } catch (IOException e) {   
	            /*tx.setText("IO error"); */ 
	            System.out.println("IO_error");
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

fileinputstream()函数
--
public int read(byte[] b) throws IOException
*  功能：从此输入流中将最多b.length个字节的数据读入一个字节数组中。在某些输入可用前，此方法将阻塞
*  覆盖：类 InputStream 中的 read
* 参数：b - 存储读取数据的缓冲区
* 返回：读入缓冲区的字节总数，如果因为已经到达文件末尾而没有更多的数据，则返回 -1。
*  抛出：IOException - 如果发生 I/O 错误。

来自 <http://blog.csdn.net/lzt623459815/article/details/8477658> 

如果要想读取两个文件，然后对这两个文件进行异或操作，可以利用fileinputstream函数中的read（byte【】 b）方法，字节会存在b中，然后对b进行异或运算

spinner的用法
--
1.         //根据id获取对象  
2.         spinner=(Spinner) findViewById(R.id.spinner1);  
3.         //显示的数组  
4.         final String arr[]=new String[]{  
5.                 "星期一",  
6.                 "星期二",  
7.                 "星期三",  
8.                 "星期四",  
9.                 "星期五",  
10.                 "星期六",  
11.                 "星期日"     
12.         };  
13.           
14.         ArrayAdapter<String> arrayAdapter = new ArrayAdapter<String>(this, android.R.layout.simple_spinner_item, arr); 
15.  spinner.setAdapter(arrayAdapter);  
16.             //注册事件  
17.         spinner.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {  
18.   
19.             @Override  
20.             public void onItemSelected(AdapterView<?> parent, View view,  
21.                     int position, long id) {  
22.                 Spinner spinner=(Spinner) parent;  
23.                 Toast.makeText(getApplicationContext(), "xxxx"+spinner.getItemAtPosition(position), Toast.LENGTH_LONG).show();  
24.             }  
25.   
26.             @Override  
27.             public void onNothingSelected(AdapterView<?> parent) {  
28.                 Toast.makeText(getApplicationContext(), "没有改变的处理", Toast.LENGTH_LONG).show();  
29.             }  
30.   
31.         });  

#疯狂Android讲义
##第5章 使用Intent和IntentFilter进行通信
###5.2
指定Component属性的Intent为显式intent，没有指定Component属性的Intent为隐式Intent。一个Intent对象最多只能包括一个Action属性，但是一个Intent对象可以包括多个Category属性。可以用intent实现点击一个自定义按钮返回home桌面，
```java
Intent intent=new Intent();
intent.setAction(Intent.ACTION_MAIN);
intent.addCategory(Intent.CATEGORY_HOME);
```
intent中data属性与type属性会相互覆盖，设置的后一个属性会把前一个属性覆盖，除非使用setDataAndType()方法。Uri字符串总是满足如下格式scheme://host:port/path.Intent的extra属性用于在多个action之间进行数据交换，是一个bundle对象。Flag属性用于为该Intent添加一些额外的控制旗标，比如Intent.FLAG_ACTIVITY_CLEAR_TOP用于清除当前activity栈中的activity
##第6章 Android应用的资源
无法通过R资源清单类访问的原生资源保存在assets目录下，可以使用AssetManager来访问这些资源。<br>
可以使用#RGB来使用颜色资源，也可以使用#RRGGBB来使用颜色资源。dimens.xml定义尺寸值资源，比如
```xml
<dimen name="cell_width">60dp</dimen>
```
2016/6/13 看到298页
StateListDrawable可用于组织多个Drawable对象，并且其对象会随着目标组件状态的改变而自动切换。<br>
LayerDrawable也可以包含一个Drawable数组，系统会按照这些Drawable对象的顺序来绘制<br>
ShapeDrawable用于定义一个基本的几何图形（如矩形，圆形，线条等）<br>
2016/6/21 看到310页<br>
主题（theme）与样式（Style）的区别在于主题不能作用于单个的view组件，主题应该对整个应用中的所有Activity起作用，或对指定的Activity起作用。主题与样式都可以继承，通过parent来设置<br>
如果要获取Java支持的国家和语言，可以调用Locale类的getAvailableLocales方法获取
##第7章 图形与图象处理
###7.1
```java
//把一个Bitmap对象包装成BitmapDrawable对象
BitmapDrawable drawable=new BitmapDrawable(bitmap);
```
BitmapFactory是一个工具类，有很多方法，用于从不同的数据源来解析、创建Bitmap对象。recycle()强制一个Bitmap对象立即回收自己。<br>
`如何在Android绘图`<br>
```java
public class MyView extends View
{
 public MyView(Context context,AttributeSet set)
 {
   super(context,set);
 }
procted void onDraw(Canvas canvas)
{
 super.onDraw(canvas);
 Paint paint =new Paint();
 ...

}
}
```
2016/6/22 看到342页
```java
 public EmbossMaskFilter(float[] direction, float ambient, float specular, float blurRadius) {
        if (direction.length < 3) {
            throw new ArrayIndexOutOfBoundsException();
        }
```
这个类能够让目标出现浮雕效果，direction 是float数组，定义长度为3的数组标量[x,y,z]，来指定光源的方向，ambient 取值在0到1之间，定义背景光 或者说是周围光，specular 定义镜面反射系数，blurRadius 模糊半径。<br>
而BlurMaskFilter能够使目标出现模糊效果，使用它后会出现一个面具在目标的边缘的指定范围，该面具的边缘是否会被包进目标中，或者是在目标里边，外边，里边都有，这是由BlurMaskFilter.Blur这个枚举所决定的。
###7.3图形特效处理
可以使用，`matrix`进行图形变换，首先获取matrix对象，然后调用matrix的方法进行平移2，旋转，缩放，倾斜等，将程序对matrix所做的变换应用到指定图形或者组件。<br>
Android提供了`Invalidate`方法实现界面刷新，但是Invalidate不能直接在线程中调用，因为他是违背了单线程模型：Android UI操作并不是线程安全的，并且这些操作必须在UI线程中调用。invalidate()是用来刷新View的，必须是在UI线程中进行工作。比如在修改某个view的显示时，调用invalidate()才能看到重新绘制的界面。invalidate()的调用是把之前的旧的view从主UI线程队列中pop掉。 一个Android 程序默认情况下也只有一个进程，但一个进程下却可以有许多个线程。在这么多线程当中，把主要是负责控制UI界面的显示、更新和控件交互的线程称为UI线程，由于onCreate()方法是由UI线程执行的，所以也可以把UI线程理解为主线程。其余的线程可以理解为工作者线程。invalidate()得在UI线程中被调动，在工作者线程中可以通过Handler来通知UI线程进行界面更新。而`postInvalidate()`在工作者线程中被调用<br>
可以使用drawBitmapMesh对图像进行扭曲，可以出现水波荡漾，风吹旗帜等效果<br>
Java从右向左的赋值运算就是当出现a=b=c这样的连续赋值时，相当于(a=(b=c)),即相当于b=c,a=b<br>
shader本身是一个抽象类，可以控制渲染效果，它的几个实现类：BitmapShader产生位图平铺，LinearGradient使用线性渐变，RadialGradient使用圆形渐变，SweepGradient使用角度渐变，ComposeShader使用组合渲染<br>
逐帧动画，补间动画（Tween）,其中自定义的补间动画可以让图片在三维空间中进行旋转。属性动画比补间动画的功能更加强大，首先补间动画只能定义帧在透明度、旋转、缩放、位移四个方面的变化，但是属性动画可以定义任何属性的变化。补间动画只能对UI组件执行动画，但是属性动画几乎可以对任何对象执行动画。ValueAnimator是属性动画主要的时间引擎，负责计算各个帧的属性值，是Animator的子类；ObjectAnimator是ValueAnimator的子类，使用起来更加简单，更常用。<br>
使用属性动画的步骤如下：<br>
1.创建ValueAnimator或者ObjectAnimator对象<br>
2.根据需要为Animator对象设置属性<br>
3.如果需要监听就为Animator对象设置事件监听器<br>
4.如果要有多个动画，用AnimatorSet组合这些动画<br>
5.调用Animator对象的start()方法启动动画。
2016/6/26 看到388页
##第8章 Android数据存储与IO
###SharedPreference
SharedPreference用于对少量的数据进行保存，保存的数据类型主要是简单地Key-Value对。Environment.getExternalStorageDirectory()用来获取外部存储器，也就是SD卡的目录。BufferedReader从字符输入流中读取文本并将字符存入缓冲区以便能提供字符、数组和线段的高效读取。可指定缓冲区尺寸或使用缺省尺寸。该缺省尺寸对大多数用途来说是足够的。
```java
BufferedReader br=new BufferedReader(new InputStreamFReader(fis));//fis为FileInputStream 流
```
如果用FileOutputStream向指定文件写入数据，会把原有的文件内容清空，如果要追加文件内容，可以使用RandomAccessFile 
###8.3 SQLite数据库
JDBC（Java Data Base Connectivity,java数据库连接）是一种用于执行SQL语句的Java API，可以为多种关系数据库提供统一访问，它由一组
用Java语言编写的类和接口组成。
2016/6/26 看到404页
`Cursor` 是每行的集合，使用 moveToFirst() 定位第一行。<br>
DDL：数据库模式定义语言，关键字：create<br>
DML：数据操纵语言，关键字：Insert、delete、update<br>
DCL：数据库控制语言 ，关键字：grant、remove<br>
DQL：数据库查询语言，关键字：select<br>
可以通过Android SDK中platform-tools目录下的sqlite3.exe工具来查询、管理数据库。当然也可以通过Android提供的insert，update，delete，query等方法来操作数据库。<br>
在实际项目中一般很少用SQLiteDatabase的方法来打开数据库，通常会继承SQLiteOpenHelper来开发子类。<br>
可以使用GestureDetector类来检测手势，其中有个onFling函数可以用来检测滑动的速度以及最终停下来的位置。GestureOverlayView可以自定义编辑手势
2016/6/27 看到235页
##第9章 使用contentprovider实现数据共享
应用程序通过contentprovider暴露自己的接口，其他应用程序可以通过该接口来操作该应用程序的内部数据。ContentProvider相当于一个网站，作用是暴露数据，ContentResolver相当于HttpClient<br>
将字符串转化成uri，uri工具类提供了parse()静态方法，Uri uri= Uri.parse("XXX")<br>
编写完contentprovider的类之后，只要在<application/>元素添加<provider>子元素即可配置contentprovider。contentprovider只存在onCreate()一个生命周期。<br>
Android系统提供了很多ContentProvider，比如联系人，比如多媒体内容（camera）。
###9.4 监听contentprovider的数据改变
如果应用程序需要监听contentprovider所共享数据的变化，并且能够通过数据改变而提供响应，这就需要用contentobserver
##第10章 service与broadcastreceiver
BroadcastReceiver组件像一个全局的事件监听器，用于监听系统发出的Broadcast，OnXXXListener在程序退出时就随之关闭，但是Broadcastreceiver
不会<br>
###Service
Service组件创建时会调用onCreate()方法，被启动时会调用onStartCommand方法，被关闭前调用onDestroy方法
IBinder onBind(Intent intent)是service必须实现的方法，该方法返回一个IBinder，应用程序可以通过该组件与service组件通信<br>
boolean onUnbind(Intent intent)当该Service上绑定的所有客户端都断开连接时将会回调该方法。<br>
运行service有两种方法：通过context的startService()方法，访问者与service没有关联，即使访问者退出了，service继续运行。<br>
通过context的bindService()方法，访问者与service绑定在一起，访问者一旦退出，service也就终止。但是如果组件调用unBindService()取消与该service的绑定时，也只是切断该Activity与Service之间的关联，并不能停止该Service组件。<br>
如果service需要和访问者之间进行方法调用或者交换数据，应该使用bindService()和unbindService()
2016/7/3日看到462页
实现ServiceConnection.你的实现必须重写两个回调方法：onServiceConnected(),系统调用这个来传送在service的onBind()中返回的IBinder．OnServiceDisconnected(),Android系统在同service的连接意外丢失时调用这个．比如当service崩溃了或被强杀了．当客户端解除绑定时，这个方法不会被调用．<br>
Service的生命周期，非绑定service的生命周期，也就是通过startservice()，过程是onCreate(),onStartCommand(),onDestroy()方法。<br>
被绑定Service的生命周期，onCreate(),onBind(),onUnbind(),onDestroy()
####IntentService
Service与它所在的应用处于同一个进程当中，Service不是一个新的线程，因此不应该在Service中直接处理耗时的任务。如果需要在Service中处理耗时的任务，建议在Service中另外开启一条新线程来处理耗时任务。也可以使用IntentService来实现。<br>
IntentService会创建单独的worker线程来处理所有的Intent请求。只需要重写onHandleIntent()方法就可以，并不需要实现onBind()与onStartCommand(）方法<br>
AlarmManager不仅可以定闹钟，而且可以用来定时更换壁纸之类的
###BroadcastReceiver
BroadcastReceiver用于接收程序发出的Broadcast Intent，如果其onReceive()方法没有在10s内执行完成，Android程序会认为该程序没有响应。发出Broadcast Intent后，所有匹配该Intent的BroadcastReceiver都有可能被启动。<br>
broadcast分为normal broadcast（普通广播），普通广播所有接收者在同一时刻接收广播，但是无法终止。orded Broadcast（有序广播），有序广播就是广播含有优先级，在android：priority属性中设定，数字越大优先级别越高。取值-1000到1000，接受者可以终止Broadcast Intent的传播。
2016/7/4看到488页
