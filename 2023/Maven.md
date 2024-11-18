## Maven

### 一、依赖管理

#### 1.依赖传递冲突问题

* 路径优先：当依赖中出现相同的资源时，层级越深，优先级越低，层级越浅，优先级越高
* 声明优先：当资源在相同层级被依赖时，配置顺序靠前的覆盖配置顺序靠后的
* 特殊优先：当同级配置了相同资源的不同版本，后配置的覆盖先配置的

#### 2.可选依赖

将自己项目使用的依赖隐藏，当开发者引入某个依赖(该依赖引入了该依赖)不会间接传递该依赖

```xml
	<dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.14</version>
			<!--间接不传递该依赖，true-->
			<optional>true</optional>
   		</dependency>
```

#### 3.排除依赖

​	排除使用的依赖中使用的其他依赖，解决和自己项目的依赖冲突

```xml
	<dependency>
      <groupId>top.aidjajd</groupId>
      <artifactId>aidjajd_project</artifactId>
      <version>1.0-SNAPSHOT</version>
      <!--排除依赖log4j-->
              <exclusions>
                <exclusion>
                <!--写入排除坐标不需要写version-->
                  <groupId>log4j</groupId>
                  <artifactId>log4j</artifactId>
                </exclusion>
              </exclusions>
    </dependency>
```

#### 4.依赖范围

* 依赖的jar默认情况可以在任何地方使用，可以通过scope标签设定其作用范围
  作用范围
* 主程序范围有效(main文件夹范围内)
* 测试程序范围有效(test文件夹范围内)》
* 是否参与打包(package指令范围内)

| Scope         | 主代码 | 测试代码 | 打包 | 范例        |
| ------------- | ------ | -------- | :--: | ----------- |
| compile(默认) | Y      | Y        |  Y   | log4j       |
| test          |        | Y        |      | junit       |
| provided      | Y      | Y        |      | servlet-api |
| runtime       |        |          |  Y   | jdbe        |

#### 5.依赖范围传递性

间接依赖为test|provided时候，因不参与打包所以不会被传递；当间接依赖为compile和runtime时，会被间接引入，当直接依赖被设置范围时，该间接依赖会跟随为该依赖范围


| 间接依赖\直接-> | compile(默认) | test | provided | runtime |
| --------------- | ------------- | ---- | :------: | ------- |
| compile(默认)   | compile       | test | provided | runtime |
| test            |               |      |          |         |
| provided        |               |      |          |         |
| runtime         | runtime       | test | provided | runtime |

### 二、生命周期与插件

Maven对项目构建的生命周期划分为3套

* clean:清理工作
* default:核心工作，例如编译，测试，打包，部署等
* site:产生报告，发布站点等

![](https://gitee.com/jsppage/img/raw/master/md/202411171036335.png)

![](https://gitee.com/jsppage/img/raw/master/md/202411171035515.png)

![](https://gitee.com/jsppage/img/raw/master/md/202411171036590.png)

主要关注阶段：clean -> compile -> test -> package -> install

生命周期过程每一个阶段对应一个插件(plugin)

### 三、聚合继承

作用：用于快速构建maven工程，一次构建多个项目/模块，管理模块

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!-- 工作组 -->
    <groupId>com.myGroup</groupId>
    <artifactId>test</artifactId>
    <version>1.0-SNAPSHOT</version>
    <packaging>pom</packaging>

    <name>parent-name</name>
    <description>父工程</description>
    <url>https://example.com</url>
    <!-- 模块管理 -->
    <modules>
        <module>my-server</module>
    </modules>
 
    <!-- 版本管理 -->
    <properties>
        <lombok.version>1.18.34</lombok.version>
        <maven-plugin.version>3.0.0-M5</maven-plugin.version>
    </properties>
	<!-- 依赖管理 -->
    <dependencyManagement>
        <dependencies>
            <dependency>
                <groupId>org.projectlombok</groupId>
                <artifactId>lombok</artifactId>
                <version>${lombok.version}</version>
            </dependency>
        </dependencies>
    </dependencyManagement>
    <!-- 构建管理 -->
    <build>
        <pluginManagement>
            <plugins>
                <plugin>
                    <groupId>org.apache.maven.plugins</groupId>
                    <artifactId>maven-surefire-plugin</artifactId>
                    <version>${maven-plugin.version}</version>
                </plugin>
            </plugins>
        </pluginManagement>
    </build>
</project>
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<project xmlns="http://maven.apache.org/POM/4.0.0"
         xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
         xsi:schemaLocation="http://maven.apache.org/POM/4.0.0 http://maven.apache.org/xsd/maven-4.0.0.xsd">
    <modelVersion>4.0.0</modelVersion>
    <!-- 父工程 -->
    <parent>
 		<groupId>com.myGroup</groupId>
    	<artifactId>test</artifactId>
    	<version>1.0-SNAPSHOT</version>
        <!-- 父工程pom相对路径 -->
        <relativePath>pom.xml</relativePath>
    </parent>
    
    <artifactId>my-server</artifactId>
    <packaging>jar</packaging>
    <name>child-name</name>
    <description>子工程</description>
    <url>https://example.com</url>
    
	<!-- 依赖版本直接继承父工程的，可以不再指定 -->
    <dependencies>
 		<dependency>
			<groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
		</dependency>
    </dependencies>
    
    <!-- 构建管理 -->
    <build>
		<plugins>
        	<plugin>
            	<groupId>org.apache.maven.plugins</groupId>
                <artifactId>maven-surefire-plugin</artifactId>
			</plugin>
		</plugins>
    </build>
</project>
```
