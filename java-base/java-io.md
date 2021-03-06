    “对语言设计人员来说，创建好的输入／输出系统是一项特别困难的任务。”――《Think in Java》
## IO的分类 ##
- Java IO一般包含两个部分：1.java.io包中堵塞型IO；2.java.nio包中的非堵塞型IO，通常称为New IO。
- Java New IO的非堵塞技术主要采用了Observer模式，就是有一个具体的观察者和＝监测IO端口，如果有数据进入就会立即通知相应的应用程序。这样我们就避免建立多个线程，同时也避免了read等待的时间
- Java的IO主要包含三个部分：
	- 1.流式部分――IO的主体部分；
	- 2.非流式部分――主要包含一些辅助流式部分的类，如：File类、RandomAccessFile类和FileDescriptor等类；
	- 3.文件读取部分的与安全相关的类，如：SerializablePermission类。以及与本地操作系统相关的文件系统的类，如：FileSystem类和Win32FileSystem类和WinNTFileSystem类。

## IO中的流 ##
- “One dimension , one direction .” 即流是一维的，同时流是单向的
- IO中的输入字节流
	- InputStream是所有的输入字节流的父类，它是一个抽象类
	- ByteArrayInputStream、StringBufferInputStream、FileInputStream是三种基本的介质流，它们分别将Byte数组、StringBuffer、和本地文件中读取数据。PipedInputStream是从与其它线程共用的管道中读取数据，
	- ObjectInputStream和所有FilterInputStream的子类都是装饰流（装饰器模式的主角）。
	- 基本输入字节流:
		- ByteArrayInputStream:将内存中的Byte数组适配为一个InputStream。从内存中的Byte数组创建该对象。使用：一般作为数据源，会使用其它装饰流提供额外的功能，一般都建议加个缓冲功能。
		- StringBufferInputStream:将内存中的字符串适配为一个InputStream。从一个String对象创建该对象。底层的实现使用StringBuffer。该类被Deprecated。主要原因是StringBuffer不应该属于字节流，所以推荐使用StringReader。使用：一般作为数据源，同样会使用其它装饰器提供额外的功能。
		- FileInputStream：最基本的文件输入流。主要用于从文件中读取信息。通过一个代表文件路径的 String、File对象或者 FileDescriptor对象创建。使用：一般作为数据源，同样会使用其它装饰器提供额外的功能。
		- PipedInputStream：读取从对应PipedOutputStream写入的数据。在流中实现了管道的概念。利用对应的PipedOutputStream创建。使用：在多线程程序中作为数据源，同样会使用其它装饰器提供额外的功能。
		- SequenceInputStream:将2个或者多个InputStream 对象转变为一个InputStream。使用两个InputStream 或者内部对象为InputStream 的Enumeration对象创建该对象。使用：一般作为数据源，同样会使用其它装饰器提供额外的功能。
		- FilterInputStream：给其它被装饰对象提供额外功能的抽象类。主要子类见下：
			- DataInputStream：一般和DataOutputStream配对使用,完成基本数据类型的读写。利用一个InputStream构造。使用：提供了大量的读取基本数据类新的读取方法。
			- BufferedInputStream：使用该对象阻止每次读取一个字节都会频繁操作IO。将字节读取一个缓存区，从缓存区读取。利用一个InputStream、或者带上一个自定义的缓存区的大小构造。使用：使用InputStream的方法读取，只是背后多一个缓存的功能。设计模式中透明装饰器的应用。
			- LineNumberInputStream：跟踪输入流中的行号。可以调用getLineNumber( )和 setLineNumber(int)方法得到和设置行号。利用一个InputStream构造。使用：紧紧增加一个行号。可以象使用其它InputStream一样使用。
			- PushbackInputStream：可以在读取最后一个byte 后将其放回到缓存中。利用一个InputStream构造。使用：一般仅仅会在设计compiler的scanner 时会用到这个类。在我们的java语言的编译器中使用它。很多程序员可能一辈子都不需要。
- IO中的输出字节流
	- OutputStream是所有的输出字节流的父类，它是一个抽象类。
	- ByteArrayOutputStream、FileOutputStream是两种基本的介质流，它们分别向Byte数组、和本地文件中写入数据。PipedOutputStream是向与其它线程共用的管道中写入数据
	- ObjectOutputStream和所有FilterOutputStream的子类都是装饰流
	- 基本输出字节流	
		- ByteArrayOutputStream:在内存中创建一个buffer。所有写入此流中的数据都被放入到此buffer中。无参或者使用一个可选的初始化buffer的大小的参数构造。使用：一般将其和FilterOutputStream套接得到额外的功能。建议首先和BufferedOutputStream套接实现缓冲功能。通过toByteArray方法可以得到流中的数据。（不通明装饰器的用法）
		- FileOutputStream：将信息写入文件中。使用代表文件路径的String、File对象或者 FileDescriptor对象创建。还可以加一个代表写入的方式是否为append的标记。使用：一般将其和FilterOutputStream套接得到额外的功能。
		- PipedOutputStream：任何写入此对象的信息都被放入对应PipedInputStream 对象的缓存中，从而完成线程的通信，实现了“管道”的概念。具体在后面详细讲解。利用PipedInputStream构造，使用：在多线程程序中数据的目的地的。一般将其和FilterOutputStream套接得到额外的功能。
		- FilterOutputStream：实现装饰器功能的抽象类。为其它OutputStream对象增加额外的功能。装饰输出字节流：
			- DataOutputStream：通常和DataInputStream配合使用，使用它可以写入基本数据类新。使用OutputStream构造。使用：包含大量的写入基本数据类型的方法。
			- PrintStream：产生具有格式的输出信息。（一般地在java程序中DataOutputStream用于数据的存储，即J2EE中持久层完成的功能，PrintStream完成显示的功能，类似于J2EE中表现层的功能）。使用OutputStream和一个可选的表示缓存是否在每次换行时是否flush的标记构造。还提供很多和文件相关的构造方法。使用：一般是一个终极（“final”）的包装器，很多时候我们都使用它！
			- BufferedOutputStream：使用它可以避免频繁地向IO写入数据，数据一般都写入一个缓存区，在调用flush方法后会清空缓存、一次完成数据的写入。从一个OutputStream或者和一个代表缓存区大小的可选参数构造。使用：提供和其它OutputStream一致的接口，只是内部提供一个缓存的功能。
- 字节流与字符流
	- Sun为什么在Java 1.1里添加了Reader和Writer层次？最重要的原因便是国际化（Internationalization――i18n）的需求。老式IO流层次结构只支持8位字节流，不能很好地控制16位的Unicode字符。Java本身支持Unicode，Sun又一致吹嘘其支持Unicode，因此有必要实现一个支持Unicode的流的层次结构，所以出现了Reader和Writer层次，以提供对所有IO操作中的Unicode的支持。除此之外，新库也对速度进行了优化，可比旧库更快地运行。
	- 8位的字节流和16位的字符流的对应关系，可以从ByteInputStream/ByteOutputStream与CharArrayInputStream/CharArrayOutputStream的对应关系中看出端倪。（还没看出来啊！赶紧去看看Java的基本数据类型）。
- IO中的输入字符流
	- Reader是所有的输入字符流的父类，它是一个抽象类
	- CharReader、StringReader是两种基本的介质流，它们分别将Char数组、String中读取数据。PipedReader是从与其它线程共用的管道中读取数据。
	- BufferedReader很明显就是一个装饰器，它和其子类负责装饰其它Reader对象。
	- FilterReader是所有自定义具体装饰流的父类，其子类PushbackReader对Reader对象进行装饰，会增加一个行号。
	- InputStreamReader是一个连接字节流和字符流的桥梁，它将字节流转变为字符流。FileReader可以说是一个达到此功能、常用的工具类，在其源代码中明显使用了将FileInputStream转变为Reader的方法。我们可以从这个类中得到一定的技巧。
	- Reader中各个类的用途和使用方法基本和InputStream中的类使用一致。
- IO中的输出字符流
	- Writer是所有的输出字符流的父类，它是一个抽象类。
	- CharArrayWriter、StringWriter是两种基本的介质流，它们分别向Char数组、String中写入数据。PipedWriter是向与其它线程共用的管道中写入数据。
	- BufferedWriter是一个装饰器为Writer提供缓冲功能。
	- PrintWriter和PrintStream极其类似，功能和使用也非常相似。
	- OutputStreamWriter是OutputStream到Writer转换的桥梁，它的子类FileWriter其实就是一个实现此功能的具体类（具体可以研究一下Source Code）。功能和使用和OutputStream极其类似

