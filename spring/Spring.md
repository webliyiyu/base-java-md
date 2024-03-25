# Spring

**`Spring`是什么？可以从两个方面来理解`Spring`：**

- `Spring Framework`，即`Spring`框架本身，包含`IoC`，`MVC`以及`AOP`等核心功能，`Spring Framework`是`Spring`其他模块的基础。
- `Spring`全家桶，如`Spring Data`，`Spring Cloud`，`Spring Boot`，`Spring Security`等模块。

### 三层架构

![image-20240319000107884](image-20240319000107884.png)

![image-20240319000232085](image-20240319000232085.png)

![image-20240319001507691](image-20240319001507691.png)

### 分层解耦

![image-20240318144524517](image-20240318144524517.png)

![image-20240318151803992](image-20240318151803992.png)



### **控制反转 IoC**

IoC(Inversion of Control)控制反转

* 使用对象时，由主动new产生对象转化为由外部提供对象，此过程中对象创建控制权由程序转移到外部，此思想称为控制反转

**@Component **





**Spring技术对IoC思想进行了实现**

* Spring提供了一个容器，称为IoC容器，用来充当IoC思想中的外部
* IoC容器负责对象的创建、初始化等一系列工作，被创建或者被管理的对象在IoC容器中统称为Bean





### **依赖注入 DI**

* 在容器中建立bean与bean之间的依赖关系的整个过程，称为依赖注入

**@Autowired **







### Bean的声明

bean默认为单例

**适合交给容器经行管理的bean**

* 表现层对象
* 业务层对象
* 数据层对象
* 工具对象

**不适合交给容器进行管理**

* 封装实体的域对象

![image-20240318150612376](image-20240318150612376.png)

![image-20240318150640147](image-20240318150640147.png)

![image-20240318151620026](image-20240318151620026.png)

## ioC基础容器

**Spring两大核心之一，其他组件功能的基础，主要涉及Bean产生和关系等**

### ioC、DI、AOP思想提出

#### ioC控制反转思想的提出

#### DI依赖注入思想的提出

#### AOP面向切面思想的提出

![image-20240318100649917](image-20240318100649917.png)



#### 框架概念的出现

**特点：**

* 框架（Framework），是基于基础技术之上，从众多业务中抽取的通用解决方案
* 框架是一个半成品，使用框架规定的语法开发可以提高开发效率，可以用简单的代码就能完成复杂的基础业务
* 框架一般都具有扩展性
* 有了框架，可以将精力尽可能的投入在纯业务开发上而不用去费心去技术实现以及一些辅助业务



#### Spring框架概述

spring是一个开源的轻量级java开发应用框架，可以简化企业级应用开发。Spring解决了开发者在javaEE开发中遇到的许多常见的问题，提供了功能强大IOC、AOP及Web MVC等功能。是当前企业中java开发几乎不能缺少的框架之一。Spring的生态及其完善，不管是Spring哪个领域的解决方案都是依附于在Spring Framework基础框架的

![image-20240318102557432](image-20240318102557432.png)



#### BeanFactory快速入门

![image-20240318102820903](image-20240318102820903.png)

![image-20240318153735446](image-20240318153735446.png)

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--  配置UserServiceImlp  bean-->
    <!--  id属性表示给bean起名字  -->
    <!--  class表示给bean定义类型  -->
    <bean id="userService" class="com.buercorp.wangyu.spring.demo.service.impl.UserServiceImpl">
        <!--name="xxx"-->
        <property name="userdao" ref="userDao"></property>

    </bean>
<!--配置userDao-->
    <bean id="userDao" class="com.buercorp.wangyu.spring.demo.dao.impl.UserDaoImpl">
</bean>
</beans>
```

```java
package com.buercorp.wangyu.spring.demo.dao.impl;

import com.buercorp.wangyu.spring.demo.dao.UserDao;
import com.buercorp.wangyu.spring.demo.service.impl.UserService;
import org.springframework.beans.factory.support.DefaultListableBeanFactory;
import org.springframework.beans.factory.xml.XmlBeanDefinitionReader;

import static org.junit.jupiter.api.Assertions.*;

class UserDaoImplTest {
    public static void main(String[] args) {

        // 创建工厂
        DefaultListableBeanFactory beanFactory = new DefaultListableBeanFactory();

        // 创建一个读取器
        XmlBeanDefinitionReader reader = new XmlBeanDefinitionReader(beanFactory);

        // 读取配置文件
        reader.loadBeanDefinitions("beans.xml");

        // 根据id获取bean
        UserDao userDao = (UserDao) beanFactory.getBean("userDao");

        System.out.println(userDao); // com.buercorp.wangyu.spring.demo.dao.impl.UserDaoImpl@667a738
    }
}
```

```java
package com.buercorp.wangyu.spring.demo;

import com.buercorp.wangyu.spring.demo.service.impl.UserService;
import org.springframework.beans.factory.support.DefaultListableBeanFactory;
import org.springframework.beans.factory.xml.XmlBeanDefinitionReader;

import static org.junit.jupiter.api.Assertions.*;

class UserServiceImplTest {
    public static void main(String[] args) {
        // 创建工厂
        DefaultListableBeanFactory beanFactory = new DefaultListableBeanFactory();

        // 创建一个读取器
        XmlBeanDefinitionReader reader = new XmlBeanDefinitionReader(beanFactory);

        // 读取配置文件
        reader.loadBeanDefinitions("beans.xml");

        // 根据id获取bean
        UserService userService = (UserService) beanFactory.getBean("userService");

        System.out.println(userService); // com.buercorp.wangyu.spring.demo.service.impl.UserServiceImpl@667a738
    }
}
```

![image-20240318154055265](image-20240318154055265.png)

![image-20240318154153982](image-20240318154153982.png)





#### ApplicationContext快速入门

```java
package com.buercorp.wangyu.spring.demo.service.impl;

import com.buercorp.wangyu.spring.demo.service.MyService;
import org.springframework.context.ApplicationContext;
import org.springframework.context.support.ClassPathXmlApplicationContext;

import static org.junit.jupiter.api.Assertions.*;

class MyServiceImpITest {
    public static void main(String[] args) {
        // 加载Spring配置文件 获取IOC容器
        ApplicationContext applicationContext = new ClassPathXmlApplicationContext("beans.xml"); // applicationContext.xml

        // 从ApplicationContext中获取bean实例
        MyService myService = (MyService) applicationContext.getBean("myService");

        // 调用bean的方法
        System.out.println("myService = " + myService);
        myService.doSomething();

//        BeanFactory去调用该方法获得userdao设置到此处com.buercorp.wangyu.spring.demo.dao.impl.UserDaoImpl@38425407
//        myService = com.buercorp.wangyu.spring.demo.service.impl.MyServiceImpI@3a52dba3
//        MyServiceImpl is doing something...
    }
}
```

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
<!--  配置UserServiceImlp  -->
    <bean id="userService" class="com.buercorp.wangyu.spring.demo.service.impl.UserServiceImpl">
        <!--  name属性表示配置哪一个具体的属性  -->
        <!--  ref属性表示参照哪一个bean  -->
        <!--name="xxx"-->
        <property name="userdao" ref="userDao"></property>

    </bean>
<!--配置userDao-->
    <!--  xxx  -->
    <bean id="userDao" class="com.buercorp.wangyu.spring.demo.dao.impl.UserDaoImpl">
</bean>
<!--  ApplicationContext  -->
    <bean id="myService" class="com.buercorp.wangyu.spring.demo.service.impl.MyServiceImpI">
<!--            <property name="userdao" ref="userDao"></property>-->
    </bean>
</beans>
```



![image-20240318161656544](image-20240318161656544.png)

#### ApplicationContext

![image-20240318163921233](image-20240318163921233.png)

![image-20240321154428428](image-20240321154428428.png)

![image-20240321154443059](image-20240321154443059.png)

![image-20240321154627358](image-20240321154627358.png)

#### 基于xml的Spring应用

![image-20240318164814895](image-20240318164814895.png)

#### 实例化bean

![image-20240321214030571](image-20240321214030571.png)

![image-20240321214355720](image-20240321214355720.png)

#### bean生命周期

![image-20240321214653646](image-20240321214653646.png)



#### DI依赖注入

![image-20240321214913078](image-20240321214913078.png)

![image-20240321214951753](image-20240321214951753.png)

![image-20240322083852128](image-20240322083852128.png)

![image-20240322083913822](image-20240322083913822.png)

![image-20240322084303804](image-20240322084303804.png)

![image-20240322084422711](image-20240322084422711.png)

![image-20240322084519043](image-20240322084519043.png)



## AOP面向切面编程

**Spring两大核心之一，用横向抽取思想对Bean进行增强，主要涉及切面配置、声明式事务控制等**

### 核心概念

* **链接点（JoinPoint）：**程序执行过程中的任意位置，粒度为执行方法、抛出异常、设置变量等
  * 在SpringAOP中，理解为方法的执行
* **切入点（Pointcut）：**匹配链接点的式子
  * 在SpringAOP中，一个切入点可以只描述一个具体方法，也可以匹配多个方法
    * 一个具体方法：com.burcom.wangyu包下的BookDao接口中的无形参无返回值的save方法
    * 匹配多个方法：所有的save方法，所有的get开头的方法，所有以Dao结尾的接口中的任意方法，所哟带有一个参数的方法
* **通知（Advice）：**在切入点处执行的操作，也就是共性功能
  * 在SpringAOP中，功能最终以方法的形式呈现
* **通知类：**定义通知的类
* **切面（Aspect）：**描述通知与切入点的对应关系

![image-20240321180320461](image-20240321180320461.png)











## Spring整合web环境

**Spring整合web的方式、原理和整合web层各个MVC框架的思想**

















## web层解决方案-SpringMVC

**基于MVC思想打造的框架，摆脱Servlet，用更简单的方式开发web代码**

![image-20240324230114824](image-20240324230114824.png)

![image-20240324230123944](image-20240324230123944.png)

![image-20240323135331361](image-20240323135331361.png)



- **@RestController**：这个注解是`@Controller`和`@ResponseBody`的组合。它表示该类中的方法默认返回数据而不是视图，通常用于构建RESTful风格的Web服务。使用`@RestController`注解的类中的所有方法都会自动将返回值作为HTTP响应的主体内容，通常是JSON或XML格式的数据。这意味着，你不需要在每个方法上添加`@ResponseBody`注解来指定返回数据而不是视图



- **@Controller**：这个注解用于定义一个Spring MVC控制器，其方法默认处理HTTP请求并返回视图的逻辑名称，然后由视图解析器解析为具体的视图页面。如果你想要让某个方法返回数据而不是视图，你需要在该具体方法上添加`@ResponseBody`注解



![image-20240323140545514](image-20240323140545514.png)



![image-20240323202006263](image-20240323202006263.png)



![image-20240323202706764](image-20240323202706764.png)

![image-20240323202732192](image-20240323202732192.png)

![image-20240323202750471](image-20240323202750471.png)

![image-20240323203457348](image-20240323203457348.png)



![image-20240323204210317](image-20240323204210317.png)



![image-20240324134026582](image-20240324134026582.png)

![image-20240324135205047](image-20240324135205047.png)

#### RequestBody&RequestParam&PathVariable

![image-20240324135244292](image-20240324135244292.png)

![image-20240324140025553](image-20240324140025553.png)

![image-20240324140033185](image-20240324140033185.png)

## 拦截器

### 概念：

拦截器（Interceptor）是一种动态拦截方法调用的机制

### 作用：

* 在指定的方法调用前后执行预先设定后的代码
* 阻止原始方法的执行

### 过滤器与拦截器区别

* **归属不同：**Filter属于Servlet技术	Interceptor属于SpringMVC技术
* **拦截内容不同：**Filter对所有访问进行增强，Interceptor仅针对SpringMVC的访问进行增强









