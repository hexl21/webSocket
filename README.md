# webSocket
用java做的webSocket握手协议

运行环境：
<br/>1 Tomcat必须7.0以上
<br/>2 JDK必须7.0以上

# Tomcat 7中的Websocket架构
因为Websocket通信分为握手和数据传输两个过程，两个过程中需要用到的处理方式是不一样的，握手过程是基于HTTP 1.1基础上的，而数据传输是直接基于TCP的流传输。
握手过程中，在HttpServletRequest的基础上，封装了WsHttpServletRequest类，添加了对Request的失效操作函数invalidate()。而在数据通信时，接受和处理数据过程中，基于org.apache.coyote.http11.upgrade.UpgradeInbound重新封装了用于处理数据输入流的类StreamInbound，并在StreamInbound的基础上扩展生成了用于消息处理的类MessageInbound。在这两个数据处理类中均留有onData，onTextData/onBinaryData，onOpen，onClose等事件操作函数接口，这些接口将在载入的代码类中实现业务逻辑。在用于数据输出流的类WsOutbound则是封装了UpgradeOutbound对象实例，基于UpgradeOutbound对象的基础上，添加了websocket响应有关的处理逻辑。这里处理函数均为同步调用的函数，保证websocket响应的时序性。

#项目Demo需求:
定时向所有在线用户推送一个广告或是推送一个通知之类的(比如服务器升级，请保存好手头工作之类的)。

#代码实现
<br/><b>1.先用MyEclipse/Eclipse引入该项目。</b>
<br/>注意要修改下面2个文件(index.html/webSocket.js)，将IP地址修改为自己的。或者修改为localhost也行。
<br/><b>2.运行WebSocket项目。注意是否会有报错。</b>
<br/><b>3.在需要接收推送信息的项目头部引入js的url</b>
<br/>例如：src="XXXXXX:8080/webSocket/webSocket.js"
即可实现。

#已解决以及未解决的问题
<br/>已解决的：实现基本的握手协议的demo。
<br/>未解决的：
<br/>1.目前是所有的会话都自动发送信息，实际上很多时候的需求是对特定用户发送消息，所以需要一个会话和用户的绑定的逻辑。
<br/>2.在线用户和非在线用户接收信息有所不一样。非在线用户可能会miss一些消息。
<br/>所以，需要设计一个信息信箱功能，让所有的信息都先发送到信箱里面；如果是在线用户，则弹出信息；非在线用户则保留在信息里面，他登录之后可以自己查看！
