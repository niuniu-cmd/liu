#XML的解析方式
##1：SAX解析
   事件驱动机制的解析方式。
     由解析器逐行读取XML文件，每当读取到开始/结束/内容/属性时，触发事件，可以根据事件的发生，执行的代码，获取相应<br>    
    

    缺点:
                1.  因为是逐行读取, 读取时不操作的数据, 过去后就无法操作了.
                2.  因为事件驱动机制, 我们程序员只能得知事件发生, 无法感知到发生事件的元素层次. 需要程序员自己标记节点层次
                3.  因为逐行解析, 效率相对较低.
                4.  sax解析方式, 是只读解析方式 , 无法修改XML文档的内容.
    优点： 
                  在读取大文件时, 节省内存.<br>
               (因为逐行读取, 所以读取下一行数据时, 上一行数据就已经被释放了)


##2:DOM解析
  直接将整个文档, 加载到内存. 在内存中建立文档树模型. 然后通过操作文档树, 来完成数据的获取 与修改.<br>


            缺点:
                无法解析大文件
            优点:                 
                1.  文档整体被加载到内存中。 虽然耗费了内存。 但是解析时效率就相对高了很多。
                2.  文档在内存中加载， 可以任意的读取 、 修改 、 删除。
            
##3:JDOM解析
  是DOM解析的扩展，与DOM的优缺点一致。

	
   
##4:DOM4J解析
  是DOM解析的扩展，与DOM的优缺点一致  
   ####流程：

     1：引入jar文件（dom4j.jar）
     2：创建指向XML稳当的输入流
     InputStream is = new FileInputStream("文件的地址");
     3：创建一个XML的读取工具对象
      SAXReader sr = new SAXReader();
     4:通过读取工具，读取xnml文档的输入流，并得到文档对象
        Document dc= sr.read(is);
    5:通过文档对象，得到xml的根节点
    Element root= dc.getRootElement
####Element常用的方法
一个Element就代表一个xml文档对象
     -获取节点名称
     String getName();

     -获取节点内容
    String getTest();

     -设置节点内容
      String setText();

      -获取所有的子节点
          List<Element> elements();

     -根据子节点名称，获取想对应的结点对象
      Element element("节点名称");

         -获取节点的属性值
         String attributeValue("属性名称")

eg:

     public static void main(String[] args) throws Exception {
		//1.	创建一个指向XML文档的输入流
			InputStream is = new FileInputStream("C:\\code\\34\\code1\\day04_XML\\src\\books.xml");
		//2.	创建XML读取工具
			SAXReader sr = new SAXReader();
		//3.	通过读取工具, 读取XML文档的输入流 , 得到文档对象
			Document doc = sr.read(is);
		//4.	通过文档对象 , 获取根节点
			Element root = doc.getRootElement();
		//Element的常用操作
		//1.	获取根节点的所有子节点
			List<Element> es = root.elements();
		//2.	循环遍历集合
			for(Element book:es) {
				//3.	获取book节点的id属性值
				String idValue = book.attributeValue("id");
				//4.	获取book节点中的name子节点
				Element name = book.element("name");
				String nameValue = name.getText();
				//5.	获取book节点中的info子节点
				Element info = book.element("info");
				String infoValue = info.getText();
				System.out.println("图书id="+idValue+" , 图书名称="+nameValue+" , 图书详情="+infoValue);
			}}}



###XPATH解析：
     作用：通过路径快速的查找一个或一组元素
  

####路径表达式
       /  :从根节点开始查找
      //  :从当前节点查找后代元素
      .   :查找当前节点
     ..   :查找父节点
      @   ：选择属性
     


###### 使用方法
    方法一：
          用于一次性查找单个节点
    Node node=  doc.selectSingleNode("xpath表达式");
    方法二：
     用于一次查找一组元素
     Node node=  doc.selectNode("xpath表达式");
   