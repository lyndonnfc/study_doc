## grpc基础知识

### grpc简介：

 GRPC是google开源的一个高性能、跨语言的RPC框架，基于HTTP2协议，基于protobuf 3.x，基于Netty 4.x +。GRPC与thrift、avro-rpc等其实在总体原理上并没有太大的区别，简而言之GRPC并没有太多突破性的创新。（如下描述，均基于JAVA语言的实现）

​    对于开发者而言：

    -  需要使用protobuf定义接口，即.proto文件
-   然后使用compile工具生成特定语言的执行代码，比如JAVA、C/C++、Python等。类似于thrift，为了解决跨语言问题。
-  启动一个Server端，server端通过侦听指定的port，来等待Client链接请求，通常使用Netty来构建，GRPC内置了Netty的支持。
-  启动一个或者多个Client端，Client也是基于Netty，Client通过与Server建立TCP长链接，并发送请求；Request与Response均被封装成HTTP2的stream Frame，通过Netty Channel进行交互。



### 原理解析：

 	GRPC的Client与Server，均通过Netty Channel作为数据通信，序列化、反序列化则使用Protobuf，每个请求都将被封装成HTTP2的Stream，在整个生命周期中，客户端Channel应该保持长连接，而不是每次调用重新创建Channel、响应结束后关闭Channel（即短连接、交互式的RPC），目的就是达到链接的复用，进而提高交互效率。

#### gRPC的客户端调用流程如下：

1) 客户端Stub(GreeterBlockingStub)调用sayHello(request)，发起RPC调用

2) 通过DnsNameResolver进行域名解析，获取服务端的地址信息（列表），随后使用默认的LoadBalancer策略，选择一个具体的gRPC服务端实例

3) 如果与路由选中的服务端之间没有可用的连接，则创建NettyClientTransport和NettyClientHandler，发起HTTP/2连接

4) 对请求消息使用PB（Protobuf）做序列化，通过HTTP/2 Stream发送给gRPC服务端

5) 接收到服务端响应之后，使用PB（Protobuf）做反序列化

6) 回调GrpcFuture的set(Response)方法，唤醒阻塞的客户端调用线程，获取RPC响应

需要指出的是，客户端同步阻塞RPC调用阻塞的是调用方线程（通常是业务线程），底层Transport的I/O线程（Netty的NioEventLoop）仍然是非阻塞的。

ManagedChannel是对Transport层SocketChannel的抽象，Transport层负责协议消息的序列化和反序列化，以及协议消息的发送和读取。ManagedChannel将处理后的请求和响应传递给与之相关联的ClientCall进行上层处理，同时，ManagedChannel提供了对Channel的生命周期管理（链路创建、空闲、关闭等）。

ManagedChannel提供了接口式的切面ClientInterceptor，它可以拦截RPC客户端调用，注入扩展点，以及功能定制，方便框架的使用者对gRPC进行功能扩展。

ManagedChannel的主要实现类ManagedChannelImpl创建流程如下：













