---
title: Spring核心框架
date: 2018-09-19 19:50:38
categories: Java
tags: 
    - Spring
---

# Beans

## 配置文件的基础属性

#### name属性

在Spring的配置文件中bean元数据的一些属性
name设置别名，如果配置文件中同一个实体类配置多次，需要使用name来区别
在BeanFactory中有方法getBean(String name, Class<T> requiredType)来获取实例
`<alias name="", alias="">`标签可以给name属性设置别名，通过别名和name获取的实例是一个实例

在xml文件中定义

```
<bean name="petLocalService"
      class="com.spring.bean.PetStoreServiceAliasNameImpl">

</bean>
<alias name="petLocalService" alias="petAliasService"/>
```

定义对应的类和接口

```
public interface PetStroeService {

    void findAllPets();

}

public class PetStoreServiceAliasNameImpl implements PetStroeService {
    public PetStoreServiceAliasNameImpl() {
        System.out.println("This is PetStoreServiceAliasNameImpl Constructor");
    }

    public void findAllPets() {
        System.out.println("This is Name or alias!");
    }
}


```

然后我们获取

```
ApplicationContext context = new ClassPathXmlApplicationContext("bean-di.xml");
PetStroeService petStroeServiceName = context.getBean("petLocalService", PetStroeService.class);
PetStroeService petStroeServiceAlias = context.getBean("petAliasService", PetStroeService.class);
petStroeServiceName.findAllPets();
petStroeServiceAlias.findAllPets();
```

输出的结果

```
This is PetStoreServiceAliasNameImpl Constructor
This is Name or alias!
This is Name or alias!
```

由此可见通过别名和通过name获取的其实是同一个实例


#### class属性

通过class属性指定的类来实例化对象，如 

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="petService" class="com.spring.bean.Person$Pet"/>
 	<bean id="petService" class="com.spring.bean.Person"/>
</beans>
```
通过`$`符号来定义静态内部类。如上Person创建的对象是Ioc通过Person的无参构造示例化的。

另外我们还可以通过类中定义的静态方法获取实例，类似单例模式创建一个对象
```
public class ClientService {

    private static ClientService clientService = new ClientService();

    private ClientService() {
        
    }
    
    public static ClientService newInstantce() {
        return clientService;
    }
}
```
在Java中我们创建ClientSercie对象这样子 ClientService cs = ClientService.newInsance()。在Spring配置文件中我们也可以通过`factory-method`属性实现这个效果
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">
	
	<bean id="clientService" class="com.spring.bean.ClientService"
          factory-method="newInstance" />
</beans>
```

如果不想通过静态方法获取，可以使用`factory-name`与`factory-method`组合的方式获取实例化对象，但是这时候就不需要配置class属性了，配置文件如下：
```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="serviceFactory" class="com.spring.bean.ServiceFactory" />
    <bean name="petServiceInFactory"
          factory-bean="serviceFactory"
          factory-method="getPetService" />
</beans>

public class ServiceFactory {

    private PetStroeService petStroeService = new PetStoreServiceImpl();

    public PetStroeService getPetService() {
        return petStroeService;
    }

}

```

综上，获取获取实例化对象可以通过下面三种方式

- class指定类，通过无参构造实例化
- class与factory-method，通过对象的静态工厂方法获取
- factory-name与factory-method 通过反射到指定对象的非静态方法获取实例

## 注入方式

说道`constructor-args`属性，就会说道注入，在Spring 中注入的原则是代码更加整洁。注入有两种主要的注入方式： 基于构造函数的注入和基于Setter的注入

### 基于构造的注入

构造函数中的参数多种类型，所以针对不同情况使用不同的配置

- **参数类型类型明确**

对于明确的参数类型可以直接引用注入，例如下面对象
```
public class NeedDiService {

    private Fun1Dao fun1Dao;

    private Fun2Dao fun2Dao;

    public NeedDiService(Fun1Dao fun1Dao, Fun2Dao fun2Dao) {
        this.fun1Dao = fun1Dao;
        this.fun2Dao = fun2Dao;
    }
}

```

参数不存在歧义，所以不需要显示在***`<constructor-arg /`>***中指定

```
...

<bean id="fun1Dao" class="com.spring.di.Fun1Dao"/>
<bean id="fun2Dao" class="com.spring.di.Fun2Dao"/>
<bean id="service" class="com.spring.di.NeedDiService">
    <constructor-arg ref="fun1Dao"/>
    <constructor-arg ref="fun2Dao"/>
</bean>

...
```

如果参数类型是基本数据类型并且没有歧义如下

```
public class NeedDiBean {
    private int age;
    private String name;

    public NeedDiBean(int age, String name) {
        this.age = age;
        this.name = name;
    }
}
```

在`<constructor-arg`中直接指定值即可

```
<bean name="diffSimpleType" class="com.spring.di.NeedDiBean">
    <constructor-arg value="30"/>
    <constructor-arg value="Json"/>
</bean>
```

- **存在歧义冲突的参数**

如果构造中多个参数都是同一个类型，这时候就需要在`<constructor-arg />`中指定参数的位置

```
public NeedDiBean(String name, String address) {
    this.name = name;
    this.address = address;
}

// xml中
<bean name="ambiguityArgs" class="com.spring.di.NeedDiBean">
    <constructor-arg index="0" value="Jack"/>
    <constructor-arg index="1" value="US.Washington"/>
</bean>
```

除了使用`<constructor-arg />`中指定`index`来处理构造中相同类型的冲突，可以使用`name`字段来解决。

```
<bean name="ambiguityArgs" class="com.spring.di.NeedDiBean">
    <constructor-arg name="name" value="Jack"/>
    <constructor-arg name="address" value="US.Washington"/>
</bean>
```

### 基于Setter的注入

基于`Setter`的注入是在你的实体类被容器反射调用无参构造或者静态工厂实例化后调用setter方法注入。
下面示例展示了通过setter注入

```
public class NeedDiBean {
    private String address;

    public void setAddress(String address) {
        System.out.println("Spring call setAddress di address -->" + address);
        this.address = address;
    }
}

// xml配置

    <bean name="setterDi" class="com.spring.di.NeedDiBean">
        <property name="address" value="Shijiazhuang Qiao Xi Region"/>
    </bean>
```

我们可以看到配置setter方法通过`property`标签配置，`name`配置实体类中需要注入的属性，`value`设置值，如果`address`是`Object`可以使用`ref`如下

```
 <bean name="setterDi" class="com.spring.di.NeedDiBean">
    <property name="address" ref="address"/>
</bean>
<bean name="address" class="com.spring.di.Address" />
```

### 基于构造的注入 VS 基于Setter的注入

对于这两种注入你可能会不知道如何使用，一个很好的原则就是对于必须的依赖通过构造注入，对于一些可选的依赖使用基于方法注入。Spring官方推荐使用构造注入，但是如果你的代码中太多的构造注入，应该考虑重构你的代码。

Setter注入主要用于在类中可以设置默认值的可选依赖。否则在使用依赖以后都会进行非空检查。Setter注入的一个优势就是可以进行重新注入和配置。

对于一些特定类(例如第三方类)，使用依赖注入是很有效的方式。例如某些第三方代码没有对你暴露setter方法，这时候使用构造注入是一个很有效的方式


## 注入的实际应用

如上所述，你座定义的实体类和构造函数参数可以定义为其他对象的引用，也可以定义为内敛定义的值。Spring配置的xml文件通过`<property>`和`<constructor-arg />`元素来支持。

对于多个setter注入的参数可以使用自定义命名空间如下

```
<bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource" destroy-method="close">
    <!-- results in a setDriverClassName(String) call -->
    <property name="driverClassName" value="com.mysql.jdbc.Driver"/>
    <property name="url" value="jdbc:mysql://localhost:3306/mydb"/>
    <property name="username" value="root"/>
    <property name="password" value="masterkaoli"/>
</bean>

可以写作如下：

<beans xmlns="http://www.springframework.org/schema/beans"
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
    xmlns:p="http://www.springframework.org/schema/p"
    xsi:schemaLocation="http://www.springframework.org/schema/beans
    http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="myDataSource" class="org.apache.commons.dbcp.BasicDataSource"
        destroy-method="close"
        p:driverClassName="com.mysql.jdbc.Driver"
        p:url="jdbc:mysql://localhost:3306/mydb"
        p:username="root"
        p:password="masterkaoli"/>

</beans>
```
Spring也支持`java.util.Properties`的注入方式如下：

```
public class PropertiesDiBean {

    private Properties properties;

    public void setProperties(Properties properties) {
        System.out.println("Spring call PropertiesDiBean`s setProperties Function: " + properties.toString());
        this.properties = properties;
    }
}

// Properties通过一组值来配置
<bean name="propertiesDi" class="com.spring.di.PropertiesDiBean">
    <property name="properties">
        <value>
            jdbc.driver.className=com.mysql.jdbc.Driver
            jdbc.url=jdbc:mysql://localhost:3306/mydb
            user.name=Jack
        </value>
    </property>
</bean>
```
Spring会通过`PropertyEditor`机制将`<value />`元素转换成一个`java.util.Properties`实例。这种有效的方式是Spring对属性嵌套`value`元素的一种方式。
另外还可以通过`props>`元素配置

```
<bean name="propertiesDiStyle2" class="com.spring.di.PropertiesDiBean">
    <property name="properties">
        <props>
            <prop key="jdbc.driver.className">com.mysql.jdbc.Driver</prop>
            <prop key="jdbc.url">jdbc:mysql://localhost:3306/mydb</prop>
        </props>
    </property>
</bean>
```
