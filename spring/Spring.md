# Spring

**`Spring`是什么？可以从两个方面来理解`Spring`：**

- `Spring Framework`，即`Spring`框架本身，包含`IoC`，`MVC`以及`AOP`等核心功能，`Spring Framework`是`Spring`其他模块的基础。
- `Spring`全家桶，如`Spring Data`，`Spring Cloud`，`Spring Boot`，`Spring Security`等模块。



### 分层解耦

![image-20240318144524517](image-20240318144524517.png)

![image-20240318151803992](image-20240318151803992.png)



**Autowired 依赖注入**

**Component 控制反转**



### Bean的声明

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
<!--  配置UserServiceImlp  -->
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
        // 加载Spring配置文件
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
        <!--name="xxx"-->
        <property name="userdao" ref="userDao"></property>

    </bean>
<!--配置userDao-->
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



#### 基于xml的Spring应用

![image-20240318164814895](image-20240318164814895.png)







## AOP面向切面编程

**Spring两大核心之一，用横向抽取思想对Bean进行增强，主要涉及切面配置、声明式事务控制等**















## Spring整合web环境

**Spring整合web的方式、原理和整合web层各个MVC框架的思想**

















## web层解决方案-SpringMVC

**基于MVC思想打造的框架，摆脱Servlet，用更简单的方式开发web代码**



















