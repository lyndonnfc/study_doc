## **Swagger2注解信息介绍**
- @Api()用于类； 
表示标识这个类是swagger的资源 

- @ApiOperation()用于方法； 
表示一个http请求的操作 

- @ApiParam()用于方法，参数，字段说明； 
表示对参数的添加元数据（说明或是否必填等）

- @ApiModel()用于类 
表示对类进行说明，用于参数用实体类接收 

- @ApiModelProperty()用于方法，字段 
表示对model属性的说明或者数据操作更改 

- @ApiIgnore()用于类，方法，方法参数 
表示这个方法或者类被忽略 

- @ApiImplicitParam() 用于方法 
表示单独的请求参数 

- @ApiImplicitParams() 用于方法，包含多个 @ApiImplicitParam

---

## **Controller使用示例**

```
@Api(tags = {"Order"}, description = "订单相关")
public class OrderController {
    
}

```

---

## **API使用示例**

GET 请求
```
@Api(tags = {"Order"}, description = "订单相关")
public class OrderController {
    
    
    @ApiOperation(value = "小程序-订单详情",notes = "酒店订单详情")
    @ApiImplicitParams({@ApiImplicitParam(name = "bookProcessId",value = "主单id",dataType = "Long")})
    public Result detail(Long bookProcessId, HttpServletRequest request) {

    }
}

```


POST 请求
```

    @ApiOperation(value = "小程序-创建订单",notes = "酒店创建订单接口")
    public Result create( @RequestBody @ApiParam(name = "creation",value = "创建订单参数",required = true) OrderCreation creation, HttpServletRequest request) throws Exception {
        
    }

```

---

## **module使用示例**

```
@ApiModel(value = "创建订单参数信息")
public class OrderCreation {

    @ApiModelProperty(value = "入住日期")
    private LocalDate checkInDate;

    @ApiModelProperty(value = "离店日期")
    private LocalDate checkOutDate;

    @ApiModelProperty(value = "酒店hid")
    private String hid;

    @ApiModelProperty(value = "内部房型code")
    private String innerRoomCode;

    @ApiModelProperty(value = "到店时间")
    private LocalDateTime arrivalTime;

    @ApiModelProperty(value = "入住日期")
    private String paymentType;

    @ApiModelProperty(value = "供应方式")
    private String supplyType;

    @ApiModelProperty(value = "货架id")
    private Long shelvesId;

    @ApiModelProperty(value = "价格计划id")
    private String ratePlanId;

}

```
