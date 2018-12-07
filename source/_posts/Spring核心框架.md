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
**Spring也支持`java.util.Properties`的注入**

方式如下：

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

对于实体类中对象的引用，我们通常使用`<ref>`标签，当然我们也可以直接在`<constractor />`或者`<property />`
标签中使用`<baen>`来注入如下：

```
<bean name="outerBean" class="com.spring.bean.OuterBean">

    <property name="target">
        <bean class="com.spring.bean.OuterBean$InnerBean">
            <property name="innerArg1" value="InnerArg1"/>
            <property name="innerArg2" value="InnerArg2"/>
        </bean>
    </property>

    <property name="arg1" value="outerArgs1"/>
    <property name="arg2" value="outerArgs2"/>
</bean>
```

**Spring中对集合的注入**

针对Collection类型的`List`, `Set`, `Map`, `Properties`注入时候使用`<list/>`, `<set/>`, `<map/>`, 和`<props/>`标签， 如下：

```
public class CollectionsDiBean {

    private List<String> list;

    private Map<String, String> map;

    private Set set;

    private Properties properties;

    ......
}

xml注入配置

 <bean class="com.spring.bean.CollectionsDiBean">
        
        <!-- Properties类型注入 -->
        <property name="properties">
            <props>
                <prop key="jdbc.driver.className">com.mysql.jdbc.Driver</prop>
                <prop key="jdbc.url">jdbc:mysql://localhost:3306/mydb</prop>
            </props>
        </property>
        <!-- List类型注入 -->
        <property name="list">
            <list>
                <value>Jack</value>
                <value>JLeo</value>
                <value>Bill</value>
            </list>
        </property>
        <!-- Map类型注入 -->
        <property name="map">
            <map>
                <entry key="Id" value="DM233ms83"/>
                <entry key="name" value="Baiyu"/>
            </map>
        </property>
        <!-- Set类型注入 -->
        <property name="set">
            <set>
                <value>SetValueA</value>
                <value>SetValueB</value>
            </set>
        </property>
    </bean>

```

对于集合类型的标签 `<list/>`, `<set/>`, `<map/>`, 和`<props/>`可以使用属性`merge`来合并子类注入的集合到父类属性中，如下：

```
父类
public class Parent {
    
    private List<String> list;

    public void setList(List<String> list) {
        this.list = list;
    }
}

子类
public class Child extends Parent {

}

xml配置

    <bean id="parentBean" class="com.spring.bean.Parent" >
        <property name="list">
            <list>
                <value>ListItem1</value>
                <value>ListItem2</value>
                <value>ListItem3</value>
            </list>
        </property>
        <property name="address" value="Washington, D.C., USA " />
        <property name="name" value="Benjamin" />
    </bean>
    <bean id="mergeChildBean" class="com.spring.bean.Child" parent="parentBean">
        <property name="list">
            <list merge="true"> <!-- 注意list标签使用属性merge="true" -->
                <value>ListItem4</value>
                <value>ListItem5</value>
                <value>ListItem6</value>
            </list>
        </property>
    </bean>
```

这样Child输出的list结合就是

> list=[ListItem1, ListItem2, ListItem3, ListItem4, ListItem5, ListItem6]}

**Spring支持注入空字符串和null**

如下xml配置将会注入字符串变量为`""`

```
<bean class="ExampleBean">
    <property name="email" value=""/>
</bean>
```

如下xml配置将会注入字符串变量为`null`

```
<bean class="ExampleBean">
    <property name="email">
        <null/>
    </property>
</bean>
```

** Spring 支持使用`p命令空间`和`c命名空间` **

为了方便，Spring可以在xml中配置p的命名空间代替`<property>`，c命名空间替代`<constractor-arg>`使得书写更加方便，如下：

```
public class Pet {

    private String name;
    private String nick;

    public Pet(String name) {
        this.name = name;
    }

    public void setNick(String nick) {
        this.nick = nick;
    }

    @Override
    public String toString() {
        return "Pet{" +
                "name='" + name + '\'' +
                ", nick='" + nick + '\'' +
                '}';
    }
}

<beans xmlns="http://www.springframework.org/schema/beans"
        <!--注意下面命名空间 -->
       xmlns:p="http://www.springframework.org/schema/p"
       xmlns:c="http://www.springframework.org/schema/c"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean class="com.spring.bean.Pet"
          c:name="Sweet"
          p:nick="honey"
    />

</beans>
```

**Spring中使用组合属性**

组合属性就是在一个实体类中的属性是另外一个对象，多层这样的设置，如下：

```
public class ThingOne {

    private Fred fred;

    public ThingOne() {
        // 必须
        this.fred = new Fred();
    }

    public Fred getFred() {
        return fred;
    }

    public class Fred {
        private Bob bob;

        public Fred() {
            // 必须
            this.bob = new Bob();
        }

        public Bob getBob() {
            return bob;
        }
    }

    public class Bob {
        private String sammy;

        public void setSammy(String sammy) {
            this.sammy = sammy;
        }
    }
}
```
这时候如果我们向给`sammy`属性注入值就可以使用下面的配置方式

```
<bean id="something" class="com.spring.bean.ThingOne">
    <property name="fred.bob.sammy" value="Jack"/>
</bean>
```
这里要注意几点：
1. `fred`和`bob`都要在构造中实例化否则会出现空指针
2. 我们可以看到使用组合属性，`fred`和`bob`都是get方法，只有真正注入的sammy是set方法，所以我们也可以推断出Spring执行流程，先实例化`ThingOne`对象，然后调用`getFred()`方法获取`fred`，在调用Fred的`getBob()`方法。获取到Bob对象，最后通过`setSammy()`方法将值`Jack`注入到Bob中


**在Spring中使用depends-on**

同禅的直接依赖都是一个实体类作为另外一个实体类的属性而注入其中，而`depends-on`标签主要适用于间接的依赖。
举个例子：实体类A实例化后使用依赖于B实例化，B实例化后会出发BB事件，而A的使用必须先执行BB事件，但是B和A并没有直接关系。又比如`Dao Bean`实例化之前必须先初始化`Database`，而`Dao Bean`并不需要持有`Database Bean`，如果Database没有初始化，`Dao Bean`实例化也没有实际意义。

通过`depends-on`一个Bean可以依赖多个实体类。

```
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <!-- 如果需要依赖多个实体类则使用,分割 -->
    <bean name="dao" class="research.spring.beanfactory.ch3.Dao" depends-on="database"/>
    <bean id="database" class="research.spring.beanfactory.ch3.Database" />
</beans>
```

**在Spring中使用lazy-init**

通过在xml配置`<bean />`属性`lazy-init`来控制实体类是否需要在加载Spring的上下文`ApplictionContext`时候实例化Bean



**在Spring中使用autowire**

Spring可以在`<bean>`标签中使用`autowire`属性实现自动装配，所谓自动装配也就是你在xml中配置对应的bean不需要通过`property`或者`constructor`来注入，Spring框架自动完成注入。自动装配一共有四种模式

- no

默认的不使用自动装配

- byName

将通过xml中配置的`<bean>`标签的`id`或者`name`来调用set方法装配， 如下

```
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
       xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
       xsi:schemaLocation="http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans.xsd">

    <bean id="dept" class="com.spring.bean.Department">
        <property name="name" value="Develop"/>
    </bean>

    <bean id="employee" class="com.spring.bean.Employee" autowire="byName"/>

</beans>
```
Employee中set方法名称必须与Department配置的id或者name保持一直否则无法自动装配


- byType

当设置byType后不需要保持id或者name与类中set方法名称保持一直，Spring会自动根据属性的类型通过set方法注入

```
...
<bean id="dept" class="com.spring.bean.Department">
    <property name="name" value="Develop"/>
</bean>
<bean id="employee" class="com.spring.bean.Employee" autowire="byType"/>

....
```
当xml中配置多个Department时候将会注入失败，Spring无法识别应该装配哪个，反过来如果实体类中有多个相同类型的属性，xml中配置一个，则多个属性都会注入xml配置的实体类

- constructor

顾名思义通过构造自动装载


***注：当配置了byTyo或者byName装配时候，优先级会低于property配置的注入***

以上集中方式可以实现自动装配对象，如果对于莫个对象你不想它被别的实体类装配可以在它的`<bean/>`标签使用`autowire-candidate`属性设置为false即可

>  <bean name="address" class="com.spring.bean.Address" p:name="Hebei" autowire-candidate="false"/>



