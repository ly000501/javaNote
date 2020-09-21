# mybatis-plus的使用 ------ 入门

# 一、mybatis-plus简介：

Mybatis-Plus（简称MP）是一个 Mybatis 的增强工具，在 Mybatis 的基础上只做增强不做改变，为简化开发、提高效率而生。这是官方给的定义，关于mybatis-plus的更多介绍及特性，可以参考[mybatis-plus官网](https://links.jianshu.com/go?to=http%3A%2F%2Fmp.baomidou.com%2F%23%2F)。那么它是怎么增强的呢？其实就是它已经封装好了一些crud方法，我们不需要再写xml了，直接调用这些方法就行，就类似于JPA。

# 二、spring整合mybatis-plus:

正如官方所说，mybatis-plus在mybatis的基础上只做增强不做改变，因此其与spring的整合亦非常简单。只需把mybatis的依赖换成mybatis-plus的依赖，再把sqlSessionFactory换成mybatis-plus的即可。接下来看具体操作：
 **1、pom.xml:**
 核心依赖如下：



```xml
        <!-- spring -->
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-context</artifactId>
            <version>4.3.14.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-orm</artifactId>
            <version>4.3.14.RELEASE</version>
        </dependency>
        <dependency>
            <groupId>org.springframework</groupId>
            <artifactId>spring-test</artifactId>
            <version>4.3.14.RELEASE</version>
            <scope>test</scope>
        </dependency>
        <!-- mp 依赖 -->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus</artifactId>
            <version>2.3</version>
        </dependency>
```

**注意：**这些是核心依赖，本项目还用到了mysql驱动、c3p0、日志（slf4j-api，slf4j-log4j2）、lombok。集成mybatis-plus要把mybatis、mybatis-spring去掉，避免冲突；lombok是一个工具，添加了这个依赖，开发工具再安装Lombok插件，就可以使用它了，最常用的用法就是在实体类中使用它的@Data注解，这样实体类就不用写set、get、toString等方法了。关于Lombok的更多用法，请自行百度。

**2、log4j.xml:**



```xml
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE log4j:configuration SYSTEM "log4j.dtd">
<log4j:configuration xmlns:log4j="http://jakarta.apache.org/log4j/">
    <appender name="STDOUT" class="org.apache.log4j.ConsoleAppender">
        <param name="Encoding" value="UTF-8" />
        <layout class="org.apache.log4j.PatternLayout">
            <param name="ConversionPattern" value="%-5p %d{MM-dd
HH:mm:ss,SSS} %m (%F:%L) \n" />
        </layout>
    </appender>
    <logger name="java.sql">
        <level value="debug" />
    </logger>
    <logger name="org.apache.ibatis">
        <level value="info" />
    </logger>
    <root>
        <level value="debug" />
        <appender-ref ref="STDOUT" />
    </root>
</log4j:configuration>
```

**3、jdbc.properties:**



```ruby
jdbc.driver=com.mysql.jdbc.Driver
jdbc.url=jdbc:mysql:///数据库名?useUnicode=true&characterEncoding=utf8
jdbc.username=#
jdbc.password=#
```

**4、mybatis-config.xml:**



```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration
PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
"http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
</configuration>
```

**注：**因为是与spring整合，所有mybatis-plus的大部分都写在spring的配置文件中，这里定义一个空的mybatis-config.xml即可。

**5、spring-dao.xml:**



```xml
<?xml version="1.0" encoding="UTF-8"?>    
<beans xmlns="http://www.springframework.org/schema/beans"    
    xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"   
    xmlns:p="http://www.springframework.org/schema/p"  
    xmlns:aop="http://www.springframework.org/schema/aop"   
    xmlns:context="http://www.springframework.org/schema/context"  
    xmlns:jee="http://www.springframework.org/schema/jee"  
    xmlns:tx="http://www.springframework.org/schema/tx"  
    xsi:schemaLocation="    
        http://www.springframework.org/schema/aop http://www.springframework.org/schema/aop/spring-aop-4.0.xsd  
        http://www.springframework.org/schema/beans http://www.springframework.org/schema/beans/spring-beans-4.0.xsd  
        http://www.springframework.org/schema/context http://www.springframework.org/schema/context/spring-context-4.0.xsd  
        http://www.springframework.org/schema/jee http://www.springframework.org/schema/jee/spring-jee-4.0.xsd  
        http://www.springframework.org/schema/tx http://www.springframework.org/schema/tx/spring-tx-4.0.xsd">    
        
    <!-- 配置整合mybatis-plus过程 -->
    <!-- 1、配置数据库相关参数properties的属性：${url} -->
    <context:property-placeholder location="classpath:jdbc.properties" />
    <!-- 2、配置数据库连接池 -->
    <bean id="dataSource" class="com.mchange.v2.c3p0.ComboPooledDataSource">
        <property name="driverClass" value="${jdbc.driver}"/>
        <property name="jdbcUrl" value="${jdbc.url}"/>
        <property name="user" value="${jdbc.username}"/>
        <property name="password" value="${jdbc.password}"/>
    </bean>
    <!-- mybatis的sqlsessionFactorybean：org.mybatis.spring.SqlSessionFactoryBean-->
    <!-- 3、配置mybatis-plus的sqlSessionFactory -->
    <bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.spring.MybatisSqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="typeAliasesPackage" value="com.zhu.mybatisplus.entity"/>
    </bean>
    <!-- 4、DAO接口所在包名，Spring会自动查找其下的类 -->
    <bean class="org.mybatis.spring.mapper.MapperScannerConfigurer">
        <property name="basePackage" value="com.zhu.mybatisplus.dao" />
        <property name="sqlSessionFactoryBeanName" value="sqlSessionFactory"/>
    </bean> 
</beans>
```

**6、entity:**



```kotlin
@Data
@TableName(value = "tb_employee")//指定表名
public class Employee {
    //value与数据库主键列名一致，若实体类属性名与表主键列名一致可省略value
    @TableId(value = "id",type = IdType.AUTO)//指定自增策略
    private Integer id;
    //若没有开启驼峰命名，或者表中列名不符合驼峰规则，可通过该注解指定数据库表中的列名，exist标明数据表中有没有对应列
    @TableField(value = "last_name",exist = true)
    private String lastName;
    private String email;
    private Integer gender;
    private Integer age;
}
```

**7、mapper:**



```java
public interface EmplopyeeDao extends BaseMapper<Employee> {
}
```

这样就完成了mybatis-plus与spring的整合。首先是把mybatis和mybatis-spring依赖换成mybatis-plus的依赖，然后把sqlsessionfactory换成mybatis-plus的，然后实体类中添加`@TableName`、`@TableId`等注解，最后mapper继承`BaseMapper`即可。

**8、测试：**



```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration({"classpath:spring/spring-dao.xml"})
public class test {
    @Autowired
    private DataSource dataSource;
    @Test
    public void testDataSource() throws SQLException {
        System.out.println(dataSource.getConnection());
    }
}
```

运行该junit，可输出获取到的连接，说明整合没问题：


![img](https:////upload-images.jianshu.io/upload_images/11531502-2a0d958939bdcde8.png?imageMogr2/auto-orient/strip|imageView2/2/w/863/format/webp)

image.png


 本文所有代码本人均亲自测试过，本文涉及代码又较多，为了不影响篇幅，故非必要处不再截图。接下来的所有操作都是基于此整合好的项目。

# 三、mp的通用crud:

**需求：**
 存在一张 tb_employee 表，且已有对应的实体类 Employee，实现tb_employee 表的 CRUD 操作我们需要做什么呢？
 **基于 Mybatis：**
 需要编写 EmployeeMapper 接口，并在 EmployeeMapper.xml 映射文件中手动编写 CRUD 方法对应的sql语句。
 **基于 MP：**
 只需要创建 EmployeeMapper 接口, 并继承 BaseMapper 接口。
 我们已经有了Employee、tb_employee了，并且EmployeeDao也继承了BaseMapper了，接下来就使用crud方法。

**1、insert操作：**



```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration({"classpath:spring/spring-dao.xml"})
public class test {
    @Autowired
    private EmplopyeeDao emplopyeeDao;
    @Test
    public void testInsert(){
        Employee employee = new Employee();
        employee.setLastName("东方不败");
        employee.setEmail("dfbb@163.com");
        employee.setGender(1);
        employee.setAge(20);
        emplopyeeDao.insert(employee);
        //mybatisplus会自动把当前插入对象在数据库中的id写回到该实体中
        System.out.println(employee.getId());
    }
}
```

执行添加操作，直接调用insert方法传入实体即可。

**2、update操作：**



```java
@Test
public void testUpdate(){
        Employee employee = new Employee();
        employee.setId(1);
        employee.setLastName("更新测试");
        //emplopyeeDao.updateById(employee);//根据id进行更新，没有传值的属性就不会更新
        emplopyeeDao.updateAllColumnById(employee);//根据id进行更新，没传值的属性就更新为null
}
```

**注：**注意这两个update操作的区别，`updateById`方法，没有传值的字段不会进行更新，比如只传入了lastName，那么age、gender等属性就会保留原来的值；`updateAllColumnById`方法，顾名思义，会更新所有的列，没有传值的列会更新为null。

**3、select操作：**

**(1)、**根据id查询：



```undefined
Employee employee = emplopyeeDao.selectById(1);
```

**(2)、**根据条件查询一条数据：



```cpp
Employee employeeCondition = new Employee();
employeeCondition.setId(1);
employeeCondition.setLastName("更新测试");
//若是数据库中符合传入的条件的记录有多条，那就不能用这个方法，会报错
Employee employee = emplopyeeDao.selectOne(employeeCondition);
```

**注：**这个方法的sql语句就是`where id = 1 and last_name = 更新测试`，若是符合这个条件的记录不止一条，那么就会报错。

**(3)、**根据查询条件返回多条数据：
 当符合指定条件的记录数有多条时，上面那个方法就会报错，就应该用这个方法。



```dart
Map<String,Object> columnMap = new HashMap<>();
columnMap.put("last_name","东方不败");//写表中的列名
columnMap.put("gender","1");
List<Employee> employees = emplopyeeDao.selectByMap(columnMap);
System.out.println(employees.size());
```

**注：**查询条件用map集合封装，columnMap，写的是数据表中的列名，而非实体类的属性名。比如属性名为lastName，数据表中字段为last_name，这里应该写的是last_name。selectByMap方法返回值用list集合接收。

**(4)、**通过id批量查询：



```csharp
List<Integer> idList = new ArrayList<>();
idList.add(1);
idList.add(2);
idList.add(3);
List<Employee> employees = emplopyeeDao.selectBatchIds(idList);
System.out.println(employees);
```

**注：**把需要查询的id都add到list集合中，然后调用selectBatchIds方法，传入该list集合即可，该方法返回的是对应id的所有记录，所有返回值也是用list接收。

**(5)、**分页查询：



```csharp
List<Employee> employees = emplopyeeDao.selectPage(new Page<>(1,2),null);
System.out.println(employees);
```

**注：**selectPage方法就是分页查询，在page中传入分页信息，后者为null的分页条件，这里先让其为null，讲了条件构造器再说其用法。这个分页其实并不是物理分页，而是内存分页。也就是说，查询的时候并没有limit语句。等配置了分页插件后才可以实现真正的分页。

**4、delete操作：**

**(1)、**根据id删除：



```css
emplopyeeDao.deleteById(1);
```

**(2)、**根据条件删除：



```dart
Map<String,Object> columnMap = new HashMap<>();
columnMap.put("gender",0);
columnMap.put("age",18);
emplopyeeDao.deleteByMap(columnMap);
```

**注：**该方法与selectByMap类似，将条件封装在columnMap中，然后调用deleteByMap方法，传入columnMap即可，返回值是Integer类型，表示影响的行数。

**(3)、**根据id批量删除：



```csharp
 List<Integer> idList = new ArrayList<>();
 idList.add(1);
 idList.add(2);
 emplopyeeDao.deleteBatchIds(idList);
```

**注：**该方法和selectBatchIds类似，把需要删除的记录的id装进idList，然后调用deleteBatchIds，传入idList即可。



# 四、全局策略配置：

通过上面的小案例我们可以发现，实体类需要加@TableName注解指定数据库表名，通过@TableId注解指定id的增长策略。实体类少倒也无所谓，实体类一多的话也麻烦。所以可以在spring-dao.xml的文件中进行全局策略配置。



```xml
<!-- 5、mybatisplus的全局策略配置 -->
<bean id="globalConfiguration" class="com.baomidou.mybatisplus.entity.GlobalConfiguration">
        <!-- 2.3版本后，驼峰命名默认值就是true，所以可不配置 -->
        <!--<property name="dbColumnUnderline" value="true"/>-->
        <!-- 全局主键自增策略，0表示auto -->
        <property name="idType" value="0"/>
        <!-- 全局表前缀配置 -->
        <property name="tablePrefix" value="tb_"/>
</bean>
```

这里配置了还没用，还需要在sqlSessionFactory中注入配置才会生效。如下：



```xml
<!-- 3、配置mybatisplus的sqlSessionFactory -->
<bean id="sqlSessionFactory" class="com.baomidou.mybatisplus.spring.MybatisSqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="typeAliasesPackage" value="com.zhu.mybatisplus.entity"/>
        <!-- 注入全局配置 -->
        <property name="globalConfig" ref="globalConfiguration"/>
</bean>
```

如此一来，实体类中的@TableName注解和@TableId注解就可以去掉了。


# 五、条件构造器(EntityWrapper)：

以上基本的 CRUD 操作，我们仅仅需要继承一个 BaseMapper 即可实现大部分单表 CRUD 操作。BaseMapper 提供了多达 17 个方法供使用, 可以极其方便的实现单一、批量、分页等操作，极大的减少开发负担。但是mybatis-plus的强大不限于此，请看如下需求该如何处理：
 **需求：**
 我们需要分页查询 tb_employee 表中，年龄在 18~50 之间性别为男且姓名为 xx 的所有用户，这时候我们该如何实现上述需求呢？
 **使用MyBatis :** 需要在 SQL 映射文件中编写带条件查询的 SQL,并用PageHelper 插件完成分页. 实现以上一个简单的需求，往往需要我们做很多重复单调的工作。
 **使用MP:** 依旧不用编写 SQL 语句，MP 提供了功能强大的条件构造器 ------  EntityWrapper。

**接下来就直接看几个案例体会EntityWrapper的使用。**

**1、分页查询年龄在18 - 50且gender为0、姓名为tom的用户：**



```dart
List<Employee> employees = emplopyeeDao.selectPage(new Page<Employee>(1,3),
     new EntityWrapper<Employee>()
        .between("age",18,50)
        .eq("gender",0)
        .eq("last_name","tom")
);
```

**注：**由此案例可知，分页查询和之前一样，new 一个page对象传入分页信息即可。至于分页条件，new 一个EntityWrapper对象，调用该对象的相关方法即可。between方法三个参数，分别是column、value1、value2，该方法表示column的值要在value1和value2之间；eq是equals的简写，该方法两个参数，column和value，表示column的值和value要相等。注意column是数据表对应的字段，而非实体类属性字段。

**2、查询gender为0且名字中带有老师、或者邮箱中带有a的用户：**



```dart
List<Employee> employees = emplopyeeDao.selectList(
                new EntityWrapper<Employee>()
               .eq("gender",0)
               .like("last_name","老师")
                //.or()//和or new 区别不大
               .orNew()
               .like("email","a")
);
```

**注：**未说分页查询，所以用selectList即可，用EntityWrapper的like方法进行模糊查询，like方法就是指column的值包含value值，此处like方法就是查询last_name中包含“老师”字样的记录；“或者”用or或者orNew方法表示，这两个方法区别不大，用哪个都可以，可以通过控制台的sql语句自行感受其区别。

**3、查询gender为0，根据age排序，简单分页：**



```dart
List<Employee> employees = emplopyeeDao.selectList(
                new EntityWrapper<Employee>()
                .eq("gender",0)
                .orderBy("age")//直接orderby 是升序，asc
                .last("desc limit 1,3")//在sql语句后面追加last里面的内容(改为降序，同时分页)
);
```

**注：**简单分页是指不用page对象进行分页。orderBy方法就是根据传入的column进行升序排序，若要降序，可以使用orderByDesc方法，也可以如案例中所示用last方法；last方法就是将last方法里面的value值追加到sql语句的后面，在该案例中，最后的sql语句就变为`select ······ order by desc limit 1, 3`，追加了`desc limit 1,3`所以可以进行降序排序和分页。

**4、分页查询年龄在18 - 50且gender为0、姓名为tom的用户：**
 条件构造器除了EntityWrapper，还有Condition。用Condition来处理一下这个需求：



```dart
 List<Employee> employees = emplopyeeDao.selectPage(
                new Page<Employee>(1,2),
                Condition.create()
                        .between("age",18,50)
                        .eq("gender","0")
 );
```

**注：**Condition和EntityWrapper的区别就是，创建条件构造器时，EntityWrapper是new出来的，而Condition是调create方法创建出来。

**5、根据条件更新：**



```java
@Test
public void testEntityWrapperUpdate(){
        Employee employee = new Employee();
        employee.setLastName("苍老师");
        employee.setEmail("cjk@sina.com");
        employee.setGender(0);
        emplopyeeDao.update(employee,
                new EntityWrapper<Employee>()
                .eq("last_name","tom")
                .eq("age",25)
        );
}
```

**注：**该案例表示把last_name为tom，age为25的所有用户的信息更新为employee中设置的信息。

**6、根据条件删除：**



```cpp
emplopyeeDao.delete(
        new EntityWrapper<Employee>()
        .eq("last_name","tom")
        .eq("age",16)
);
```

**注：**该案例表示把last_name为tom、age为16的所有用户删除。


# 总结：

以上便是mybatis-plus的入门教程，介绍了其如何与spring整合、通用crud的使用、全局策略的配置以及条件构造器的使用，但是这并不是MP的所有内容，其强大不限于此。避免篇幅过长，想了解mybatis-plus的更多用法请参考《[mybatis-plus的使用 ------ 进阶](https://www.jianshu.com/p/a4d5d310daf8)》，同时在文末会给出案例的源码。







# mybatis-plus的使用 ------ 进阶

# 一、ActiveRecord:

Active Record(活动记录)，是一种领域模型模式，特点是一个模型类对应关系型数据库中的一个表，而模型类的一个实例对应表中的一行记录。ActiveRecord 一直广受动态语言（ PHP 、 Ruby 等）的喜爱，而 Java 作为准静态语言，对于 ActiveRecord 往往只能感叹其优雅，所以 MP 也在 AR 道路上进行了一定的探索，仅仅需要让实体类继承 Model 类且实现主键指定方法，即可开启 AR 之旅。接下来看具体代码：

**1、entity:**



```java
@Data
public class User extends Model<User> {
    private Integer id;
    private String name;
    private Integer age;
    private Integer gender;
    //重写这个方法，return当前类的主键
    @Override
    protected Serializable pkVal() {
        return id;
    }
}
```

**注：**实体类继承Model类，重写pkVal方法。

**2、mapper:**



```java
public interface UserDao extends BaseMapper<User> {
}
```

**注：**虽然AR模式用不到该接口，但是一定要定义，否则使用AR时会报空指针异常。

**3、使用AR:**

**(1)、**AR插入操作：



```java
@RunWith(SpringJUnit4ClassRunner.class)
@ContextConfiguration({"classpath:spring/spring-dao.xml"})
public class TestAR {
    @Test
    public void testArInsert(){
        User user = new User();
        user.setName("林青霞");
        user.setAge(22);
        user.setGender(1);
        boolean result = user.insert();
        System.out.println(result);
    }
}
```

![img](https:////upload-images.jianshu.io/upload_images/11531502-8eecfba7c07de941.png?imageMogr2/auto-orient/strip|imageView2/2/w/749/format/webp)

image.png

**注：**可以看到我们并不需要注入mapper接口，不过正如刚才所说，不使用但还是要定义，否则会报错。AR操作是通过对象本身调用相关方法，比如要insert一个user，那就用这个user调用insert方法即可。返回值为布尔类型，由上图可看到返回了true，是操作成功的。

**(2)、**AR更新操作：



```java
    @Test
    public void testArUpdate(){
        User user = new User();
        user.setId(1);
        user.setName("刘亦菲");
        boolean result = user.updateById();
        System.out.println(result);
    }
```

**注：**user调用updateById方法，将id为1的用户进行更新。

**(3)、**AR查询操作：



```csharp
    @Test
    public void testArSelect(){
        User user = new User();
        //1、根据id查询
        //user = user.selectById(1);
        //或者这样用
        //user.setId(1);
        //user = user.selectById();

        //2、查询所有
        //List<User> users = user.selectAll();

        //3、根据条件查询
        //List<User> users = user.selectList(new EntityWrapper<User>().like("name","刘"));

        //4、查询符合条件的总数
        int result = user.selectCount(new EntityWrapper<User>().eq("gender",1));
        System.out.println(result);
    }
```

**注：**上面的代码涉及到了四个不同的查询操作，其实用法与MP的BaseMapper提供的方法的用法差不多，只不过这里是实体对象调用。

**(4)、**AR删除操作：



```java
@Test
    public void testArDelete(){
        User user = new User();
        //删除数据库中不存在的数据也是返回true
        //1、根据id删除数据
        //boolean result = user.deleteById(1);
        //或者这样写
        //user.setId(1);
        //boolean result = user.deleteById();

        //2、根据条件删除
        boolean result = user.delete(new EntityWrapper<User>().like("name","玲"));
        System.out.println(result);
    }
```

**注：**这里介绍了两个删除方法，代码中已有注释说明。需要注意的是，删除数据库中不存在的数据，结果也是true。

**(5)、**AR分页操作：



```csharp
@Test
    public void testArPage(){
       User user = new User();
       Page<User> page =
               user.selectPage(new Page<>(1,4),
               new EntityWrapper<User>().like("name","刘"));
       List<User> users = page.getRecords();
       System.out.println(users);
    }
```

**注：**这个分页方法和BaseMapper提供的分页一样都是内存分页，并非物理分页，因为sql语句中没用limit，和BaseMapper的selectPage方法一样，配置了分页插件后就可以实现真正的物理分页。AR的分页方法与BaseMapper提供的分页方法不同的是，BaseMapper的selectPage方法返回值是查询到的记录的list集合，而AR的selectPage方法返回的是page对象，该page对象封装了查询到的信息，可以通过getRecords方法获取信息。

# 二、插件的配置：

MP提供了很多好用的插件，而且配置简单，使用方便。接下来一起看看MP的插件如何使用。

**1、分页插件：**
 之前就有说到，BaseMapper的selectPage方法和AR提供的selectPage方法都不是物理分页，需要配置分页插件后才是物理分页，那么现在就来看看如何配置这个插件。



```xml
<!-- 3、配置mybatisplus的sqlSessionFactory -->
    <bean id="sqlSessionFactory" class=
                       "com.baomidou.mybatisplus.spring.MybatisSqlSessionFactoryBean">
        <property name="dataSource" ref="dataSource" />
        <property name="configLocation" value="classpath:mybatis-config.xml"/>
        <property name="typeAliasesPackage" value="com.zhu.mybatisplus.entity"/>
        <!-- 注入全局配置 -->
        <property name="globalConfig" ref="globalConfiguration"/>
        <!-- 配置插件 -->
        <property name="plugins">
            <list>
                <!-- 分页插件 -->
                <bean class="com.baomidou.mybatisplus.plugins.PaginationInterceptor"/>
            </list>
        </property>
    </bean>
```

**注：**在sqlSessionFactory这个bean中，通过``配置插件，接下来的所有插件都配置在这个list中。



```csharp
@Test
    public void testPage() {
        //配置了分页插件后，还是和以前一样的使用selectpage方法，
        //但是现在就是真正的物理分页了，sql语句中有limit了
        Page<Employee> page = new Page<>(1, 2);
        List<Employee> employeeList =
                emplopyeeDao.selectPage(page, null);
        System.out.println(employeeList);
        System.out.println("================= 相关的分页信息 ==================");
        System.out.println("总条数:" + page.getTotal());
        System.out.println("当前页码:" + page.getCurrent());
        System.out.println("总页数:" + page.getPages());
        System.out.println("每页显示条数:" + page.getSize());
        System.out.println("是否有上一页:" + page.hasPrevious());
        System.out.println("是否有下一页:" + page.hasNext());
        //还可以将查询到的结果set进page对象中
        page.setRecords(employeeList);
    }
```

![img](https:////upload-images.jianshu.io/upload_images/11531502-a4a6dd1a9a0dfa85.png?imageMogr2/auto-orient/strip|imageView2/2/w/710/format/webp)

image.png



由图可知，sql语句中已经有了limit，是物理分页了。



![img](https:////upload-images.jianshu.io/upload_images/11531502-e6189c835369c081.png?imageMogr2/auto-orient/strip|imageView2/2/w/690/format/webp)

image.png



也可以通过page调用相关方法获取到相关的分页信息，而且还可以把查询到的结果set回page对象中，方便前端使用。

**2、性能分析插件：**
 在plugin的list中添加如下bean即可开启性能分析插件：



```xml
 <!-- 输出每条SQL语句及其执行时间，生产环境不建议使用该插件 -->
 <bean class="com.baomidou.mybatisplus.plugins.PerformanceInterceptor">
        <property name="format" value="true"/><!-- 格式化SQL语句 -->
        <property name="maxTime" value="1000"/><!-- sql执行时间超过value值就会停止执行，
                                                   单位是毫秒 -->
 </bean>
```

**注：**这个性能分析插件配置了两个属性，第一个是格式化sql语句，设置为true后，sql语句格式就像上面的截图中的一样；第二个属性是sql语句执行的最大时间，超过value值就会报错，这里表示超过1000毫秒就会停止执行sql语句。

**3、执行分析插件：**



```xml
<!-- 如果是对全表的删除或更新操作，就会终止该操作 -->
<bean class="com.baomidou.mybatisplus.plugins.SqlExplainInterceptor">
       <property name="stopProceed" value="true"/>
</bean>
```

**注：**这个插件配置了一个属性，stopProceed设置为true后，如果执行的是删除表中全部内容，那就会抛出异常，终止该操作。该插件主要是防止手抖误删数据。



```java
@Test
public void testSqlExplain(){
        //条件为null，就是删除全表，执行分析插件会终止该操作
    emplopyeeDao.delete(null);
}
```

运行该junit测试，可以看到报如下错误，说明该插件生效了。



![img](https:////upload-images.jianshu.io/upload_images/11531502-e0d8379c8cb1b769.png?imageMogr2/auto-orient/strip|imageView2/2/w/668/format/webp)

image.png

# 三、MP的逆向工程：

**MyBatis 的代码生成器**基于xml文件进行生成，可生成: 实体类、Mapper 接口、Mapper 映射文件。
 **MP 的代码生成器**基于Java代码进行生成，可生成: 实体类(可以选择是否支持 AR)、Mapper 接口、Mapper 映射文件、 Service 层、Controller 层。
 **1、添加依赖：**



```xml
        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <version>5.1.37</version>
        </dependency>
        <!-- mp 依赖 -->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus</artifactId>
            <version>2.3</version>
        </dependency>
        <!-- mybatisplus逆向工程需要模板引擎，用freemaker也行 -->
        <dependency>
            <groupId>org.apache.velocity</groupId>
            <artifactId>velocity-engine-core</artifactId>
            <version>2.0</version>
        </dependency>
```

**注：**上面是必须的三个依赖，为了可以在控制台直观的看到生成情况，可以添加日志包(slf4j-api和slf4j-log4j2)，为了让生成的代码不会报错，还可以根据情况添加spring相关的依赖、lombok依赖等。

**2、生成器示例代码：**



```java
/**
 * @author: zhu
 * @date: 2018/8/20 11:17
 * mybatis-plus逆向工程示例代码
 */
public class test {
    @Test
    public void testGenerator(){
        //1、全局配置
        GlobalConfig config = new GlobalConfig();
        config.setActiveRecord(true)//开启AR模式
              .setAuthor("zhu")//设置作者
              //生成路径(一般都是生成在此项目的src/main/java下面)
              .setOutputDir("E:\\develop\\Java\\workspace\\ideaworkspace\\mpg\\src\\main\\java")
              .setFileOverride(true)//第二次生成会把第一次生成的覆盖掉
              .setIdType(IdType.AUTO)//主键策略
              .setServiceName("%sService")//生成的service接口名字首字母是否为I，这样设置就没有I
              .setBaseResultMap(true)//生成resultMap
              .setBaseColumnList(true);//在xml中生成基础列
        //2、数据源配置
        DataSourceConfig dataSourceConfig = new DataSourceConfig();
        dataSourceConfig.setDbType(DbType.MYSQL)//数据库类型
                        .setDriverName("com.mysql.jdbc.Driver")
                        .setUrl("jdbc:mysql:///数据库名")
                        .setUsername("数据库用户名")
                        .setPassword("数据库密码");
        //3、策略配置
        StrategyConfig strategyConfig = new StrategyConfig();
        strategyConfig.setCapitalMode(true)//开启全局大写命名
                      .setDbColumnUnderline(true)//表名字段名使用下划线
                      .setNaming(NamingStrategy.underline_to_camel)//下划线到驼峰的命名方式
                      .setTablePrefix("tb_")//表名前缀
                      .setEntityLombokModel(true)//使用lombok
                      .setInclude("表1","表2");//逆向工程使用的表
        //4、包名策略配置
        PackageConfig packageConfig = new PackageConfig();
        packageConfig.setParent("com.zhu.mpg")//设置包名的parent
                     .setMapper("mapper")
                     .setService("service")
                     .setController("controller")
                     .setEntity("entity")
                     .setXml("mapper");//设置xml文件的目录
        //5、整合配置
        AutoGenerator autoGenerator = new AutoGenerator();
        autoGenerator.setGlobalConfig(config)
                     .setDataSource(dataSourceConfig)
                     .setStrategy(strategyConfig)
                     .setPackageInfo(packageConfig);
        //6、执行
        autoGenerator.execute();
    }
}
```

**注：**以上便是示例代码，只要运行该junit测试，就会生成entity、mapper接口、mapper的xml文件、service、serviceImpl、controller代码。每一个设置代码中均有详细注释，此处不再赘述。

# 四、自定义全局操作：

##### (一)、AutoSqlInjector ：

BaseMapper提供了17个常用方法，但是有些需求这些方法还是不能很好的实现，那么怎么办呢？大家肯定会想到是在xml文件中写sql语句解决。这样确实可以，因为MP是只做增强不做改变，我们完全可以按照mybatis的原来的方式来解决。不过MP也提供了另一种解决办法，那就是自定义全局操作。所谓自定义全局操作，也就是我们可以在mapper中自定义一些方法，然后通过某些操作，让自定义的这个方法也能像BaseMapper的内置方法，供全局调用。接下来就看看如何实现(以deleteAll方法为例)。
 **1、在mapper接口中定义方法：**



```java
public interface EmplopyeeDao extends BaseMapper<Employee> {
    int deleteAll();
}
```



```java
public interface UserDao extends BaseMapper<User> {
    int deleteAll();
}
```

在这两个mapper接口中都定义了deleteAll方法。

**2、编写自定义注入类：**



```java
public class MySqlInjector extends AutoSqlInjector {
    @Override
    public void inject(Configuration configuration, MapperBuilderAssistant builderAssistant, 
                              Class<?> mapperClass, Class<?> modelClass, TableInfo table) {
        /* 添加一个自定义方法 */
        deleteAllUser(mapperClass, modelClass, table);
        System.out.println(table.getTableName());
    }

    public void deleteAllUser(Class<?> mapperClass, Class<?> modelClass, TableInfo table) {
        /* 执行 SQL ，动态 SQL 参考类 SqlMethod */
        String sql = "delete from " + table.getTableName();
        /* mapper 接口方法名一致 */
        String method = "deleteAll";
        SqlSource sqlSource = languageDriver.createSqlSource(configuration, sql, modelClass);
        this.addDeleteMappedStatement(mapperClass, method, sqlSource);
    }
}
```

**注：**该类继承AutoSqlInjector，重写inject方法。然后编写sql语句，指定mapper接口中的方法，最后调用addDeleteMappedStatement方法即可。

**3、在spring配置文件中配置：**



```xml
<!-- 定义自定义注入器 -->
<bean class="com.zhu.mybatisplus.injector.MySqlInjector" id="mySqlInjector"/>
```



```xml
<!-- 5、mybatisplus的全局策略配置 -->
    <bean id="globalConfiguration" class="com.baomidou.mybatisplus.entity.GlobalConfiguration">
        <property name="idType" value="0"/>
        <property name="tablePrefix" value="tb_"/>
        <!-- 注入自定义全局操作 -->
        <property name="sqlInjector" ref="mySqlInjector"/>
    </bean>
```

**注：**先把刚才自定义的类注册成bean，然后在全局策略配置的bean中引用自定义类的bean即可。

**4、测试：**



```csharp
@Test
public void testMySqlInjector(){
    Integer result = userDao.deleteAll();
    System.out.println(result);
}

@Test
public void testMySqlInjector2(){
    Integer result = emplopyeeDao.deleteAll();
    System.out.println(result);
}
```

**注：**经测试，当userDao调用deleteAll方法时，会删除tb_user表的所有数据，employeeDao调用deleteAll方法时，会删除tb_employee表的所有数据。说明deleteAll方法是有效的。不过在运行这两个测试时，由于是全表删除操作，所有要先把执行分析插件关了。

##### (二)、逻辑删除：

其实数据并不会轻易的删除掉，毕竟数据收集不易，所以就有了逻辑删除。逻辑删除: 并不会真正的从数据库中将数据删除掉，而是将当前被删除的这条数据中的一个逻辑删除字段置为删除状态，比如该数据有一个字段logic_flag，当其值为1表示未删除，值为-1表示删除，那么逻辑删除就是将1变成-1。
 **1、数据表：**
 在数据表中需要添加逻辑删除字段(logic_flag)。


![img](https:////upload-images.jianshu.io/upload_images/11531502-ec05bfdca3f421e5.png?imageMogr2/auto-orient/strip|imageView2/2/w/631/format/webp)

image.png



**2、实体类：**



```java
@Data
public class User{
    private Integer id;
    private String name;
    private Integer age;
    private Integer gender;
    @TableLogic //标记逻辑删除属性
    private Integer logicFlag;
}
```

**注：**数据库中逻辑删除字段是logic_flag，所以实体类中的logicFlag需要用@TableLogic注解标记。

**3、mapper:**



```java
public interface UserDao extends BaseMapper<User> {
}
```

**4、配置逻辑删除：**
 需要在spring-dao.xml中做如下配置：
 首先定义逻辑删除的bean：



```xml
<!-- 逻辑删除 -->
<bean class="com.baomidou.mybatisplus.mapper.LogicSqlInjector" id="logicSqlInjector"/>
```

再在全局配置的bean中注入逻辑删除以及逻辑删除值：



```xml
<!-- 5、mybatisplus的全局策略配置 -->
    <bean id="globalConfiguration" class="com.baomidou.mybatisplus.entity.GlobalConfiguration">
        <!-- 此处省略其他全局配置 -->
        <!-- 注入自定义全局操作，做逻辑删除时需要先注释掉 -->
        <!--<property name="sqlInjector" ref="mySqlInjector"/>-->
        <!-- 注入逻辑删除，先要把自定义的注释掉 -->
        <property name="sqlInjector" ref="logicSqlInjector"/>
        <!-- 注入逻辑删除值 -->
        <property name="logicDeleteValue" value="-1"/><!-- -1是删除状态 -->
        <property name="logicNotDeleteValue" value="1"/><!-- 1是未删除状态 -->
    </bean>
```

**注：**因为逻辑删除实际上也是一个sqlInjector，所以先要把刚才做自定义全局操作时注入的自定义全局操作注释掉，上面代码中已有详细注释说明。

**6、测试：**



```csharp
@Test
    public void testLogicDelete(){
        Integer result = userDao.deleteById(1);
        System.out.println(result);
        //User user = userDao.selectById(1);
        //System.out.println(user);
    }
```

**注：**运行该测试，执行删除操作的时候，真正执行的sql语句是`UPDATE tb_user SET logic_flag=-1 WHERE id=?`，就是把逻辑删除字段的值设置为-1；当逻辑删除字段的值是-1时再执行查询操作，sql是`SELECT ... FROM tb_user WHERE id=? AND logic_flag=1`，所以查询结果是null。


# 五、公共字段自动填充：

我们知道，当我们进行插入或者更新操作时，没有设置值的属性，那么在数据表中要么是为null，要么是保留原来的值。有的时候我们我们没有赋值但是却不想让其为空，比如name属性，我们插入时会默认赋上“林志玲”，更新时会默认赋值上“朱茵”，那么就可以用公共字段自动填充。
 **1、使用@TableField注解标记填充字段**



```kotlin
@TableField(fill = FieldFill.INSERT_UPDATE)//插入和更新时填充
 private String name;
```

**2、编写公共字段填充处理器类：**



```java
public class MyMetaObjectHandler extends MetaObjectHandler {
    @Override
    public void insertFill(MetaObject metaObject) {
        Object fieldValue = getFieldValByName("name",metaObject); //获取需要填充的字段
        if(fieldValue == null){   //如果该字段没有设置值
            setFieldValByName("name","林志玲",metaObject); //那就将其设置为"林志玲"
        }
    }

    @Override
    public void updateFill(MetaObject metaObject) {
        Object fieldValue = getFieldValByName("name",metaObject);//获取需要填充的字段
        if(fieldValue == null){ //如果该字段没有设置值
            setFieldValByName("name","朱茵",metaObject);  //那就将其设置为"朱茵"
        }
    }
}
```

**注：**该类继承了MetaObjectHandler类，重写了insertFill和updateFill方法，在这两个方法获取需要填充的字段以及默认填充的值。

**3、在spring-dao.xml中配置：**



```xml
<!-- 公共字段填充处理器 -->
<bean class="com.zhu.mybatisplus.handler.MyMetaObjectHandler" id="myMetaObjectHandler"/>
```



```xml
<!-- 5、mybatisplus的全局策略配置 -->
    <bean id="globalConfiguration" class="com.baomidou.mybatisplus.entity.GlobalConfiguration">
        <!-- 此处省略其他配置 -->
        <!-- 注入公共字段填充处理器 -->
        <property name="metaObjectHandler" ref="myMetaObjectHandler"/>
    </bean>
```

**注：**和配置逻辑删除一样，都是先将自定义的类注册成bean，再在全局策略配置中引用这个bean即可。

**4、测试：**



```java
@Test
public void testHandlerInsert() {
        User user = new User();
        user.setGender(1);
        user.setAge(22);
        user.setLogicFlag(1);
        userDao.insert(user);
}
```



![img](https:////upload-images.jianshu.io/upload_images/11531502-c4379c86e50b7b4e.png?imageMogr2/auto-orient/strip|imageView2/2/w/655/format/webp)

image.png


**注：** 可以看到，虽然我们并没有给name赋值，但是已经自动把“林志玲”传进去了。更新时也一样有效，此处就不将测试代码贴出来了。 





# 六、总结：

mybatis-plus的大部分用法都在《[mybatis-plus的使用 ------ 入门](https://www.jianshu.com/p/ceb1df475021)》和本文中讲解到了，总的来说包括但不限于以下知识点：
 通用crud、全局策略配置、条件构造器、AR模式、插件配置、代码生成器、自定义全局操作、公共字段自动填充等功能。熟练使用这些功能，定能让你写代码时感觉游刃有余！

- 点[我](https://links.jianshu.com/go?to=https%3A%2F%2Fgitee.com%2Frwxing%2Fmp.git)下载项目源码。
- 点[我](https://links.jianshu.com/go?to=https%3A%2F%2Fgitee.com%2Frwxing%2Fmpg.git)下载逆向工程项目源码。



# SpringBoot集成mybatis-plus

### 一、配置

### 1.1、pom.xml

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-web</artifactId>
        </dependency>

        <dependency>
            <groupId>mysql</groupId>
            <artifactId>mysql-connector-java</artifactId>
            <scope>runtime</scope>
        </dependency>
        <dependency>
            <groupId>org.projectlombok</groupId>
            <artifactId>lombok</artifactId>
            <optional>true</optional>
        </dependency>
        <!--mybatis-plus自动的维护了mybatis以及mybatis-spring的依赖，
         在springboot中这三者不能同时的出现，避免版本的冲突，表示：跳进过这个坑-->
        <dependency>
            <groupId>com.baomidou</groupId>
            <artifactId>mybatis-plus-boot-starter</artifactId>
            <version>3.4.0</version>
        </dependency>

        <!-- 引入Druid依赖，阿里巴巴所提供的数据源 -->
        <dependency>
            <groupId>com.alibaba</groupId>
            <artifactId>druid</artifactId>
            <version>1.0.29</version>
        </dependency>
```

### 1.2、application.yml

```yml
# DataSource Config
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url: jdbc:mysql://localhost:3306/test?useSSL=true&useUnicode=true&characterEncoding=UTF-8&serverTimezone=UTC
    username: root
    password: 12138
    type: com.alibaba.druid.pool.DruidDataSource

# mybatis-plus相关配置
mybatis-plus:
  # xml扫描，多个目录用逗号或者分号分隔（告诉 Mapper 所对应的 XML 文件位置）
  #mapper-locations: classpath:mapper/*.xml
  # 以下配置均有默认值,可以不设置
  global-config:
    db-config:
      #主键类型 AUTO:"数据库ID自增" INPUT:"用户输入ID",ID_WORKER:"全局唯一ID (数字类型唯一ID)", UUID:"全局唯一ID UUID";
      id-type: auto
      #字段策略 IGNORED:"忽略判断"  NOT_NULL:"非 NULL 判断")  NOT_EMPTY:"非空判断"
      field-strategy: NOT_EMPTY
      #数据库类型
      db-type: MYSQL
  configuration:
    # 是否开启自动驼峰命名规则映射:从数据库列名到Java属性驼峰命名的类似映射
    map-underscore-to-camel-case: true
    # 如果查询结果中包含空值的列，则 MyBatis 在映射的时候，不会映射这个字段
    call-setters-on-nulls: true
    # 这个配置会将执行的sql打印出来，在开发或测试的时候可以用
    log-impl: org.apache.ibatis.logging.stdout.StdOutImpl
```



## 二、代码层

### 2.1、pojo层

```java
@Data
@TableName("student")
public class Student implements Serializable {
    @TableId(type = IdType.AUTO)
    private Integer id;
    private String name;
    private String sex;
    private String room;
    private int age;
    private String address;
    private String password;

}
```



### 2.2、Dao层

```java
public interface StudentDao extends BaseMapper<Student> {

}
```

### 2.3、Service层

```java
public interface StudentService extends IService<Student> {

}
```

### 2.4、ServiceImpl层

```java
@Service
public class StudentServiceImpl extends ServiceImpl<StudentDao, Student> implements StudentService {

}
```

### 2.5、Controller层

```java
@Controller
@RequestMapping("student")
public class StudentController {
    @Autowired
    private StudentService studentService;

    @CrossOrigin
    @RequestMapping("/getAll")
    @ResponseBody
    public Object getAll() {
        List<Student> list = studentService.list();
        System.out.println("list:" + list);
        return list;
    }
}
```

### 2.6、跨域设置

 1.首先在config目录下的index.js进行跨域设置： 

```java
proxyTable: {
  '/api': {
     target:"http://127.0.0.1:8000/",
     chunkOrigins: true,// 允许跨域
    pathRewrite:{
          '^/api': '' // 路径重写，使用"/api"代替target.
     }
   }
},
```

2.在controller层上加上@CrossOrigin

```java
@CrossOrigin
    @RequestMapping("/listStuPage.do")
    @ResponseBody
    public Object listStuPage(int page,int limit,String name) {
        Map<String,Object> result = new HashMap<>();
        /**包装查询条件*/
        QueryWrapper<Student> query = new QueryWrapper<>();
        if(name!=null){
            query.like(true,"name",name);
        }
        result.put("count",studentService.count());
        result.put("data",studentService.page(new Page<>(page,limit),query).getRecords());
        return result;
    }
```



# 七、vue+element

### 7.1、main.js

```js
// The Vue build version to load with the `import` command
// (runtime-only or standalone) has been set in webpack.base.conf with an alias.
import Vue from 'vue'
import App from './App'
import router from './router'
import ElementUI from 'element-ui';
import 'element-ui/lib/theme-chalk/index.css'; 
/*引入 axios依赖*/
import axios from 'axios'

Vue.use(ElementUI)
Vue.prototype.$axios = axios    //全局注册，使用方法为:this.$axios
Vue.config.productionTip = false

/* eslint-disable no-new */
new Vue({
  el: '#app',
  router,
  components: { App },
  template: '<App/>'
})

```

### 7.2、App.vue

```vue
<template>
  <div id="app">
    <router-view/>
  </div>
</template>

<script>
export default {
  name: 'App'
}
</script>

<style>
</style>

```

### 7.3、index.js

```js
import Vue from 'vue'
import Router from 'vue-router'
import Login from '@/components/login'
import Index from '@/components/views/index'
import StuList from '@/components/views/stulist'
import StuOpt from '@/components/views/stuopt'

Vue.use(Router)

export default new Router({
  routes: [
    {
      path:"/",
      redirect:"/index"
    },
    {
      path: '/login',
      name: 'login',
      component: Login
    },
    {
      path: '/index',
      name: 'index',
      component: Index,
      children:[{
        path: '/stulist',
        name: 'stulist',
        component: StuList
    },{
        path: '/stuopt',
        name: 'stuopt',
        component: StuOpt
    }]
  }
  ]
})

```

### 7.4、Login.vue

```vue
<template>
  <el-dialog title="登录" :show-close="false" :visible.sync="dialogFormVisible" center width="30%">
    <el-form :rules="rules" :model="student"  ref="student">
      <el-form-item label="用户名" :label-width="formLabelWidth" prop="name">
        <el-input v-model="student.name" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="密码" :label-width="formLabelWidth" prop="password">
        <el-input type="password" show-password v-model="student.password" autocomplete="off"></el-input>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button @click="dialogFormVisible = false">取 消</el-button>
      <el-button type="primary" @click="lobinValidate('student')">登 录</el-button>
    </div>
  </el-dialog>
</template>

<script>
export default {
  name: "Login",
  props: ["msg"],
  data() {
    return {
      //表单验证
      rules: {
        name: [
            { required: true, message: '请输入账号', trigger: 'blur' },
            { min: 2, max: 10, message: '长度在 2 到 10 个字符', trigger: 'blur' }
          ],
          password: [
            { required: true, message: '请输入密码', trigger: 'blur' },
            { min: 3, max: 11, message: '长度在 3 到 11 个字符', trigger: 'blur' }
          ]
      },

      student: { name: "", password: "" },
      formLabelWidth: "120px",
      dialogFormVisible: true,
    };
  },
  methods: {
    //登录验证
    lobinValidate(student){
      this.$refs[student].validate((valid) => {
          if (valid) {
            alert(studnet);
            this.login();
          } else {
            this.$message.error("账号或密码格式错误！");
            return false;
          }
        });
    },


    //  登录
    login() {
      const url = "http://localhost:8080/student/loginStu.do";
      this.$axios
        .get(url, {
          params: { name: this.student.name, password: this.student.password },
        })
        .then((res) => {
          console.log(res.data);
          if (res.data == 0) {
            this.$message.error("用户名输入错误！");
          } else if (res.data == -1) {
            this.$message.error("密码输入错误！");
          } else if (res.data == 1) {
              this.$message.success("登录成功！");
              //关闭对话框
              this.dialogFormVisible = false;
            //必须在此存储登录成功的标记
            window.sessionStorage.setItem("stuData", res.data);
            //跳转页面，前端跳转页面时必须使用路由
            this.$router.push("/index"); //跳转页面
          }
        })
        .catch((arror) => {
          this.$message.error("网络异常！");
        });
    },
  },
};
</script>

<style scoped>
</style>
```

### 7.5、index.vue(首页布局)

```vue
<template>
  <el-container>
  <el-header>学生系统</el-header>
  <el-container>
    <el-aside width="200px">
        <el-menu
        router
      default-active="2"
      class="el-menu-vertical-demo"
      background-color="#545c64"
      text-color="#fff"
      active-text-color="#ffd04b">
      <el-submenu index="1">
        <template slot="title">
          <i class="el-icon-location"></i>
          <span>学生管理</span>
        </template>
          <el-menu-item index="/stulist">学生列表</el-menu-item>
          <el-menu-item index="/stuopt">学生管理</el-menu-item>
      </el-submenu>
    </el-menu>
    </el-aside>
    <el-container>
      <el-main>
          <router-view></router-view>
      </el-main>
    </el-container>
  </el-container>
  <el-footer>Footer</el-footer>
</el-container>

</template>

<script>
export default {

}
</script>

<style scoped>
/* 头部样式 */
.el-header {
  background-color: #23262E;
  color: white;
  text-align: center;
  line-height: 60px;
  line-height: 60px;
}
/* 导航样式 */
.el-aside {
  background-color: #282B33;
  height: 570px;
  color: #333;
  text-align: center;
  line-height: 60px;
}

.el-main {
  height: 500px;
  color: #333;
  text-align: center;
  line-height: 60px;
}

.el-footer {
  background-color: #409EFF;
  color: #333;
  text-align: center;
  line-height: 60px;
}

</style>
```

### 7.6、stulist.vue(分页)

```vue
<template>
  <el-card class="box-card">
    <el-breadcrumb separator="/">
  <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
  <el-breadcrumb-item :to="{ path: '/stulist' }">学生列表</el-breadcrumb-item>
  <el-breadcrumb-item :to="{ path: '/stuopt' }">学生管理</el-breadcrumb-item>
</el-breadcrumb>
<br>
    <el-table :data="tableData" border style="width: 100%">
      <el-table-column prop="id" label="编号" width="180"></el-table-column>
      <el-table-column prop="name" label="姓名" width="180"></el-table-column>
      <el-table-column prop="age" label="年龄"></el-table-column>
      <el-table-column prop="sex" label="性别"></el-table-column>
      <el-table-column prop="room" label="班级"></el-table-column>
      <el-table-column prop="address" label="地址"></el-table-column>
      <el-table-column prop="password" label="密码"></el-table-column>
    </el-table>

    <!--分页-->
    <div class="block">
    <el-pagination
      @size-change="handleSizeChange"
      @current-change="handleCurrentChange"
      :current-page="page"
      :page-sizes="[2, 3, 5, 10]"
      :page-size="limit"
      layout="total, sizes, prev, pager, next, jumper"
      :total="total">
    </el-pagination>
  </div>

  </el-card>
</template>

<script>
export default {
  data() {
    return {
      tableData: [],
      limit:3,
      page:1,
      total:0
    };
  },
  //页面加载后调用
  mounted() {
    this.search();
  },
  methods: {
    //分页
    //每页条数
    handleSizeChange(val) {
        this.limit=val;
        this.search();
      },
    //当前页
    handleCurrentChange(val) {
      console.log('currentPage', val)
        this.page=val;
        this.search();
    },


    async search() {
      const url = "http://localhost:8080/student/listStuPage.do";
      const { data: res } = await this.$axios.get(url,{
        params:{
          page:this.page,
          limit:this.limit
        }
      });
      console.log('res', res);
      this.total=res.count;
      this.tableData=res.data;
      this.page=res.page;

    },
  },
};
</script>
<style scoped>
</style>
```

### 7.7、stuopt.vue(增删改查)

```vue
<template>
  <el-card class="box-card">
    <el-breadcrumb separator="/">
  <el-breadcrumb-item :to="{ path: '/' }">首页</el-breadcrumb-item>
  <el-breadcrumb-item :to="{ path: '/stulist' }">学生列表</el-breadcrumb-item>
  <el-breadcrumb-item :to="{ path: '/stuopt' }">学生管理</el-breadcrumb-item>
</el-breadcrumb>

<el-row :gutter="10">
  <el-col :span="5">
    <el-input clearable prefix-icon="el-icon-search" v-model="id" placeholder="请输入学生编号"></el-input>
  </el-col>
  <el-col :span="3">
    <el-button type="primary" v-on:click="search2()"  >查询</el-button>
  </el-col>
  <el-col :span="3">
    <el-button type="primary" @click="showSaveDialog"><i class="el-icon-plus"></i>添加</el-button>
  </el-col>
</el-row>

<!--添加对话框-->
<el-dialog title="添加"  :visible.sync="saveDialog" center width="30%">
    <el-form>
      <el-form-item label="用户名" :label-width="formLabelWidth">
        <el-input v-model="student.name" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="性别" :label-width="formLabelWidth">
        <el-input v-model="student.sex" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="班级" :label-width="formLabelWidth">
        <el-input v-model="student.room" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="年龄" :label-width="formLabelWidth">
        <el-input v-model="student.age" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="家庭住址" :label-width="formLabelWidth">
        <el-input v-model="student.address" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="密码" :label-width="formLabelWidth">
        <el-input type="password" show-password v-model="student.password" autocomplete="off"></el-input>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button @click="saveDialog = false">取 消</el-button>
      <el-button type="primary" @click="saveStu">提 交</el-button>
    </div>
  </el-dialog>


<!--编辑对话框-->
<el-dialog title="修改" :visible.sync="editDialog" center width="30%">
    <el-form>
      <el-form-item label="用户名" :label-width="formLabelWidth">
        <el-input v-model="student.name" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="性别" :label-width="formLabelWidth">
        <el-input v-model="student.sex" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="班级" :label-width="formLabelWidth">
        <el-input v-model="student.room" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="年龄" :label-width="formLabelWidth">
        <el-input v-model="student.age" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="家庭住址" :label-width="formLabelWidth">
        <el-input v-model="student.address" autocomplete="off"></el-input>
      </el-form-item>
      <el-form-item label="密码" :label-width="formLabelWidth">
        <el-input type="password" show-password v-model="student.password" autocomplete="off"></el-input>
      </el-form-item>
    </el-form>
    <div slot="footer" class="dialog-footer">
      <el-button @click="editDialog = false">取 消</el-button>
      <el-button type="primary" @click="editStu">提 交</el-button>
    </div>
  </el-dialog>





    <!--表格-->
    <el-table :data="tableData" border style="width:900px">
      <el-table-column prop="id" label="编号" width="50"></el-table-column>
      <el-table-column prop="name" label="姓名" width="100"></el-table-column>
      <el-table-column prop="age" label="年龄" width="70"></el-table-column>
      <el-table-column prop="sex" label="性别" width="70"></el-table-column>
      <el-table-column prop="room" label="班级" width="100"></el-table-column>
      <el-table-column prop="address" label="地址"></el-table-column>
      <el-table-column prop="password" label="密码"></el-table-column>
      <!--操作-->
      <el-table-column label="操作">
      <template slot-scope="scope">
        <el-button
          size="mini"
          @click="showEditDialog(scope.row)">编辑</el-button>
        <el-button
          size="mini"
          type="danger"
          @click="removeStu(scope.row)">删除</el-button>
      </template>
    </el-table-column>
    </el-table>
  </el-card>
</template>

<script>
export default {
  data() {
    return {
      student: { name: "", sex: "", room: "", age: "", address: "", password: "" },
      tableData: [],
      id: '',
      formLabelWidth: "120px",
      saveDialog:false,
      editDialog:false
    };
  },
  //页面加载后调用
  mounted() {
    this.search();
  },
  methods: {
    async search() {
      const url = "http://localhost:8080/student/findStuAll.do";
      const { data: res } = await this.$axios.get(url);
      this.tableData = res;
    },
    async search2() {
      const url = "http://localhost:8080/student/findStuById.do";
      const { data: res } = await this.$axios.get(url,{
        params:{
          id:this.id
        }
      });
      this.tableData = res;
    },


    //显示添加对话框
    showSaveDialog(){
      this.saveDialog=true;
    },
    // 添加
    async saveStu() {
      this.saveDialog=false;
      const url = "http://localhost:8080/student/saveStu.do";
      const { data: res } = await this.$axios.get(url,{
        params:{
          name:this.student.name,
          sex:this.student.sex,
          room:this.student.room,
          age:this.student.age,
          address:this.student.address,
          password:this.student.password,
        }
      });
      if(res){
        this.$message.success("添加成功！");
        this.search();
        this.clearStu();
        this.saveDialog=false;
      }else{
        this.$message.error("添加失败！");
      }
    },
    //清空内容
    clearStu(){
      this.student={
        name: "", sex: "", room: "", age: "", address: "", password: "" 
      }
    },
    //删除学生
    async removeStu(row){
      const url = "http://localhost:8080/student/deleteStu.do";
      const { data: res } = await this.$axios.get(url,{
        params:{
          id: row.id
        }
      });
      if(res){
        this.$message.success("删除成功！");
        this.search();
      }else{
        this.$message.error("删除失败！");
      }
    },

    //显示编辑对话框
    showEditDialog(row){
      this.editDialog=true;
      this.id = row.id;
      this.student.name = row.name;
      this.student.sex = row.sex;
      this.student.room = row.room;
      this.student.age = row.age;
      this.student.address = row.address;
      this.student.password = row.password;
    },
    //修改学生
    async editStu(){
      const url = "http://localhost:8080/student/updateStu.do";
      const { data: res } = await this.$axios.get(url,{
        params:{
          id:this.id,
          name:this.student.name,
          sex:this.student.sex,
          room:this.student.room,
          age:this.student.age,
          address:this.student.address,
          password:this.student.password,
        }
      });
      if(res){
        this.$message.success("修改成功！");
        this.search();
        this.clearStu();
        this.id='';
        this.editDialog=false;
      }else{
        this.$message.error("修改失败！");
      }
    }


  },
};
</script>
<style scoped>
</style>
```

### 7.8、StudentController(mybatis-plus后台)

#### 7.8.1、Controller层

```java
@Controller
@RequestMapping("student")
public class StudentController {
    @Autowired
    private StudentService studentService;

    /**学生登录*/
    @CrossOrigin
    @RequestMapping(value="/loginStu.do",produces ="application/json;charset=utf-8")
    @ResponseBody
    public Integer loginStu(@RequestParam("name") String name,@RequestParam("password") String password) {
        QueryWrapper<Student> wrapper=new QueryWrapper<>();
        wrapper.eq("name",name);
        Student stu1 = studentService.getOne(wrapper);
        if(stu1!=null){
            wrapper.eq("password",password);
            Student stu2 = studentService.getOne(wrapper);
            if(stu2!=null){
                return 1;
            }else{
                return -1;
            }
        }else{
            return 0;
        }
    }

    /**查询所有学生*/
    @CrossOrigin
    @RequestMapping("/findStuAll.do")
    @ResponseBody
    public Object findStuAll() {
        List<Student> list = studentService.list();
        return list;
    }

    /**分页查询所有学生*/
    @CrossOrigin
    @RequestMapping("/listStuPage.do")
    @ResponseBody
    public Object listStuPage(int page,int limit,String name) {
        System.out.println("page:"+page);
        Map<String,Object> result = new HashMap<>();
        /**包装查询条件*/
        QueryWrapper<Student> query = new QueryWrapper<>();
        if(name!=null){
            query.like(true,"name",name);
        }
        result.put("count",studentService.count());
        result.put("data",studentService.page(new Page<>(page,limit),query).getRecords());
        result.put("page",page);
        return result;
    }

    /**根据条件查询学生所有学生*/
    @CrossOrigin
    @RequestMapping("/findStuById.do")
    @ResponseBody
    public Object findStuById(@RequestParam("id") Integer id) {
        Map<String,Object> map = new HashMap<>();
        map.put("id",id);
        List<Student> list = studentService.listByMap(map);
        return list;
    }


    /**添加学生*/
    @CrossOrigin
    @RequestMapping("/saveStu.do")
    @ResponseBody
    public boolean saveStu(Student student){
        studentService.save(student);
        return true;
    }

    /**根据编号删除学生*/
    @CrossOrigin
    @RequestMapping("/deleteStu.do")
    @ResponseBody
    public boolean deleteStu(@RequestParam("id") Integer id){
        studentService.removeById(id);
        return true;
    }

    /**修改学生*/
    @CrossOrigin
    @RequestMapping("/updateStu.do")
    @ResponseBody
    public boolean updateStu(Student student){
        studentService.updateById(student);
        return true;
    }

}
```

7.8.2、分页的使用

新建 **config** 包，再在config包中新建 **MybatisPlusConfig** 类

```java
@Configuration
@ConditionalOnClass(value = {PaginationInterceptor.class})
public class MybatisPlusConfig {
    @Bean
    public PaginationInterceptor paginationInterceptor() {
        PaginationInterceptor paginationInterceptor = new PaginationInterceptor();
        return paginationInterceptor;
    }
}
```

