#IO与NIO
********************
##IO
###什么是IO
###IO分为哪几种流。
如图：
![](IO.png)<br>
<b>从数据来源或者说是操作对象角度看，IO 类可以分为：</b><br>
<b>* 1 文件（file）: FileoutStream ,FileoutPutStream,FileReader , FileWriter<br>
   * 2 数组（[]） <br>
   *   2.1字节数组(byte[]): ByteArrayInputStream , ByteArrayOutputStream<br>
   *   2.2字符数组(char[]): CharrrayReader ,CharArrayWriter<br>
   * 3管道操作：PipedInputStream , PipedOutputStream , PipedReader , PipedWirter<br>
   * 4:基本数据类型： DataputStream , DataOutputStream<br>
   * 5:缓冲操作：BufferedInputStream ,BufferedOutputStream,BufferedReader,BufferedWriter<br>
   * 6:打印：PrintStream ,PrintWriter<br>
   * 7:对象序列化反序列化：ObjectInputStream ,ObjectOutputStream<br>
   * 8：转换：InputStreamReader、OutputStreWriter<br>
<b>从数据传输方式或者说是运输方式角度看，IO 类可以分为：</b><br>
 *1：字节流<br>
 *2: 字符流<br>
###字节流与字符流的区别
 字节流读取单个字节，字符流读取单个字符（在UTF-8中一个字符对用3个字节，在中文编码中对应2个字节）。<p>
 字节流用来读取二进制文件（图片，MP3 视频），字符流用来处理文件<p>
如图：
![](1.png)
##IO相关的类
 *常用的几个类，如文件最基本的读写类File开头，文件读写带缓冲的Buffered开头，对象序列化反序列化想换的Object开头。<p>
 *最基本的抽象类 InputStream、OutputStream、Reader、Writer<br>


###InputStream方法:<p> ![](inputstream.png)

###OutputStream方法:<p>
![](OutputStream.png)<br>
要想多次写入数据时，要在实参中增加true,要想写入字符串时用字符串.getBytes()把字符串转换为数组



###Reader方法:<p>
![](reader.png)<br>

 
###wirte方法:<p>
![](writer.png)<br>




###PrintStream方法：<p>
![](printStream.png)<p>
用法：
PrintStream  pm = new PrintStream(new FileOutputStream())
readLine()--读取一行，遇到行分隔符停止
###BuffereReader
基本概念
      主要用于读取      单个字符，字符数组，一行字符串
 
用法：
     BuffereReader br = new BuffereReader(new InputStreamReader(new FileInputStream("c/.txt")))
###objectOutputStream 
基本概念：
将对象整体写入出输流中，只支持java .io.Serializable接口的对象写入流中。也就是被写入的对象数据类型必须实现该接口（java .io.Serializable）<p>
![](ObjectoutputSrteam.png)

####用法
    ObjectoutputSrteam oos  =new ObjectoutputSrteam(new FileOutpurStream("c.txt"))

********  
<b>序列化：<br>
   将一个对象需要班存的所有相关的信息有效组织成字节序列 的转化过程  <b/> <p>
###ObjectinputSrteam
基本概念：<br>
   从输入流中讲一个对象整体读取出来。<p>
常用方法：
![](ObjectInputStream.png)
用法：
    ObjectInputStream ois  = new ObjectInputStream(new FlieInputStream("c.txt"))

******************************
 
******************************
******************************
#NIO
###什么是NIO：

    N IO是面向缓冲区非堵塞的。核心在于通道（Channel，负责数据传输） 缓冲区（Buffer,数据存储）
      选择器（Selector ,使一个单独的线程管理多个Channel.Selector是费堵塞IO的核心）
###流程

# 数据总是从通道读取到缓冲区中，或者从缓冲区写入到通道中
###定义：
     即 java new IO ,是一个全新的JDK1.4，java 8后提供的IO API。


###作用：
     提供了与标准的IO不同的IO工作方式
###新特性：
  ![](newIO.png)
    即 基于通道&缓冲区的操作。具有非阻塞的特性，带有选择器的功能。自带字符集   编码解码的解决方案。还支持锁&内存映射的文件访问接口。
 
#核心组件：
##1：通道：
   用户数据源节点和目标节点的连接。
   在nio中负责缓冲区的数据传输。
   本身不存储数据，需要配合缓冲区仅限传输
###实现类
   FileChannel---文件的数据读写。<br>
   SocketChannelTCP---的数据读写。<br>
   ServerSocketChannel---允许我们监听TCP链接请求，每个请求会创建会一个SocketChannel<br>
   DatagramChannel---UDP的数据读写<br>

##2:缓冲区(Buffer)<br>
###<b>2.1:Buffer的核心组件</b><br>
      * capacity：容量，表示缓冲区中最大存储数据容量，一旦声明不可改变<br>
      * limit： 界限，表示缓冲区可以操作的数据大小<br>
      * position :位置，表示缓冲区正在操作的数据位置<br>
      * mark:标记，表示记录当前的position的位置，可以通过reset()恢复到mark的位置<br>
      * mark<=position<=limit<=capacity<br>
###<b>2.2直接缓冲区和非直接缓冲区：</b><br>
   非直接缓冲区：通过allcate()分配缓冲区，缓冲区建立在JVM<br>
   直接缓冲区：通过allcateDirect()方法创建。缓冲区建立在系统的物理内存<br>
## 3:选择器<p>





##具体操作一( 基于通道 & 缓冲数据)
    1： 获取数据源 和 目标传输地的输入输出流（此处以数据源 = 文件为例）
     FileInputStream fis = new  FileInputStream("c/a.txt");
     FileoutputStream fps  = new   FileoutputStream ("c/a.txt");

     2:获取数据输入输出的通道
     FileChannel fcin =fis.getChannel();
     FileChannel fcout = fps.getChannel();

    3:创建缓冲区 对象：Buffer(共有两种方法)
    方式一：  使用allocate()静态方法
    ByteBuffer buff = ByteBuffer .allocate(256);
    //创建一个容量大小为256的ByteBuffer。若容量太小需要重新创建。

    方式二：通过包装的方法创建缓冲区保留了被包装数组内保存的数据
    ByteBuffer buff = ByteBuffer.wrap(byteArray);

    // 额外：若需将1个字符串存入ByteBuffer，则如下
     
        String sendString="你好,服务器. ";
     ByteBuffer   buff = ByteBuffer.warp(sendString.getBytes("utf-8"));

    4:从通道读取数据 & 传出数据到通道
     //注：读到通道末尾返回-1
        fcin.read(buff)；

     5：传出数据准备：将缓存区的写模式 转换->>读模式
        buff.flip();
   
     6:从缓冲区读取数据 & 写入到通道
      fcout.wirte(buff)
     7：重置缓冲区
      buff.claer()
##具体操作二(基于选择器)（Selecter）
      1:创建Selector对象
    Selector select = new Selector.open()
       2:向Selector对象绑定通道
        a:创建可选择通道，并配置为非堵塞模式
       ServerSocketChannel server =  ServerSocketChannel.open();
          server.configureBlocking(false);
        b:绑定通到指定端口
        ServerScoket socket=servcer.socket();
        InetSocketAddress address = new InetSocketAddress(8080); 
           
            socket.bind(address);
         c:向Selector中注册感兴趣的事件
         server.register(sel,SelectionKey.OP_ACCEPT);
          return sel;  
         3:处理事件    
    try {    
       while(true) { 
        // 该调用会阻塞，直到至少有一个事件就绪、准备发生 
        selector.select(); 
        // 一旦上述方法返回，线程就可以处理这些事件
        Set<SelectionKey> keys = selector.selectedKeys(); 
        Iterator<SelectionKey> iter = keys.iterator(); 
        while (iter.hasNext()) { 
            SelectionKey key = (SelectionKey) iter.next(); 
            iter.remove(); 
            process(key); 
        }    
    }    
    } catch (IOException e) {    
    e.printStackTrace();   
    }
##感兴趣的事
	OP_ACCEPT：接收

	OP_CONNECT ：连接

	OP_READ ：读

	OP_WRITE：写

###缓冲区一些常用的方法
    Buffer clear( )	清空缓冲区并返回对缓冲区的引用
	boolean hasRemaining( )	判断缓冲区中是否还有元素
	Buffer reset( )	将位置position转到以前设置 mark 所在的位置
	capacity()返回缓冲区的容量。
	position()返回当前的位置，而position(int)可以设置新的位置，如果给定的参数超过了limit或小于0，会抛出IllegalArgumentException，另外position小于mark，会把mark置为-1，因此在缓冲区使用过程中会一直保持mark<=position<=limit。
	limit()直接返回limit属性值。
	(int)会设置新的limit值，如果新的limit超过了capacity会抛出IllegalArgumentException，如果小于position，则会把position设置为limit，如果小于mark，则mark设为无效值-1。
	 mark()方法会把当前position记录到mark属性，而reset()方法会把position值设置为mark。
	clear()将Buffer置为初始状态，position置为0，limit设置为capacity，mark设置为-1。通常在写入数据之前需要调用该方法。
	flip()表面意思是“翻转”，生动的描述了Buffer从写状态进入读状态，该方法会将limit设置为position，同时position设置为0，mark置为-1。
	rewind()方法将position设置为0，以便能重头开始读写数据。
	remaining()返回缓冲区剩余元素的大小，limit-position。hashRemaining()表示缓冲区是否还有剩余元素
         
##缓冲区的数据操作
Buffer 所有子类提供了两个用于数据操作的方法：get( ) 与 put( ) 方法
*获取 Buffer 中的数据
	get( ) ： 读取单个字节<br>
	get( byte[] dst) : 批量读取多个字节到dst中<br>
	get( int index) : 读取指定索引位置的字节(不会移动position)<br>
*放入 数据到 Buffer 中
	put(byte b ) : 将给定单个字节写入缓冲区的当前位置<br>
	put(byte [] src) :	将src中的字节写入缓冲区中的当前位置<br>
	put(int index,byte b ) : 将指定的字节写入缓冲区的索引位置( 不会移动position )<br>
###分散读取，聚集写入
		 //1.分散读取
        RandomAccessFile raf1=new RandomAccessFile("1.txt", "rw");
        //获取通道
        FileChannel channel1 = raf1.getChannel();
        //分配两个指定大小的缓冲区
        ByteBuffer buf1 = ByteBuffer.allocate(100);     
        ByteBuffer buf2 = ByteBuffer.allocate(1024);    
        //构建缓冲区数组
        ByteBuffer[] bufArr={buf1,buf2};
        //通道读取
        channel1.read(bufArr);
        //切换缓冲区为写模式
        for (ByteBuffer byteBuffer : bufArr) {
            byteBuffer.flip();
        }
        System.out.println(new String(bufArr[0].array(), 0, bufArr[0].limit()));
        System.out.println("--------------------------");
        System.out.println(new String(bufArr[1].array(), 0, bufArr[1].limit()));

    /2.聚集写入
        //聚集写入到2.txt中
        RandomAccessFile raf2=new RandomAccessFile("2.txt", "rw");
        FileChannel channel2 = raf2.getChannel();
        //将缓冲区数组写入通道中
        channel2.write(bufArr);
##IO和NIO区别
###*IO 是面向流的，NIO是面向缓冲区的。
IO面向流的操作一次一个字节的处理数据，一个输入流产生一个字节的数据，一个输出流消费数据。导致读取和写入效率不佳<br>
NIO是面向快(缓冲区)的操作，在一次中产生或者消费一块数据。按块处理比(按流的)字节处理数据
要快的多，同时数据读取到一个稍后的缓冲区，需要时可在缓冲区中前后移动。通俗来说，NIO采用了预读的方式。当你读取某一部分数据时，他就会猜测你下一步可能会读取的数据而预先缓冲下来。
###IO是堵塞的，NIO是费堵塞的
 IO：当一个线程调用read()或write()时，该线程被堵塞，直到一些数据被读取或者完全写入时。
该线程在此期间不能再干任何事情了<br>
NIO:使用一个线程发送请求时，么有得到相应之前，线程是空闲的，此时线程可以执行其他任务，不像IO只能等待响应
###NIO和IO各适用的场景：
 当同时打开成千上万的链接，这些链接每次发送少量数据时，可以用NIO。<br>
 当少量连接时，每个链接发送大量数据是用IO。
###NIO不足
   因为NIO 是面向缓冲区的操作，每一次数据处理多是都缓冲区进行，在数据处理之前要对缓冲区的数据是否完整或者已经读取完毕进行判断。假设数据只读取了一部分，那么对不完整的数据处理没有任何意义。所以每次数据处理之前都要检测缓冲区数据。
 
  
 
    