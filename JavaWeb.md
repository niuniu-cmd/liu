#Web总结
###Http协议：
   一个传输层的网络协议，超文本传输协议。
###特点：
      1：简单快速，支持多种不同的数据提交方式
	  2：传输数据时，数据类型与大小不受限制
      3：无连接协议：每次连接处理一次请求，服务器回应一次后立即断开连接
      4：无状态协议：服务器在处理客户端的请求时，没有记忆能力。
###组成部分：
        1.请求（客户端找服务器要数据）
		2.相应（服务器回应客户端的操作）
请求组成的四部分：

      1：请求头：
           有一个个键值对组成，用于描述客户端的信息。
	  2：请求体
           由一个个键值对组成，存储的POST请求的数据，GET的数据存储在网址中，没有请求体。
	  3：请求空行：
	 		请求头部和请求空行之间的一行空白行
      4：请求行：
            由一个个的键值对组成，描述的请求方式，请求地址，以及版本协议
相应的三部分组成：
	
	  1：响应头： 
         有一个个键值对值对组成，用于描述服务器的信息。
	  2：响应体：
		 响应的内容，通常是一个HTMLW文件
      3：相应行
		 由一个个键值对组成，描述的是服务器的协议版本，响应码的状态，以及响应成功失败的原因。
###HttpServlet
    
     Servlet是一个类，运行在tomcat的一个类。
      是用来处理客户端的请求，可以给客户端进行动态的响应。
###编写步骤
	
	编写一个java类继承HttpServlet. 
    重写service方法
    在service方法中，处理请求，对客户端进行响应。
    配置servlet的访问地址，供浏览器浏览。
###servlet访问地址的配置
   
     方式一：(web3.0新方式)
       在类名前通过注解，添加访问地址
        添加单个访问地址 @WebServlet("/地址")
		添加一组地址：@WebServlet({"/地址","/地址"})
###Servlet访问地址：
        协议：ip:端口号/项目名+映射地址
           例如：
           http:192.168.34.77/day01_Servlet/hello
###Servlet
	
	指的是Servlet对象从创建到垃圾回收的过程。
	
	一个Servlet在tomcat中只会被创建一次，每次访问，都是在重复的调用此对象的service方法
	
	创建时机：

    	 	默认情况下，用户第一次访问servlet时，对象被创建。以后在访问，重复使用该对象
    销毁时机：
			在web应用在卸载时，或服务器关闭时，导致Servlet会被垃圾回收。

 
    	
###为了解servlet的生命周期，添加了三个事件，分别是
 		
	init()  : 当servlet对象创建完毕时。执行
    
    service：  当用户访问servlet时，执行

    destory():  当servlet准备销毁时，执行
###线程的安全问题
	
	 异步即并行，同步即排队；
       解决线程安全的问题，可以采用同步机制，让线程排队执行。

      同步机制的两只实现方式：
    
     1：同步代码块
     2：同步方法
  
    注意：同步执行，多个线程必须使用同一把锁才可以实现排队。
   
               同步代码块的锁对象是传入的
			   同步方法的锁是this
			   静态同步方法的锁事 类名.class
###GET请求和POST请求的区别：
 
	get:
		1:没有请求体，请求参数在网址？后一键值对的形式拼接
		2：只能传输字符串的类型
		3：网址的最大容量为4kb
		4：数据传输时，参数一明文的方式显示，不安全
		5：tomcat8的版本。传输中文不乱码

   POST：
	    1：   请求的参数，一键值对的形式单独存储在数据包中，数据包就是请求体  
		2：可以传输任意类型的数据
		3：数据传输的大小理论上是没有限制的
		4：数据传输时，无法在网址上观察的，较为安全
        5：POST传出中文时需要调整编码格式，否则乱码。
###接受请求中的参数
		
	根据参数名称，获取一个参数的值	
		String  value= request.getParameter(String name);

	根据参数的名称，获取一组参数的名称
		string []values = request.getParametrtValues(Sting name);
###请求乱码处理
   
    方案1(适合所有的情况)
  
        将乱码文字 按照ISO-8859-1编码 转换为字节数组
 			
            byte [] by  = 乱码文字.getBytes("ISO-8859-1");
	
		按照utf-8编码 将字节数组, 组装为新的string对象
  		
		   String  正常的文字 = new String (by ,"UTF-8");
    方案2：(仅用于post请求)
     
        注：  在获取请求参数之前设置

		request.setCharacterEncoding("UTF-8");
###创建Servlet时机的调整

    	<servlet>
			<servlet-name></servlet-name>
            <servlet-class></servlet-class>
     		<load-on-startup>整数</oad-on-startup>
        </servlet>
          整数：
             默认为  -1
			当小于0时，第一次访问时创建
			当大于0时，在服务器启东时创建，值越小创建的越早
        

###请求转发
		 步骤：
	1：获取请求转发器
		 RequestDispatcher rd =request.getRequestDispatcher("要转发的地址");
	2：进行转发
		 rd.forward(请求对象,响应对象);
	
	内部流程：
   
        当浏览器请求Servlet时，Servlet可以通过tomcat将请求信息和响应功能，转交给另一个service
	
	
    特点 ：
	1：	转发过程，多个组件共享一个请求和响应。 
    2：无论转发多少次，对于浏览器，只发起了一次请求，会接到一次响应，浏览器的地址栏不变
    3：不能夸项目
    4：相对于重定向，效率高·
###请求重定向
 	
     步骤：
     request.sendRedirect("重定向的地址")

     内部流程：
       
      当浏览器请求服务器是=是，服务器会给浏览器一个302的值，以及一个键值对，键为location，值为新地址
      当浏览器就收到302后，主动寻找location的值
       知道location后，自定发起新的请求，请求这个新地址
###注意
    
     1： 请求转发和重定向 在使用时一定要有出口
     2：在一个Servlet处理流程中，一旦发生了转发和重定向,此Servlet就不在响应。
###ServletContext
  
     用于关联多个Servlet 用于Servlet之间数据的共享与通信。
     
       类似于一个map集合,任何的Serlet都可以向其存储数据和获取数据。
		一个项目只会拥有一个servletContext对象，在项目启动时创建，项目销毁时销毁
###上下文对象的使用


    1  设置键值对  替换键值对
   
       context,setAttribute(String 键，Sttring 值);
   
    2  根据建获取值 ，键值不存在时返回null
   
        object  值 = context.getAttruibute(String 键);
  
    3  根据建移除键值对
 
          context.[removeAttribute(String 键);
 
    4：获取当前项目所在的文件夹的路径
	
      String  path=  context.getRealPath("/") ;

##Cookie
  
     原理：
  
        1：当浏览器请求服务器时
		2:服务器创建一个Cookie
		3:服务器将Cookie添加到响应体头部(可以添加多个)
		4:浏览器接收到响应头得到Cookie，将其全部存储到文本文档中
		5：当浏览器再次访问服务器时，会将文本文件中所有服务器储存的Cookie带到请求头发送给服务器
		6：服务器得到请求头的Cookie后就知道浏览器曾经访问过
   
###Cookie常用的方法
	
		1： 创建Cookie(多个相同Cookie，值会覆盖 )
  	
	  Cookie cookie = new Cookie(String 键,String 值);
		
        2：从请求头部获取Cookie
		
			Cookie[] cookie=request.getCookie(); 

		3: 取出值：		
	       cookie.geiName();
		
         4:取出值：  
		
		  cookie.getvalue();
		 5:设置路径的问题
	
		  cookie.setPATH("/"); 
###Cookie的优缺点

	缺点：
	1：Cookie存储数据类型只是字符串，且tomcat8.5版之前无法储存中文
	2：储存的数据大小有限，cookie键+cookie值 不能超过4kb
    3:一个服务器储存的cookie数量有限
	4：数据存储在客户端，不安全
	5：依赖于用户的浏览器设置，用户可以禁用cookie，还可以指定删除我们存储的cookie
###Session
  
	原理：
      1：当浏览器请求服务器时，服务器创建一个HttpSession的对象
	  2：session对象会根据id产生一个Sessionid,在java中生成的Jsessionid
	  3：tomcat 会将此id按照Cookie的形式发送给浏览器
	  4：浏览器接收到Cookie后，存储文本文件。
	  5:浏览器再次访问服务器时，会携带Cookie,将Ssessionid传递给服务器
	  6：当程序使用的session存储数据时，tomcat会自动根据sessionID找到对应的session对象，供我们是用
###session对象
		    1：  获取session：
	 HttpSession session =request.getSession();
 		
			2：设置与替换数据
	
	 session.setAttruibute(String 键,String 值);

		3:获取数据

	 session.geyAttrubite(String 键);
		4:移除数据

	session.removeAttribute(String key);
		5:销毁session

	 session.invalidate();
##JSP


   1：输出表达式

    <%=变量%>
    
   2:java代码执行区

     <%
      代码执行区
        %>
   3：三种注释

	单行注释：//
	多行注释：/**/
	文档注释：<%-- --%>
在用户请求JSP时，用户接到相应的流程
   

    Java代码注释, 在JSP文件转换为Servlet时, 会作为Java代码原封不动的转换到Servlet中.
    HTML注释, 在JSP文件转换为Servlet时, 会看作HTML代码, 在Servlet就将其打印到响应体中.
    JSP注释, 在JSP文件转换为Servlet时 会忽略.
###三大指令
 
 
       page指令  include指令  tagli指令

###page指令：
        用于配置页面信息的指令

    
      <%@ page
        language="java" //语言类型
        contentType="text/html; charset=UTF-8" //内容类型
        pageEncoding="UTF-8" //编码
        extends="继承的父类"
        buffer="数字或none"//是否允许缓存, 默认值8kb
        autoFlush="true或false"//是否自动清除缓存 默认值true
        session="true或false"//是否提前创建session 默认值true
        import="导包列表"//用于导包, 多个包之间使用逗号隔开.   *****
        isThredSafe="true或false"//代码执行区是否是线程安全的,默认false.
        errorPage="网址"//当前页面发生BUG时, 自动跳转到指定的处理错误页面.
        isErrorPage="true或false" //当前页面是否是处理错误的页面,默认值false,
                                // 当设置为true时, 如果因为其他页面发生BUG跳转至此页面, 则此页面会提前创建一个Exception对象, 包含了发生BUG的页面的错误信息.
     %>
###include指令

		作用：用于讲一个JSP或HTML文件，引入另一个JSP中
        格式：<%@include file ="文件地址"%>
###include动作

		作用：用于讲一个JSP或HTML文件，引入另一个JSP中
		格式：<%@include page="文件地址"%>
###区别
    include指令：引入文件时，发生在引擎的转换，多个JSP最终生成一个。java文件
    incldue动作：引入文件时，发生在浏览器，多个JSP文件最终生成多个Servlet文件
###九大内置对象

     1：对象名： request
	    对象类型：HTTPServletRequest
		对象作用：请求对象，包含了请求信息，大量的get方法。可以获取请求的数据
	  
		
     2： 对象名： response
	    对象类型：HTTPServletResponse
		对象作用：响应对象，包含了一些响应的功能
    

      3： 对象名： config
	    对象类型：ServletConfig
		对象作用：Servlet的配置对象, 用于给Servlet进行初始化的配置.


	  4： 对象名： out
	    对象类型：JSPWriter
		对象作用：打印流，用于打印响应体


	  5： 对象名： exception
	    对象类型： Throwable
		对象作用：只有当前页面的isErroePage=true时。此对象才存在， exception默认为null.作用：其他页面发生BUG时，跳转到此页面 exception才不为null。

	  6 ： 对象名： page
	    对象类型：Object
		对象作用：在代码中，page对象的赋值为this，表示当前对象


      7： 对象名： session
	    对象类型：HttpSession
		对象作用：会话对象，用于会话跟踪，以及状态管理


       8： 对象名：APPliocation
	    对象类型：ServletContext
		对象作用：servlet的上下文对象，用于多个servlet与jsp之间的进行通信

	  9： 对象名：pageContext 
	    对象类型：PageContext
		对象作用：页面上下文对象，用于当前页面的多个代码块之间通信。也可以获取其他内置对象


###四大域对象对象

九大内置对象中，存在四个较为特殊的对象，四个对象用于在不同的作用域下存储数据

其都具备以下方法：
 
	存储数据：
	
		setAttribute(String键, String值 )；
	获取数据
		Object 值=getAttribute("String键");
	移除数据
		removeAttribute("String 键")
四大域对象：

	pageContext:作用域当前页面的第一行到最后一行

	request:作用域一次请求，一次请求经转发后，可能包含多个Servlet或多个JSP页面

	session：作用域一次会话，可能会发生多次请求

	Appliction:作用域一次服务(服务器启动到关闭)，可能存再多次会话


###EL表达式：
取出数值直接输出：

	${域对象存储的键}

取出对象属性输出

	格式1： ${对象存储的键.属性名}
	
	格式2： ${对象存储的键["属性名"]}

	格式3： ${对象存储的键[属性名存储的键]}
###EL取出表达式的流程

	现在pageContext中找数据是否存在

	如果不存在就去request中寻找数据是否存在

	如果不存在就去session中数据是否存在

	如果不存在就去appliction中找是否存在，如果不存在就不输出任何数据。

	在上述流程中，任意步骤找到了，数据直接输出，流程终止。

###将JAVA对象转为JSON字符串

	步骤：
			创建GSON对象
	
				gson  g = new gson();
		将需要的转换的对象，转化为字符串

			String tete = g.tojson(java对象);
###将json转换为java对象

	    创建GSON对象
	
				gson  g = new gson();
		将字符串转换为指定类型的对象

		g.fromJson(json格式的字符串,类名.Class);
    当我们要转换的JSON数据，没有可匹配的类，可已经JSON数据转换为Map集合。
##AJax
###GET

	创建一个异步请求对象
		
	  var  xhr = new XMLHttpRquest();
	
	设置异步请求方式，以及网址和参数

		xhr.open("GET","网址?参数列表");

	设置请求状态的事件的处理函数

	xhr.onreadystatechange= function{

      通过xhr.readyState 获取请求的状态.
            //  状态值有五个:
            //  0:  正在初始化
            //  1:  请求正在发送
            //  2:  请求发送完毕
            //  3:  服务器正在响应
            //  4:  响应完毕, 连接已断开

	`	判断事件发生时, 状态为4 
		if(xhr.redyState==4){
	`判断服务器的响应状态码(xhr.status) (200/404/500 等等)
			if(xhr.status==200){
				//请求成功
				成功时, 可以通过xhr.responseText 接收响应体.
			     }else{
				 //请求失败, 一般是因为服务器BUG  或 用户网络问题.
					}

               }
            }


		发送请求
		xhr.send();
###POST

	1.  创建一个用于异步请求的对象

		var xhr= new HttpRquest();
  	2：设置异步请求的方式, 以及网址
	   xhr.opend("POST","网址");
   3：设置请求状态事件的处理函数

	xhr.onreadystatechange=function{
//  通过xhr.readyState 获取请求的状态.
            //  状态值有五个:
            //  0:  正在初始化
            //  1:  请求正在发送
            //  2:  请求发送完毕
            //  3:  服务器正在响应
            //  4:  响应完毕, 连接已断开

		4.  判断事件发生时, 状态为4 .
		
              if(xhr.readyState == 4){
	5.  判断服务器的响应状态码(xhr.status) (200/404/500 等等)
                if(xhr.status == 200){
                    //请求成功
		6.  成功时, 可以通过xhr.responseText 接收响应体.
                }else{
                    //请求失败, 一般是因为服务器BUG  或 用户网络问题.
                }
            }
        }


    }
	7:如果要传递参数，需要加入POST请求头

		xhr.setRequestHeader("content-type","application/x-www-form-urlencoded");
	8:发送请求

		xhr.send(参数列表);
##ajax-Jquery
	函数名称：$.ajax
	格式：$.ajax({

	url:"请求网址",
	type:"GET或Post",
	async:"请求是否异步",
	data:"请求的参数列表",
    dataType:"Text或Josn",
	success:finction(data){

	




      },
	error:function(){
	/当服务器响应的状态码不再200-299之间时, 表示失败, 此函数执行

     }
     })



###get函数与post函数

函数名称$.get或$.post
 
		参数列表：

			参数1：  url:String类型  请求网址
			参数2：  data :String类型 请求参数
			参数3：success:function函数，成功时候处理的函数
			参数4：dataType: 响应的数据类型，text或json
###getJson函数


函数名称：$.getJSON
	
	参数列表：url:请求网址
			 data:请求参数
			 success:functiona函数，成功的时候执行的函数
	
##Ajax-Vue
   全局的Ajax
		
		在不使用Vue实例的情况下，使用ajax函数

		GET格式:
		  Vue.http.get("请求地址",["请求的参数"]).then(success,error);
		
		Post格式：
	
			Vue.http.post("请求地址",["请求参数"]),{"emulateJSON":true}.then(success,error);

###基于Vue实例的ajax函数


	GET格式:
		 this.http.get("请求地址",["请求的参数"]).then(success,error);
		
	Post格式：
	
			this.http.post("请求地址",["请求参数"]),{"emulateJSON":true}.then(success,error);
###上述格式中，出现的参数的写法
  
     1.请求地址:   string类型的字符串. 相对路径即可.
     2.GET请求的参数:   请求的参数以对象的方式传递. 例如: 我们需要传递username和password, 可以按照如下格式编写:
        var user = new Object();
        user.username = "xxx";
        user.password = "xxx";
        var obj = new Object();
        obj.params = user;
        //传递obj对象即可

        或者:
        {
            params:{
                username:"xxx",
                password:"xxx"
            }
        }

		3.POST请求的参数: 请求的参数以对象的方式传递. 例如: 我们需要传递username和password, 可以按照如下格式编写:
        var user = new Object();
        user.username = "xxx";
        user.password = "xxx";

        或者:
        {
            username:"xxx",
            password:"xxx"
        }

		4.success函数 与 error函数的编写
        success函数 与 error函数编写方法完全一致;
        格式:
            function(res){

            }

        res(响应对象)的常用属性:
            -   url :   响应的网址
            -   body:   响应体 , 如果响应的内容是JSON格式的, 得到的是对象, 如果不是JSON,得到的是string
            -   ok  :   boolean值, 当响应码为200-299之间时, 为true
            -   status  :   响应码: 200 , 404 ,500 等等
            -   statusText : 响应码对应的文字, 例如: 200对应的是ok 

        res(响应对象)的常用函数:
            -   text()  :   以字符串形式, 返回响应体
            -   json()  :   以对象形式, 返回响应体
            -   blob()  :   以大数据二进制形式, 返回响应体.