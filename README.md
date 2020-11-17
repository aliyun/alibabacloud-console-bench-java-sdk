<p align="center">
<a href=" https://www.alibabacloud.com"><img src="https://aliyunsdk-pages.alicdn.com/icons/Aliyun.svg"></a>
</p>

<h1 align="center"> 企业工作台 Java SDK </h1>

## 基本原理

在官网 [SDK](https://github.com/aliyun/aliyun-openapi-java-sdk) 的基础上，对 Client进行重写，满足企业工作台的调用逻辑，同时完全兼容官网
SDK，这样就形成了 企业工作台定制 Client + 官网 SDK 提供 APIMETA 的模式。

## 环境要求

- Java 需要1.6以上的JDK
- 找阿里云企业工作台团队，提供 OpenAPI 访问凭证(`consoleKey`、`consoleSecret`)

## 安装依赖

```xml
<dependency>
    <groupId>com.aliyun</groupId>
    <artifactId>aliyun-java-sdk-console-bench</artifactId>
    <version>1.0.0</version>
</dependency>
```

## 快速使用

企业工作台的[业务模式](https://help.aliyun.com/product/144772.html)分为 工作台托管、聚石塔自管 两种模式，因此API调用也有针对性区分。

### 工作台托管 SDK 调用示例

```java
public class Demo {
    public static void main(String[] args) {
        // 企业工作台定制SDK
        DefaultProfile profile = DefaultProfile.getProfile(${regionId}, ${consoleKey}, ${consoleSecret}); 
        ConsoleAcsClient client = new ConsoleAcsClient(profile);
        client.setEndpoint("console-work.aliyuncs.com");
        client.addQueryParam("AliUid", "xxx");  // OneConsole传递的主账号id
        client.addQueryParam("Mode", "online"); // Mode: online为在线调用，否则为离线调用
        
        // 创建API请求并设置参数
        DescribeInstancesRequest request = new DescribeInstancesRequest();
        request.setVpcId("xxx");
        try {
            DescribeInstancesResponse response = client.getAcsResponse(request);
            System.out.println("response: " + response);
        } catch (ClientException e) {
            e.printStackTrace();
        }
    }
}
```

说明：

- endpoint: 测试环境下需要 host 绑定 `114.55.202.134 console-work.aliyuncs.com`

## 聚石塔托管 SDK 调用示例

```java
public class Demo {
    public static void main(String[] args) {
        // 企业工作台定制SDK
        DefaultProfile profile = DefaultProfile.getProfile(${regionId}, ${consoleKey}, ${consoleSecret}); 
        ConsoleAcsClient client = new ConsoleAcsClient(profile);
        client.setEndpoint("console-work.aliyuncs.com");
        client.addQueryParam("IdToken", "xxx");  // Oauth授权后获取的身份信息
        
        // 创建API请求并设置参数
        DescribeInstancesRequest request = new DescribeInstancesRequest();
        request.setVpcId("xxx");
        try {
            DescribeInstancesResponse response = client.getAcsResponse(request);
            System.out.println("response: " + response);
        } catch (ClientException e) {
            e.printStackTrace();
        }
    }
}
```

说明:

- endpoint: 测试环境下需要 host 绑定 `114.55.202.134 console-work.aliyuncs.com`

## 许可证
[Apache-2.0](http://www.apache.org/licenses/LICENSE-2.0)

Copyright (c) 2009-present, Alibaba Cloud All rights reserved.