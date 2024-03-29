```
import java.io.IOException;
import java.net.InetSocketAddress;

import java.nio.ByteBuffer;
import java.nio.channels.*;
import java.nio.charset.Charset;
import java.util.Iterator;
import java.util.Set;

//NIO服务器端
public class NioServer {
    //启动
      public void start() throws IOException {
            //1.创建Selector
          Selector selector=Selector.open();
          //2.通过ServerSocketChannel创建channel通道
          ServerSocketChannel serverSocketChannel=ServerSocketChannel.open();
          //3.为channel通道绑定监听端口
          serverSocketChannel.bind(new InetSocketAddress(8000));
          //4.为channel为非阻塞模式
          serverSocketChannel.configureBlocking(false);
          //5.将channel注册到selector上，监听连接事件
          serverSocketChannel.register(selector, SelectionKey.OP_ACCEPT);
          System.out.println("服务器启动成功");
          //6.调用selectKeys方法获取就绪channel集合
          for(;;){
              //获取可用channel数量
              int readyChannels=selector.select();

              if(readyChannels==0)continue;
              //获取可channel的集合
              Set<SelectionKey> selectionKeys=selector.selectedKeys();

              Iterator iterator=selectionKeys.iterator();

              while(iterator.hasNext()){
                 //selectionKey实例
                 SelectionKey selectionKey=(SelectionKey)iterator.next();
                 //移除set中的selectionKey
                 iterator.remove();
         // 7.判断就绪事件种类，调用业务处理方法
                 //如果是接入事件
                 if(selectionKey.isAcceptable()){
                     acceptHandler(serverSocketChannel,selector);
                 }
                // 如果是可读事件
                  if(selectionKey.isReadable()){
                      readHandle(selectionKey,selector);
                  }
              }

          }

          //8.根据业务需要决定是否再次注册监听事件，重复执行第三步操作
        }

        //接入事件处理器
        private void acceptHandler(ServerSocketChannel serverSocketChannel,Selector selector)throws IOException{
        //如果是接入事件，创建socketChannel
            SocketChannel socketChannel = serverSocketChannel.accept();
        //将socketChannel设置为非阻塞模式
           socketChannel.configureBlocking(false);
        //将channel注册到selector上，监听可读事件
           socketChannel.register(selector,SelectionKey.OP_READ);
        //回复客户端提示信息
            socketChannel.write(Charset.forName("UTF-8").encode("注意安全，不要相信任何陌生人"));
        }

        //可读事件处理器
        private void readHandle(SelectionKey selectionKey,Selector selector)throws IOException{
             //从selectionKey中获取到已经就绪的channel
            SocketChannel socketChannel =(SocketChannel)selectionKey.channel();
            //创建buffer
            ByteBuffer byteBuffer=ByteBuffer.allocate(1024);
            //循环读取客户端请求信息
            String request="";
            while(socketChannel.read(byteBuffer)>0){
                //切换buffer为读模式
                byteBuffer.flip();

                //读取buffer中的内容
                request+=Charset.forName("UTF-8").decode(byteBuffer);
            }
            //将channel再次注册到selector上，监听他的可读事件
            socketChannel.register(selector,SelectionKey.OP_READ);

            if(request.length()>0){
                //将客户端发送的请求信息广播给其他客户端
                //System.out.println("::"+request);
                broadCast(selector,socketChannel,request);
            }

        }

        //广播给其他客户端
    private void broadCast(Selector selector,
                           SocketChannel sourceChannel,String request){
          //获取所有已接入的客户端channel
           Set<SelectionKey> selectionKeySet=selector.keys();
           selectionKeySet.forEach(selectionKey -> {
               Channel targetChannel=selectionKey.channel();
               //剔除发消息的客户端
               if(targetChannel instanceof SocketChannel
                       &&targetChannel!=sourceChannel){
                   try {
                       //将信息发送到targetChannel客户端
                       ((SocketChannel)targetChannel).write(
                               Charset.forName("UTF-8").encode(request));
                   } catch (IOException e) {
                       e.printStackTrace();
                   }
               }
           });


        //循环向所有channel广播信息
    }
        //主方法
        public static void main(String[] args)throws IOException{
          NioServer nioServer=new NioServer();
          nioServer.start();
        }

 }
```