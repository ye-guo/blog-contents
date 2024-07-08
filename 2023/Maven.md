## Maven

### 一、依赖管理

#### 1.依赖传递冲突问题

* 路径优先：当依赖中出现相同的资源时，层级越深，优先级越低，层级越浅，优先级越高
* 声明优先：当资源在相同层级被依赖时，配置顺序靠前的覆盖配置顺序靠后的
* 特殊优先：当同级配置了相同资源的不同版本，后配置的覆盖先配置的

#### 2.可选依赖

​	将自己项目使用的依赖隐藏(不透明)

```xml
	<dependency>
            <groupId>log4j</groupId>
            <artifactId>log4j</artifactId>
            <version>1.2.14</version>
			<!--隐藏依赖，true-->
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

![](https://files.catbox.moe/qov6i5.png)

![](https://files.catbox.moe/o8h1it.png)

![](https://files.catbox.moe/bsz6xt.png)

主要关注阶段：clean -> compile -> test -> package -> install

生命周期过程每一个阶段对应一个插件(plugin)

