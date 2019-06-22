## JAVA Socket编程

​	首先上图,

![1561016731829](C:\Users\mayn\AppData\Roaming\Typora\typora-user-images\1561016731829.png)



多对多聊天

客户端  

```java
/**
 * 客户端程序
 */
public class Client {


    public static void main(String[] args) throws IOException{
        Socket client = new Socket("localhost",9999);
		
        //创建发送线程的对象

        Send send = new Send(client);
        //创建接受线程的对象
        Receive receive = new Receive(client);
        new Thread(send).start();
        new Thread(receive).start();
    }
}

```

客户端发送线程 :首先我们是发送,所以我们必须得到客户端输出流对象,

```java
/**
 * 客户端发送线程
 */
public class Send implements Runnable{


    //监听键盘所输入的数据
    private BufferedReader br = null;
    //发送数据流
    private DataOutputStream dos = null;

    //状态标识位
    boolean flage = true;

    public Send(){
        //首先监听键盘事件
        br = new BufferedReader(new InputStreamReader(System.in));
    }
    public Send(Socket client){

        this();
        try{

            dos = new DataOutputStream(client.getOutputStream());
        }catch (IOException e){
            e.printStackTrace();
            CloseUtils.closeAll(br,client,dos);
        }

    }
    //得到键盘输入的对象
    public String getMessage() {
        String str = null;
        try {
            str = br.readLine();
        } catch (IOException e) {
            e.printStackTrace();
            CloseUtils.closeAll(br,dos);
        }
        return str;
    }
    //run方法,线程,
    @Override
    public void run() {

        try {
            //如果没有出什么异常,我们就把键盘读到的数据通过io流输出出去
            while(flage){
                String message = getMessage();
                dos.writeUTF(message);
            }
        } catch (IOException e) {
            e.printStackTrace();
        }

    }
}

```

客户端接收线程:必须得到Socket 的输入流对象,才能进行读取

```java
public class Receive implements Runnable {
    //数据接收流
    private DataInputStream dis = null;
    private boolean flage = true;

    public Receive(Socket client){

        try{

            dis = new DataInputStream(client.getInputStream());
        }catch (IOException e){
            flage = false;
            e.printStackTrace();
            CloseUtils.closeAll(dis,client);

        }
    }

    public String getMessage(){
        String str = "";
        try {
          str = dis.readUTF();

          return str;
        } catch (IOException e) {
            e.printStackTrace();
            CloseUtils.closeAll(dis);
        }
        return str;
    }
    @Override
    public void run() {

        while (flage){

            System.out.println(this.getMessage());
        }

    }
}

```

然后是服务端

每加入一个客户端,我们都会创建一个通道线程和该客户端进行连接,接收并发送该客户端的信息

```java
public class BidConset {

    //每连接一个客户端,服务端都会创建一个新的MyChannle对象进行管理
    /**
     * 记录聊天室中的所有客户端,
     */
    public static List<Mychannle> list = new ArrayList<>();

}

```



我们服务端需要接收用户的信息并且返回给其他用户,



服务端:

```java
/**
 * 服务端程序
 */
public class Server {

    //创建集合对象,存储所有连接到该服务器的客户端

    public static void main(String[] args) throws IOException{

        List<Mychannle> list = new ArrayList<>();
        ServerSocket server = new ServerSocket(9999);



        while(true){
            //接收客户端的连接
           Socket client = server.accept();
            System.out.println("有新的客户连接进来>>>>");
           //然后创建一个客户端连接线程,用于接收和发送客户端消息
           Mychannle mychannle = new Mychannle(client);
            //每加入一个客户端,我们就把该客户端加入进来

            BidConset.list.add(mychannle);
           //每加入一个客户端,都创建一个新的线程类,然后去接收客户端发送过来的消息,并接收
           new Thread(mychannle).start();

        }



    }
```

每个连接一个客户端,服务端都需要去监听,也就是去创建一个线程,去接收客户端发送的消息,并且发送给其他客户端

```java

/**
 * 服务器的发送,也就是接收所有的服务端数据,然后分别发送给其他客户端,聊天室作用
 */
public class Mychannle implements Runnable{

    private DataOutputStream dos;
    private DataInputStream dis;

    private boolean flage = true;

    public Mychannle(Socket client){

        try{

            dis = new DataInputStream(client.getInputStream());
            dos = new DataOutputStream(client.getOutputStream());
        }catch (IOException e){
            e.printStackTrace();
            //出错之后不能够关闭流,而是移除客户端,移除当前客户端

            flage = false;
            BidConset.list.remove(this);
        }

    }
    public void send(String str){
        //当发送的数据不为空
        if(str!=null&&str.length()!=0){
        try {

            dos.writeUTF(str);
            dos.flush();
        } catch (IOException e) {
            e.printStackTrace();
            flage = false;

            BidConset.list.remove(this);
        }
        }
    }
    //接收服务端的消息
    public String receive(){
        String str = "";
        try {
            str = dis.readUTF();

            return str;
        } catch (IOException e) {
            e.printStackTrace();
            flage = false;

            BidConset.list.remove(this);

        }
        return str;
    }
    //我们客户端发送一条消息,我们服务端又把消息推送给其他客户端
    public void sendOthers(){
        //接收当前客户端所发送的数据
        String str = this.receive();
        System.out.println(str);
        //遍历所有客户端,然后一个客户端发送消息,所有客户端都可以收到,不包括自己
        List<Mychannle>  list = BidConset.list;
        System.out.println(list.size());
        for(Mychannle c : list){

            if(c == this){
                continue;
            }
            c.send(str);
        }
    }
    //把服务端发送过来的消息发送出去
    @Override
    public void run() {

        while (flage){


            sendOthers();
        }
    }
}

```

**如果要实现单对单,多对多,而且还必须有不同的聊天室的话,我们可以使用Map,把所有创建的客户端进行分组,map的key存储聊天室的唯一标示,而value则存储加入该聊天室中监听客户端的线程MyChannel**

```java
Map<String,List<MyChannels>>  chatroom;
```

==**然后每次发送或者接受前,我们需要验证要发送到那个聊天室去,然后通知给哪些客户端接收消息.**==

==**问题:如果有一个客户端断开连接,我们怎么保证服务端不会发送内容给他,避免出现IOException,**==

​	==**如何验证群号,并发送信息给在聊天室中所有客户端,客户端每次发送消息都需要带上聊天室唯一标示,然后服务器端的监听线程才能够根据唯一标示来发送指定聊天室中的客户端来接收该消息.**==

​	

