## grpc-java接入流程(基于spring-boot)

#### 服务提供方：

1，添加依赖：

```
<dependencies>
        <dependency>
            <groupId>net.devh</groupId>
            <artifactId>grpc-server-spring-boot-starter</artifactId>
            <version>1.4.1.RELEASE</version>
        </dependency>
</dependencies>
```

2，添加protobuf-maven-plugin插件：

```
<build>
        <extensions>
            <extension>
                <groupId>kr.motd.maven</groupId>
                <artifactId>os-maven-plugin</artifactId>
                <version>1.5.0.Final</version>
            </extension>
        </extensions>
        <plugins>
            <plugin>
                <groupId>org.xolstice.maven.plugins</groupId>
                <artifactId>protobuf-maven-plugin</artifactId>
                <version>0.5.1</version>
                <configuration>
                    <protocArtifact>com.google.protobuf:protoc:3.5.1-1:exe:${os.detected.classifier}</protocArtifact>
                    <pluginId>grpc-java</pluginId>
                    <pluginArtifact>io.grpc:protoc-gen-grpc-java:1.13.1:exe:${os.detected.classifier}</pluginArtifact>
                </configuration>
                <executions>
                    <execution>
                        <goals>
                            <goal>compile</goal>
                            <goal>compile-custom</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
 </build>
```

3，在main包下面新增proto包，新建service.proto文件，编写接口文档：

```
syntax = "proto3";

option java_package = "com.innapp.base";
import "google/protobuf/wrappers.proto"; // 基础类型
option java_multiple_files = true;

package base; // 指定当前包名防止冲突，建议都加上

service ConstantService {
    // 通过group,key获取常量值
    rpc getConstantByGroupKey (ConstantQuery) returns (google.protobuf.StringValue) {
    }
    // 通过group获取常量列表
    rpc getConstantsByGroup (google.protobuf.StringValue) returns (Constants) {
    }
}

// 常量值列表
message Constants {
    repeated Constant value = 1;
}

// 常量值
message Constant {
    string group = 2;
    string key = 3;
    string value = 4;
}

// 首页模块
message ConstantQuery {
    string group = 1; // 分组
    string key = 2; //key值
}
```

4，运行maven clear && install，protobuf-maven-plugin会帮我们自动生成需要的代码：

5，编写服务端: 使用 @GrpcService注入服务，可以指定服务名称和拦截器

```
@GrpcService(value = ConstantServiceGrpc.class, interceptors = {PlatformInterceptor.class})
public class ConstantServiceGrpcImpl extends ConstantServiceGrpc.ConstantServiceImplBase {

    @Resource
    private ConstantService constantService;

    @Override
    public void getConstantByGroupKey(ConstantQuery request, StreamObserver<StringValue> responseObserver) {
        responseObserver.onNext(StringValue.of(constantService.getStrByBizAndKey(request.getGroup(), request.getKey())));
        responseObserver.onCompleted();
    }

    @Override
    public void getConstantsByGroup(StringValue request, StreamObserver<Constants> responseObserver) {
        responseObserver.onNext(Constants.newBuilder().addAllValue(GrpcUtils.coverToGrpcList(constantService.listByBiz(request.getValue()))).build());
        responseObserver.onCompleted();
    }
}
```

6，启动服务：

```
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }

}
```

```
gRPC Server started, listening on address: 0.0.0.0, port: 9090 服务默认端口9090
```



#### 服务消费方：

1，添加依赖：

```
<dependencies>
        <dependency>
            <groupId>net.devh</groupId>
            <artifactId>grpc-client-spring-boot-starter</artifactId>
            <version>1.4.1.RELEASE</version>
        </dependency>
</dependencies>
```

2，添加protobuf-maven-plugin插件：

```
<build>
        <extensions>
            <extension>
                <groupId>kr.motd.maven</groupId>
                <artifactId>os-maven-plugin</artifactId>
                <version>1.5.0.Final</version>
            </extension>
        </extensions>
        <plugins>
            <plugin>
                <groupId>org.xolstice.maven.plugins</groupId>
                <artifactId>protobuf-maven-plugin</artifactId>
                <version>0.5.1</version>
                <configuration>
                    <protocArtifact>com.google.protobuf:protoc:3.5.1-1:exe:${os.detected.classifier}</protocArtifact>
                    <pluginId>grpc-java</pluginId>
                    <pluginArtifact>io.grpc:protoc-gen-grpc-java:1.13.1:exe:${os.detected.classifier}</pluginArtifact>
                </configuration>
                <executions>
                    <execution>
                        <goals>
                            <goal>compile</goal>
                            <goal>compile-custom</goal>
                        </goals>
                    </execution>
                </executions>
            </plugin>
        </plugins>
 </build>
```

3，在main包下面新增proto包，新建service.proto文件，编写接口文档：

```
syntax = "proto3";

option java_package = "com.innapp.base";
import "google/protobuf/wrappers.proto"; // 基础类型
option java_multiple_files = true;

package base; // 指定当前包名防止冲突，建议都加上

service ConstantService {
    // 通过group,key获取常量值
    rpc getConstantByGroupKey (ConstantQuery) returns (google.protobuf.StringValue) {
    }
    // 通过group获取常量列表
    rpc getConstantsByGroup (google.protobuf.StringValue) returns (Constants) {
    }
}

// 常量值列表
message Constants {
    repeated Constant value = 1;
}

// 常量值
message Constant {
    string group = 2;
    string key = 3;
    string value = 4;
}

// 首页模块
message ConstantQuery {
    string group = 1; // 分组
    string key = 2; //key值
}
```

4，运行maven clear && install，protobuf-maven-plugin会帮我们自动生成需要的代码：

5，配置文件新增连接grpc服务的配置：

```
grpc:
  client:
    medical-basic-server:  ## 服务的名称
      host: medical-service-test.medical-service  ## 服务的连接地址(k8s地址)
#      host: 192.168.2.230
      port: 9090  ## 服务端口
```

6，增加grpc的config：

```
@Configuration
@EnableAsync // 同步顺序注入
public class GrpcConfig {
    
    @Value("${grpc.client.medical-basic-server.host}")
    private String grpcServiceHost;
    
    @Value("${grpc.client.medical-basic-server.port}")
    private String grpcServicePort;
    
    @Bean
    public Channel getChannel() {
        ManagedChannel channel = ManagedChannelBuilder.forAddress(grpcServiceHost, Integer.parseInt(grpcServicePort)).usePlaintext().build();
        return ClientInterceptors.intercept(channel, new MyClientInterceptor());
    }
    
    @Bean
    public ConstantServiceGrpc.ConstantServiceBlockingStub constantServiceBlockingStub() {
        return ConstantServiceGrpc.newBlockingStub(getChannel());
    }
     
}
```

7，引入服务：

```
@Service
public class ConstantServiceImpl {
    
    @Resource
    private ConstantServiceGrpc.ConstantServiceBlockingStub constantServiceBlockingStub;
    
    /**
     * 获取常量值
     *
     * @param group
     * @param key
     * @return
     */
    @Override
    public String selectByGroupKey(String group, String key) {
        ConstantQuery request = ConstantQuery.newBuilder().setGroup(group).setKey(key).build();
        return StringValue.newBuilder(constantServiceBlockingStub.getConstantByGroupKey(request)).getValue();
    }

    /**
     * 获取常量值（若无返回默认值）
     *
     * @param group
     * @param key
     * @param defaultValue
     * @return
     */
    @Override
    public String selectByGroupKey(String group, String key, String defaultValue) {
        ConstantQuery request = ConstantQuery.newBuilder().setGroup(group).setKey(key).build();
        String value = StringValue.newBuilder(constantServiceBlockingStub.getConstantByGroupKey(request)).getValue();
        return StringUtils.isBlank(value) ? defaultValue : value;
    }
}
```

```
@RestController
@RequestMapping
public class IndexController {
    
    @Resource
    private ConstantServiceImpl constantServiceImpl;
    
    // 测试grpc
    @GetMapping(value = "test_grpc", produces = MediaType.APPLICATION_JSON_VALUE)
    public Result testGrpc(String group,String key,HttpServletRequest request) {
        return Result.success(constantServiceImpl.selectByGroupKey(String group, String key));
    }   
}
```

8，启动客户端：

```
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```



### 联调测试

```
curl -X GET "http://127.0.0.1:8080/test_grpc"
```



## protobuf基本语法：

参考地址：http://colobu.com/2017/03/16/Protobuf3-language-guide/#%E6%8C%87%E5%AE%9A%E5%AD%97%E6%AE%B5%E7%B1%BB%E5%9E%8B 

