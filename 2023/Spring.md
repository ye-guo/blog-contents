### Spring

### 一、bean	

* name：定义bean别名，可以定义多个使用(,)/(;)/( )分割
* scope: 
  * singleton：单例 (默认)
  * prototype: 非单例

* 适合交给容器进行管理的bean
  * 表现层对象
  * 业务层对象
  * 数据层对象
  * 工具对象

* 不适合交给容器进行管理的bean
  * 封装实体的域对象

### 二、bean的生命周期

* 初始化容器
  1.创建对象（内存分配)
  2.执行构造方法
  3.执行属性注入(set操作)
  4.执行bean初始化方法
* 使用bean
  1.执行业务操作
* 关闭/销毁容器
  1.执行bean销毁方法

### 三、依赖注入

依赖注入方式选择

* 1.强制依赖使用构造器进行，使用setteri注入有概率不进行注入导致nu11对象出现
* 2.可选依赖使用setter注入进行，灵活性强
* 3,Spring框架倡导使用构造器，第三方框架内部大多数采用构造器注入的形式进行数据初始化，相对严谨
* 4.如果有必要可以两者同时使用，使用构造器注入完成强制依赖的注入，使用setteri注入完成可选依赖的注入
* 5.实际开发过程中还要根据实际情况分析，如果受控对象没有提供setter方法就必须使用构造器注入
* 6.自己开发的模块推荐使用setteri注入

自动注入：autowire

* 自动装配用于引用类型依赖注入，不能对简单类型进行操作
* 使用按类型装配时(byType)必须保障容器中相同类型的bean唯一，推荐使用
* 使用按名称装配时(byName)必须保障容器中具有指定名称的bean,因变量名与配置耦合，不推荐使用
* 自动装配优先级低于setteri注入与构造器注入，同时出现时自动装配配置失效

### 四、注解开发

![dr56mv.png](https://files.catbox.moe/dr56mv.png)