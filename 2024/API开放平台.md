# API开放平台

---

title: API开放平台

date: 2024-8-5

category: 开发记录

tags:

---

项目地址：

* 前端：https://github.com/ye-guo/yeguo-api-frontend
* 后端：https://github.com/ye-guo/yeguo-api-backend

## 一、技术选型

### 前端：

* Ant Design Pro 6.0.0
* Ant Design 5.20
* React 18
* TypeScript 5.4.3
* UmiJS 4.3.11 

### 后端

* JDK17
* Spring
* SpringMVC
* SpringBoot 3.3.0
* Mybatis-Plus 3.5.5
* SpringCloud Gateway 4.1.4
* Redis 3.2.12
* Hutool 5.8.27
* mysql 8.0.12
* dubbo 3.2.12
* nacos 2.3.2
* maven 3.9.7
* Swagger + Knife4j 接口文档

## 二、数据库表

### 用户表

```mysql
CREATE TABLE IF NOT EXISTS `user`
(
    `id`            bigint(20) unsigned                  NOT NULL AUTO_INCREMENT COMMENT '主键',
    `username`      varchar(128) COLLATE utf8_unicode_ci                     DEFAULT NULL COMMENT '用户名',
    `user_account`  varchar(256) COLLATE utf8_unicode_ci                     DEFAULT NULL COMMENT '用户账号',
    `user_password` varchar(512) CHARACTER SET utf8 COLLATE utf8_unicode_ci  DEFAULT NULL COMMENT '用户密码',
    `avatar_url`    varchar(1024) CHARACTER SET utf8 COLLATE utf8_unicode_ci DEFAULT 'https://cdn.jsdelivr.net/gh/ye-guo/Images/images/2.jpg' COMMENT '头像',
    `gender`        tinyint(3) unsigned                                      DEFAULT NULL COMMENT '性别',
    `phone`         varchar(128) COLLATE utf8_unicode_ci                     DEFAULT NULL COMMENT '电话',
    `email`         varchar(512) COLLATE utf8_unicode_ci                     DEFAULT NULL COMMENT '邮箱',
    `gold_coin`     bigint(20) unsigned                  NOT NULL            DEFAULT '100' COMMENT '金币',
    `access_key`    varchar(512) COLLATE utf8_unicode_ci NOT NULL COMMENT 'accessKey',
    `secret_key`    varchar(512) COLLATE utf8_unicode_ci NOT NULL COMMENT 'secretKey',
    `user_status`   tinyint(3) unsigned                  NOT NULL            DEFAULT '0' COMMENT '用户状态 0-正常',
    `user_role`     tinyint(3) unsigned                  NOT NULL            DEFAULT '0' COMMENT '用户角色 0-普通用户 1-管理员',
    `create_time`   datetime                             NOT NULL            DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    `update_time`   datetime                             NOT NULL            DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
    `is_deleted`    tinyint(3) unsigned                  NOT NULL            DEFAULT '0' COMMENT '逻辑删除 0-未删除 1-删除',
    PRIMARY KEY (`id`)
) ENGINE = InnoDB
  AUTO_INCREMENT = 1
  DEFAULT CHARSET = utf8
  COLLATE = utf8_unicode_ci COMMENT ='用户表';
```

### 接口信息表

```mysql
CREATE TABLE IF NOT EXISTS `interface_info`
(
    `id`                  bigint(20) unsigned                                      NOT NULL AUTO_INCREMENT COMMENT '接口id主键',
    `user_id`             bigint(20) unsigned                                      NOT NULL COMMENT '接口发布人id',
    `name`                varchar(256) CHARACTER SET utf8 COLLATE utf8_unicode_ci           DEFAULT NULL COMMENT '接口名称',
    `description`         varchar(512) COLLATE utf8_unicode_ci                              DEFAULT NULL COMMENT '接口描述',
    `method`              varchar(128) CHARACTER SET utf8 COLLATE utf8_unicode_ci           DEFAULT NULL COMMENT '接口方法',
    `url`                 varchar(1024) CHARACTER SET utf8 COLLATE utf8_unicode_ci          DEFAULT NULL COMMENT '接口地址',
    `request_params`      text CHARACTER SET utf8 COLLATE utf8_unicode_ci COMMENT '请求参数',
    `response_params`     text CHARACTER SET utf8 COLLATE utf8_unicode_ci COMMENT '响应参数',
    `response_format`     varchar(128) CHARACTER SET utf8 COLLATE utf8_unicode_ci           DEFAULT 'JSON' COMMENT '响应格式',
    `request_example`     mediumtext CHARACTER SET utf8 COLLATE utf8_unicode_ci COMMENT '请求示例',
    `response_example`    mediumtext COLLATE utf8_unicode_ci COMMENT '响应示例',
    `interface_status`    tinyint(3) unsigned                                      NOT NULL DEFAULT '0' COMMENT '接口状态 0-关闭 1-开启',
    `invoking_count`      bigint(20) unsigned                                      NOT NULL DEFAULT '0' COMMENT '调用次数',
    `avatar_url`          varchar(1024) CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL DEFAULT 'https://cdn.jsdelivr.net/gh/ye-guo/Images/images/2.jpg' COMMENT '接口头像',
    `required_gold_coins` bigint(20) unsigned                                      NOT NULL DEFAULT '1' COMMENT '调用一次所需金币',
    `request_header`      text CHARACTER SET utf8 COLLATE utf8_unicode_ci COMMENT '请求头',
    `response_header`     text CHARACTER SET utf8 COLLATE utf8_unicode_ci COMMENT '响应头',
    `create_time`         datetime                                                 NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    `update_time`         datetime                                                 NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
    `is_deleted`          tinyint(3) unsigned                                      NOT NULL DEFAULT '0' COMMENT '逻辑删除 0-正常 1 删除',
    PRIMARY KEY (`id`)
) ENGINE = InnoDB
  AUTO_INCREMENT = 1
  DEFAULT CHARSET = utf8
  COLLATE = utf8_unicode_ci COMMENT ='接口信息表';
```

### 订单表

```mysql
CREATE TABLE IF NOT EXISTS `order_info`
(
    `id`                bigint(20) unsigned                                    NOT NULL AUTO_INCREMENT COMMENT '自增长id',
    `order_id`          varchar(50) CHARACTER SET utf8 COLLATE utf8_unicode_ci          DEFAULT NULL COMMENT '订单编号',
    `user_id`           bigint(20)                                             NOT NULL COMMENT '用户id',
    `pay_type`          tinyint(4) unsigned                                    NOT NULL DEFAULT '0' COMMENT '支付方式（0 wxpay 1 alipay）',
    `money`             decimal(10, 2)                                                  DEFAULT NULL COMMENT '价格',
    `pay_status`        tinyint(4) unsigned                                    NOT NULL DEFAULT '0' COMMENT '支付状态（0 未支付 1 已失效 2 正在审核 3 已完成）',
    `commodity_content` varchar(50) CHARACTER SET utf8 COLLATE utf8_unicode_ci NOT NULL COMMENT '商品内容',
    `create_time`       datetime                                               NOT NULL DEFAULT CURRENT_TIMESTAMP COMMENT '创建时间',
    `update_time`       datetime                                               NOT NULL DEFAULT CURRENT_TIMESTAMP ON UPDATE CURRENT_TIMESTAMP COMMENT '更新时间',
    `expire_time`       datetime                                               NOT NULL COMMENT '过期时间',
    `id_deleted`        tinyint(4) unsigned                                    NOT NULL DEFAULT '0' COMMENT '逻辑删除 （0 正常 1 删除）',
    PRIMARY KEY (`id`)
) ENGINE = InnoDB
  AUTO_INCREMENT = 1
  DEFAULT CHARSET = utf8
  COLLATE = utf8_unicode_ci COMMENT ='订单表';
```

## 三、功能

|                            功能                            | 普通用户 | 管理员 |
| :--------------------------------------------------------: | :------: | :----: |
|       [开发者API在线文档](https://apidocs.yeguo.icu)       |    √     |   √    |
| [SDK使用](https://apidocs.yeguo.icu/guide/getting-started) |    √     |   √    |
|                       登录注册(邮箱)                       |    √     |   √    |
|                     找回密码(需有邮箱)                     |    √     |   √    |
|                          接口市集                          |    √     |   √    |
|                          在线调试                          |    √     |   √    |
|                          钱包充值                          |    √     |   √    |
|                    订单支付、取消、删除                    |    √     |   √    |
|                        更新个人信息                        |    √     |   √    |
|                          更新凭证                          |    √     |   √    |
|                    管理用户、接口、订单                    |    ×     |   √    |

## 四、父类管理依赖

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0" xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 https://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <packaging>pom</packaging>

    <groupId>icu.yeguo</groupId>
    <artifactId>yeguo-API</artifactId>
    <version>0.0.1</version>
    <name>yeguo-API</name>
    <description>yeguo-API</description>

    <modules>
        <module>API_backend</module>
        <module>API_interface</module>
        <module>API_gateway</module>
        <module>API_common</module>
    </modules>

    <properties>
        <java.version>17</java.version>
        <maven.compiler.target>17</maven.compiler.target>
        <project.build.sourceEncoding>UTF-8</project.build.sourceEncoding>
        <spring-boot.version>3.3.0</spring-boot.version>
        <knife4j.version>4.4.0</knife4j.version>
        <mysql.version>8.0.33</mysql.version>
        <mybatis-plus.version>3.5.5</mybatis-plus.version>
        <springfox-swagger2.version>2.10.5</springfox-swagger2.version>
        <org.jetbrains-annotations.version>13.0</org.jetbrains-annotations.version>
        <hutool-all.version>5.8.27</hutool-all.version>
        <bcprov-jdk15to18.version>1.74</bcprov-jdk15to18.version>
        <javax.mail.version>1.6.2</javax.mail.version>
        <spring-boot-maven-plugin.version>3.3.0</spring-boot-maven-plugin.version>
        <spring-cloud-gateway.version>4.1.4</spring-cloud-gateway.version>
        <dubbo.version>3.2.12</dubbo.version>
        <jakarta.servlet-api.version>6.0.0</jakarta.servlet-api.version>
    </properties>

    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.apache.dubbo</groupId>
                <artifactId>dubbo-bom</artifactId>
                <version>${dubbo.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!--spring cloud gateway-->
            <dependency>
                <groupId>org.springframework.cloud</groupId>
                <artifactId>spring-cloud-starter-gateway</artifactId>
                <version>${spring-cloud-gateway.version}</version>
            </dependency>
            <dependency>
                <groupId>org.springframework.boot</groupId>
                <artifactId>spring-boot-dependencies</artifactId>
                <version>${spring-boot.version}</version>
                <type>pom</type>
                <scope>import</scope>
            </dependency>
            <!-- knife4j -->
            <dependency>
                <groupId>com.github.xiaoymin</groupId>
                <artifactId>knife4j-openapi3-jakarta-spring-boot-starter</artifactId>
                <version>${knife4j.version}</version>
            </dependency>
            <dependency>
                <groupId>mysql</groupId>
                <artifactId>mysql-connector-java</artifactId>
                <version>${mysql.version}</version>
            </dependency>
            <!-- mybatis plus -->
            <dependency>
                <groupId>com.baomidou</groupId>
                <artifactId>mybatis-plus-spring-boot3-starter</artifactId>
                <version>${mybatis-plus.version}</version>
            </dependency>
            <dependency>
                <groupId>io.springfox</groupId>
                <artifactId>springfox-swagger2</artifactId>
                <version>${springfox-swagger2.version}</version>
            </dependency>
            <dependency>
                <groupId>org.jetbrains</groupId>
                <artifactId>annotations</artifactId>
                <version>${org.jetbrains-annotations.version}</version>
                <scope>compile</scope>
            </dependency>
            <dependency>
                <groupId>cn.hutool</groupId>
                <artifactId>hutool-all</artifactId>
                <version>${hutool-all.version}</version>
            </dependency>
            <dependency>
                <groupId>org.bouncycastle</groupId>
                <artifactId>bcprov-jdk15to18</artifactId>
                <version>${bcprov-jdk15to18.version}</version>
            </dependency>
            <dependency>
                <groupId>com.sun.mail</groupId>
                <artifactId>javax.mail</artifactId>
                <version>${javax.mail.version}</version>
            </dependency>
            <dependency>
                <groupId>jakarta.servlet</groupId>
                <artifactId>jakarta.servlet-api</artifactId>
                <version>${jakarta.servlet-api.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>

    <build>
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.springframework.boot</groupId>
                    <artifactId>spring-boot-maven-plugin</artifactId>
                    <version>${spring-boot-maven-plugin.version}</version>
                    <configuration>
                        <excludes>
                            <exclude>
                                <groupId>org.projectlombok</groupId>
                                <artifactId>lombok</artifactId>
                            </exclude>
                        </excludes>
                    </configuration>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>

</project>
```

## 五、redis & session

**依赖**

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-data-redis</artifactId>
</dependency>
<dependency>
    <groupId>org.springframework.session</groupId>
    <artifactId>spring-session-data-redis</artifactId>
</dependency>
```

**配置**

```yaml
spring
 # session
  session:
    timeout: 86400
    store-type: redis
  # redis
  data:
    redis:
      port: 6379
      host: 127.0.0.1
      database: 0
```

session存储入到redis中实现分布式登录

## 六、Knife4j & swagger

**简单介绍**

Swagger 是一个开源的框架，主要用于设计、构建、记录和使用 RESTful Web 服务。它提供了一整套用于生成、描述、调用和可视化 RESTful API 的工具。

Knife4j 是基于 Swagger 的一个增强工具，旨在提供更友好的 API 文档界面和更多实用功能，特别适用于 Spring Boot 项目。它最初被称为 "Swagger-Bootstrap-UI"。

**依赖**

```xml
<dependency>
    <groupId>io.springfox</groupId>
    <artifactId>springfox-swagger2</artifactId>
</dependency>
<dependency>
    <groupId>com.github.xiaoymin</groupId>
    <artifactId>knife4j-openapi3-jakarta-spring-boot-   starter</artifactId>
    <version>4.4.0</version>
</dependency>
```

**配置**

```yaml
# springdoc-openapi
springdoc:
  swagger-ui:
    path: /swagger-ui.html
    tags-sorter: alpha
    operations-sorter: alpha
  api-docs:
    path: /v3/api-docs
  group-configs:
    - group: 'default'
      paths-to-match: '/**'
      packages-to-scan: icu.yeguo.yeguoapi.controller
# knife4j
knife4j:
  enable: true
  setting:
    language: zh_cn
```

```java
@Configuration
@EnableSwagger2WebMvc
@Profile("dev") // 只在开发阶段启用
public class Knife4jConfiguration {

    @Bean
    public Docket defaultApi2() {
        return new Docket(DocumentationType.SWAGGER_2)
                .apiInfo(new ApiInfoBuilder()
                        .title("yeguoAPI")
                        .description("yeguoAPI")
                        .version("1.0")
                        .build())
                .select()
                // 指定 Controller 扫描包路径
                .apis(RequestHandlerSelectors
                        .basePackage("com.yeguo.yeguoapi.controller"))
                .paths(PathSelectors.any())
                .build();
    }
}
```

## 七、Dubbo

**简单介绍**

Dubbo 是一个开源的高性能 RPC（Remote Procedure Call，远程过程调用）框架，最初由阿里巴巴开发，用于解决分布式系统中的服务治理和服务调用问题。它能够有效地帮助开发者实现微服务架构，支持高并发和大规模分布式系统的构建。

**依赖**

```xml
<dependency>
    <groupId>org.apache.dubbo</groupId>
    <artifactId>dubbo-spring-boot-starter</artifactId>
    <version>3.2.12</version>
</dependency>
```

**配置生产者**

```yaml
dubbo:
  application:
    enable-file-cache: false
    qosEnable: false
    name: dubbo-springboot-consumer
  protocol:
    name: dubbo
#    port: -1 不指定默认为20880
  registry:
    id: nacos-registry
    address: nacos://localhost:8848
```

**配置消费者**

```yaml
dubbo:
  application:
    enable-file-cache: false
    qosEnable: false
    name: dubbo-springboot-consumer
  protocol:
    name: dubbo
    port: -1  # 自动分配
  registry:
    id: nacos-registry
    address: nacos://localhost:8848
```

> 文档：https://cn.dubbo.apache.org/zh-cn/overview/home/

## 八、Nacos

**简单介绍**

Nacos 是阿里巴巴开源的一款动态服务发现、配置管理和服务治理平台。Nacos 的全称是 **N**aming and **C**onfiguration **S**ervice。它旨在帮助开发者轻松构建和管理微服务架构。

**依赖**

```xml
<dependency>
    <groupId>org.apache.dubbo</groupId>
    <artifactId>dubbo-nacos-spring-boot-starter</artifactId>
</dependency>
```

**生产者和消费者配置**

```yaml
  registry:
    id: nacos-registry
    address: nacos://localhost:8848
```

> 文档：https://nacos.io/zh-cn/docs/quick-start.html
>
> 下载：https://github.com/alibaba/nacos/releases

## 九、spring cloud gateway

**简单介绍**

Spring Cloud Gateway 是基于 Spring 生态系统开发的 API 网关服务，旨在提供简单、有效和统一的 API 路由管理。作为 Spring Cloud 微服务架构中的重要组成部分，Spring Cloud Gateway 提供了动态路由、负载均衡、路径重写、断路器、限流等功能。

**依赖**

```xml
<dependency>
	<groupId>org.springframework.cloud</groupId>
	<artifactId>spring-cloud-starter-gateway</artifactId>
</dependency>
```

**作用：**

* 统一鉴权 required
* 统一日志
* 路由
* 接口调用计数
* 统一业务逻辑
* 跨域
* 访问控制
  * 黑白名单
* 发布控制
  * 灰度发布
* 流量染色
  * 例：经过网关的请求打上标记 如：加个标识请求头
* 统一接口保护
  a.限制请求
  b.信息脱敏
  c.降级（熔断）
  d.限流
  e.超时时间

**工作流程图**

![spring_cloud_gateway_diagram](https://cdn.jsdelivr.net/gh/ye-guo/Images/images/spring_cloud_gateway_diagram.webp)

**工作流程:**

* 客户端发起请求

* Hadler Mapping：根据断言，请求转发到对应路由

* Web Handler：处理请求，层层过滤器

**配置式配置：**

```yaml
spring:
  application:
    name:
      API_gataway
  cloud:
    gateway:
      default-filters:
        - AddRequestHeader=source, yeguo_api_gateway # 流量染色
      routes:
        - id: api_router
          uri: http://localhost:8082/api
          predicates: # 断言
            - Path=/api/**
 logging:
  level:
    org:
      springframework:
        cloud:
          gateway: info # 开发阶段可以开启trace模式跟踪日志
```

**filter**

自定义过滤器，根据业务对请求和响应进行处理

```java
@Bean
public GlobalFilter customFilter() {
    return new CustomGlobalFilter();
}

public class CustomGlobalFilter implements GlobalFilter, Ordered {

    @Override
    public Mono<Void> filter(ServerWebExchange exchange, GatewayFilterChain chain) {
        log.info("custom global filter");
        return chain.filter(exchange);
    }

    @Override
    public int getOrder() {
        return -1;
    }
}
```

> 文档：https://docs.spring.io/spring-cloud-gateway/docs/4.1.x/reference/html/

## 十、SDK开发

依赖:

引入的项目配置提供提示

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-configuration-processor</artifactId>
    <optional>true</optional>
</dependency>
```

配置：

```java
@ConfigurationProperties(prefix = "yeguo.api")
@Data
public class YGApiClientConfig {

    public String accessKey;
    public String secretKey;
    public String gateway;

    @Bean
    public YGApiClient ygapiClient() {
        return new YGApiClient(accessKey, secretKey, gateway);
    }
}
```

springboot 2.7后需要在

METAINF/spring/org.springframework.boot.autoconfigure.AutoConfiguration.imports 文件下 加入配置类的包路径

```java
icu.yeguo.yeguoapisdk.config.YGApiClientConfig
```

SDK使用：

* 注入使用

  * 配置.yaml

  * ```yaml
    yeguo:
      api:
        accessKey: your-accessKey
        secretKey: your-secretKey
        gateway: https://gw.yeguo.icu
    ```

  * ```java
    @Autowired
    private YGAPIClient ygapiClient;
    ```

* 手动实例化使用

  * ```java
    String accessKey = "your-accessKey";
    String secretKey = "your-secretKey";
    String gateway = "https://gw.yeguo.icu";
    YGApiClient ygApiClient = new YGApiClient(accessKey,secretKey,gateway);
    ```



## 十一、上传SDK到Maven仓库

> 参考文章：https://blog.csdn.net/qq_34905631/article/details/136822495
>
> 参考文章：https://zhuanlan.zhihu.com/p/693639656
>
> 参考文档：https://central.sonatype.org/register/central-portal/

## 十二、部署服务(Centos7.9)

### JDK17

**1.下载 RPM 包**： 使用 `wget` 或 `curl` 从 Oracle 官方网站下载 JDK 17 的 RPM 包：

```bash
wget https://download.oracle.com/java/17/latest/jdk-17_linux-x64_bin.rpm
```

或使用curl

```bash
curl -O https://download.oracle.com/java/17/latest/jdk-17_linux-x64_bin.rpm
```

**2.安装 RPM 包**： 使用 `yum localinstall` 命令安装下载的 RPM 包：

```bash
sudo yum localinstall jdk-17_linux-x64_bin.rpm
```

**3.验证安装**： 检查 Java 版本以确认 JDK 是否正确安装：

```bash
java -version
```

### maven

**1.下载 Maven 3.9.7**：

```bash
wget https://downloads.apache.org/maven/maven-3/3.9.7/binaries/apache-maven-3.9.7-bin.tar.gz
```

**2.解压文件**：

```bash
tar -xzf apache-maven-3.9.7-bin.tar.gz
```

**3.移动到合适的位置**：

```bash
sudo mv apache-maven-3.9.7 /opt/maven
```

**4.配置 Maven 环境变量:**

```bash
sudo vi /etc/profile
```

或者，编辑用户的 `bash` 配置文件（只对当前用户生效）：

```
vi ~/.bash_profile
```

**5.添加 Maven 环境变量**：在文件末尾添加以下内容（根据实际安装路径调整）：

```
# Maven
export MAVEN_HOME=/opt/maven
export PATH=$PATH:$MAVEN_HOME/bin
```

**6.使更改生效**：

```bash
source /etc/profile
or
source ~/.bash_profile
```

**7.验证 Maven 安装:**

```bash
mvn -version
# 输出
Apache Maven 3.9.7 (8b094c9513efc1b9ce2d952b3b9c8eaedaf8cbf0)
Maven home: /opt/apache-maven-3.9.7
Java version: 17.0.12, vendor: Oracle Corporation, runtime: /usr/lib/jvm/jdk-17.0.12-oracle-x64
Default locale: en_US, platform encoding: UTF-8
OS name: "linux", version: "3.10.0-1160.45.1.el7.x86_64", arch: "amd64", family: "unix"
```

### git 

**1.下载依赖:**

```bash
sudo yum install -y wget perl-CPAN gettext-devel perl-devel openssl-devel zlib-devel curl-devel expat-devel
```

**2.下载 Git 2.46.0 的二进制 tar.gz 包：**

```bash
https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.46.0.tar.gz
或
wget https://mirrors.edge.kernel.org/pub/software/scm/git/git-2.46.0.tar.gz
```

**3.解压安装:**

```bash
tar -xzf git-2.46.0.tar.gz
cd git-2.46.0
make prefix=/usr/local all
sudo make prefix=/usr/local install
```

**4.验证安装**

```bash
git --version
# 输出
git version 2.46.0
```

### nacos

```bash
git clone https://github.com/alibaba/nacos.git
cd nacos/
mvn -Prelease-nacos -Dmaven.test.skip=true clean install -U  
ls -al distribution/target/

// change the $version to your actual path
cd distribution/target/nacos-server-$version/nacos/bin
```

### redis

**1.安装 EPEL 仓库：**

Redis 不是 CentOS 默认仓库的一部分，因此需要启用 EPEL（Extra Packages for Enterprise Linux）仓库：

```sh
sudo yum install epel-release
```

**2.安装 Redis：**

```sh
sudo yum install redis
```

**3.配置 Redis：**

```sh
sudo nano /etc/redis.conf
```

**4.启动和启用 Redis 服务:**

启动 Redis 服务并使其在系统启动时自动启动：

```sh
sudo systemctl start redis
sudo systemctl enable redis
```

**5.检查 Redis 服务状态:**

确认 Redis 服务正在运行：

```bash
sudo systemctl status redis
```

### mysql

* 参考文章：https://blog.csdn.net/qq_45316925/article/details/139457426

* 参考文章：https://www.cnblogs.com/asker009/p/15072354.html

* 依赖：wget https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-community-common-8.0.12-1.el7.x86_64.rpm

* 依赖：wget https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-community-libs-8.0.12-1.el7.x86_64.rpm

* 客户端：wget https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-community-client-8.0.12-1.el7.x86_64.rpm

* 服务端：wget https://dev.mysql.com/get/Downloads/MySQL-8.0/mysql-community-server-8.0.12-1.el7.x86_64.rpm

* sudo rpm -ivh *.rpm

* > 按从上到下顺序安装

```bash
# 状态
systemctl status mysqld
# 启动
systemctl start mysqld
# 查看初始密码
cat /var/log/mysqld.log | grep password
# 或
grep 'temporary password' /var/log/mysqld.log

mysql -uroot -p
# 设置密码
ALTER USER 'root'@'localhost' IDENTIFIED BY '自己设置密码';
#修改远程连接
update user set host='%' where user='root';
```

### nginx

```bash
# 安装 EPEL 仓库
sudo yum install epel-release
#安装 Nginx
sudo yum install nginx
# 启动和启用 Nginx 服务
sudo systemctl start nginx
# Nginx在系统启动时自动启动
sudo systemctl enable nginx
#检查 Nginx 服务状态
sudo systemctl status nginx
```

### 前端部署

**多环境配置**

```ts
export const request:RequestConfig = {
  baseURL:
    process.env.NODE_ENV === 'production'
      ? 'https://server.api.yeguo.icu'
      : '',
    // 注意不要设置开发环境地址，可能和会跨域代理冲突导致不能使用
};
```

**直接托管到vercle**

### 后端部署

* 选购服务器

* 接口服务部署在内网

  * ```bash
    nohup java -jar API_interface-0.0.1.jar > api_interface.log 2>&1 &
    ```

* 平台后端部署暴露公网

  * 申请证书

    * ```bash
      sudo certbot certonly --standalone -d server.api.yeguo.icu
      ```

  * ```bash
    # 启用生产环境配置
    nohup java -jar ./API_backend-0.0.1.jar  --spring.profiles.active=prod > api_backend.log 2>&1 &
    ```

* 接口网关部署暴露公网 

  * 申请证书

    * ```bash
      sudo certbot certonly --standalone -d gw.yeguo.icu
      ```

  * ```bash
    # 启用生产环境配置
    nohup java -jar ./API_gateway-0.0.1.jar --spring.profiles.active=prod > api_gateway.log 2>&1 &
    ```

### nginx配置

* **平台后端**：只允许前端源跨域、cookie配置

```nginx
server {
  listen 443 ssl;
  listen [::]:443 ssl;
  server_name server.api.yeguo.icu;

  ssl_certificate /etc/letsencrypt/live/server.api.yeguo.icu/fullchain.pem;
  ssl_certificate_key /etc/letsencrypt/live/server.api.yeguo.icu/privkey.pem;
  ssl_session_timeout 5m;
  ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
  ssl_protocols TLSv1.2 TLSv1.3;
  ssl_prefer_server_ciphers on;
  ssl_stapling on;
  ssl_stapling_verify on;

  location / {
    # 设置 CORS 头部
    add_header 'Access-Control-Allow-Origin' 'https://api.yeguo.icu' always;
    add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS, DELETE, PUT' always;
    add_header 'Access-Control-Allow-Headers' 'Content-Type, Authorization' always;
    add_header 'Access-Control-Allow-Credentials' 'true' always;

    # 处理预检请求
    if ($request_method = 'OPTIONS') {
      add_header 'Access-Control-Allow-Origin' $cors_header always;
      add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS, DELETE, PUT' always;
      add_header 'Access-Control-Allow-Headers' 'Content-Type, Authorization' always;
      add_header 'Access-Control-Allow-Credentials' 'true' always;
      add_header 'Access-Control-Max-Age' 1728000;
      add_header 'Content-Type' 'text/plain charset=UTF-8';
      add_header 'Content-Length' 0;
      return 204;
    }

    proxy_redirect off;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_pass http://127.0.0.1:8080;
  }
}

server {
  listen 80;
  listen [::]:80;
  server_name server.api.yeguo.icu;
  return 301 https://$host$request_uri;
}
```

* 网关gateway跨域配置，允许所有源

```nginx
server {
  # SSL 默认访问端口号为 443
  listen 443 ssl;
  listen [::]:443 ssl; # 添加IPv6监听
  # 请填写绑定证书的域名
  server_name gw.yeguo.icu;
  # 请填写证书文件的相对路径或绝对路径
  ssl_certificate /etc/letsencrypt/live/gw.yeguo.icu/fullchain.pem;
  # 请填写私钥文件的相对路径或绝对路径
  ssl_certificate_key /etc/letsencrypt/live/gw.yeguo.icu/privkey.pem;
  ssl_session_timeout 5m;
  # 请按照以下套件配置，配置加密套件，写法遵循 openssl 标准。
  ssl_ciphers ECDHE-RSA-AES128-GCM-SHA256:ECDHE:ECDH:AES:HIGH:!NULL:!aNULL:!MD5:!ADH:!RC4;
  # 请按照以下协议配置
  ssl_protocols TLSv1.2 TLSv1.3;
  ssl_prefer_server_ciphers on;
  ssl_stapling on;
  ssl_stapling_verify on;
  location / {
    # 添加 CORS 头部
    add_header 'Access-Control-Allow-Origin' '*';
    add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS, DELETE, PUT';
    add_header 'Access-Control-Allow-Headers' 'Content-Type, Authorization';
    if ($request_method = 'OPTIONS') {
      add_header 'Access-Control-Allow-Origin' '*';
      add_header 'Access-Control-Allow-Methods' 'GET, POST, OPTIONS, DELETE, PUT';
      add_header 'Access-Control-Allow-Headers' 'Content-Type, Authorization';
      add_header 'Access-Control-Max-Age' 1728000;
      add_header 'Content-Type' 'text/plain charset=UTF-8';
      add_header 'Content-Length' 0;
      return 204;
    }
    proxy_redirect off;
    proxy_set_header Host $host;
    proxy_set_header X-Real-IP $remote_addr;
    proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
    proxy_pass http://127.0.0.1:8081;
  }
}

server {
  listen 80;
  listen [::]:80; # 添加IPv6监听
  # 请填写绑定证书的域名
  server_name gw.yeguo.icu;
  # 把http的域名请求转成https
  return 301 https://$host$request_uri;
}
```

## 十三、配置防火墙

**1.添加 `ACCEPT` 规则，允许本地流量访问特定端口**

```bash
sudo iptables -A INPUT -p tcp --dport 8848 -s 127.0.0.1 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 8080 -s 127.0.0.1 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 8081 -s 127.0.0.1 -j ACCEPT
sudo iptables -A INPUT -p tcp --dport 8082 -s 127.0.0.1 -j ACCEPT
```

**2.添加 `DROP` 规则，阻止外部流量访问这些端口**

```bash
sudo iptables -A INPUT -p tcp --dport 8848 -j DROP
sudo iptables -A INPUT -p tcp --dport 8080 -j DROP
sudo iptables -A INPUT -p tcp --dport 8081 -j DROP
sudo iptables -A INPUT -p tcp --dport 8082 -j DROP
```

**3.检查规则列表**

```bash
sudo iptables -L -v -n
```

> `ACCEPT` 规则需要在 `DROP` 规则之前，不然会拒绝所有流量

**4.保存规则，以确保在系统重启后规则仍然有效**

```bash
sudo iptables-save > /etc/sysconfig/iptables
```

## 其他、🕳️坑

### 地址问题

上线时内网地址填 127.0.0.1，填localhost可能会出现问题 （数据库填的localhost导致后端一直报错，排错到吐血🤮💔）

### cookie问题

当 `Access-Control-Allow-Credentials` 设置为 `true` 时，`Access-Control-Allow-Origin` 头不能为 `*`。必须明确指定允许的源。

前端请求需要设置`withCredentials: true`（axios配置）。

不然会出现，响应头有set-cookie,但浏览器一直没法保存，请求时无法携带cookie，导致没法获取seesion。（差点崩溃😭）

**cookie中属性** 

* ``name和value``
  - **name**: Cookie 的名称。
  - **value**: Cookie 的值。
* `expires`
  - **描述**: 指定 Cookie 的过期时间。可以是一个日期对象或 UTC 时间字符串。
  - **类型**: Date。
  - **示例**: `Expires=Wed, 21 Oct 2024 07:28:00 GMT`。

*  `max-age`

  - **描述**: 指定 Cookie 的有效期（以秒为单位）。与 `expires` 属性类似，但 `max-age` 是相对时间，而 `expires` 是绝对时间。

  - **类型**: Integer (秒)。

  - **示例**: `Max-Age=3600`（表示 1 小时）。

* `domain`

  - **描述**: 指定 Cookie 可以被哪些域访问。默认是设置 Cookie 的域。

  - **类型**: String。

  - **示例**: `Domain=example.com`，允许 `example.com` 及其子域名访问。

* `path`

  - **描述**: 指定 Cookie 可以被哪些路径访问。默认是设置 Cookie 的路径。

  - **类型**: String。

  - **示例**: `Path=/app`，表示 Cookie 只能在 `/app` 路径下访问。

* `secure`

  - **描述**: 指定 Cookie 仅通过 HTTPS 传输。

  - **类型**: Boolean。

  - **示例**: `Secure`。

* `HttpOnly`

  - **描述**: 指定 Cookie 不能被 JavaScript 访问，只有 HTTP 请求可以访问。提高安全性，防止 XSS 攻击。
  - 带有 `HttpOnly` 属性的 cookie 只能通过 HTTP/HTTPS 请求被发送到服务器。在用户发起的请求中，这些 cookie 会被自动添加到请求头中（例如通过浏览器自动处理）。

  - **类型**: Boolean。

  - **示例**: `HttpOnly`。

* `SameSite`

  - 描述

    : 指定 Cookie 的跨站请求行为。用于防止 CSRF 攻击。

    - `Strict`: 仅在同站请求时发送 Cookie。
    - `Lax`: 在跨站 GET 请求中发送 Cookie，但在其他请求中不发送。
    - `None`: 允许跨站请求发送 Cookie（需要 `Secure` 属性）。

  - **类型**: String。

  - **示例**: `SameSite=Strict`。

> **前端和后端分离**: 如果前端和后端部署在不同的端口上，但同在一个域名下，确保你的跨域配置正确。对于 CORS 请求，你需要在前端和后端都正确设置 `withCredentials`，并且确保 Nginx 配置中的 `Access-Control-Allow-Origin` 不使用 `*`，而是指定准确的源。
