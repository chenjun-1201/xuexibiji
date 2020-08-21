# mybatis

1：创建步骤

- 引入mybatis的jar包和相关依赖(mysql,log4j,junit)
- 新建一个mybatis.xml，配置相关配置

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">

     
 <configuration>
 <environments default="mysql">
 <environment id="mysql">
 
 <transactionManager type="jdbc"></transactionManager>
 <dataSource type="POOLED">
<property name="url" value="jdbc:mysql://localhost:3306/mysql"/>
<property name="driver" value="com.mysql.jdbc.Driver"/>
<property name="username" value="root"/>
<property name="password" value="123456"/>
 </dataSource>
 
 </environment>
 
 </environments>
 
 
 
 <mappers>
 <!-- 使用注解时这里配置为class，使用xml配置时，用 resource-->
<!--  <mapper class="mybatis01.IAccountDao"/> -->

   <!-- 配置xml的映射-->
<mapper resource="IAccountDao.xml"/>
 </mappers>
 </configuration>
```

- 创建mapper.xml ,namespace 指向映射的接口

```xml
<?xml version="1.0" encoding="UTF-8"?>
<!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">

<!--namespace 指向映射的接口-->
<mapper namespace="mybatis01.IAccountDao">
<select id="findall" resultType="mybatis01.Account">
select * from account
</select>

</mapper>
```

```java
package mybatis01;

import java.util.ArrayList;
import java.util.List;

import org.apache.ibatis.annotations.Select;

public interface IAccountDao {

	
	//@Select("select * from account") //使用注解方式
	public ArrayList<Account> findall();
	

}

```



## 使用实现类的方式

````java
package mybatis01;

import java.util.ArrayList;
import java.util.List;

import org.apache.ibatis.session.SqlSession;

public class AccountDaoImpl implements IAccountDao{
	
	private SqlSession session;
	
	public AccountDaoImpl(SqlSession s) {
		// TODO Auto-generated constructor stub
		this.session=s;
	}

	public List<Account> findall() {
		// TODO Auto-generated method stub
    //selectList的参数为statement，指定mapper.xml的namespace+findall
	List<Account>list=	session.selectList("mybatis01.IAccountDao.findall");
		
		
		return list;
	}
	

}

````



mybatis原理和流程

- 使用dom4j解析mybatis.xml中的内容，得到了数据库连接的信息，使用inputstream接收该信息
- 使用工厂模式创建sqlsessionfactory，把inputstream穿进去
- 使用sqlsessionfactory创建sqlsession ，
- sqlsession.getmapper(接口.class);使用jdk动态代理，实现invokerhander,将调用sqlsession中的方法,selectlist,selectone....等等,得到一个代理对象
- 调用代理对象的方法
- 释放资源



和jdbc很像，

jdbc第一步获取数据库连接《==》使用dom4j解析mybatis.xml中的内容，得到了数据库连接的信息

jdbc第二步preparestatement,执行sql语句 <=>读取某个mapper.xml中的

````xml
<mapper namespace="mybatis01.IAccountDao">
<select id="findall" resultType="mybatis01.Account">
select * from account
</select>

````

通过得到对应的mapper,每一个增删盖茶的片段都封装成了一个map

key是namespace+id，value是sql语句和resluttype,得到sql后就可以使用preparestatement执行了





## paramtype

### ongl表达式

 ongl 写法 ： 对象.username

  一般写法   对象.getusername  ,

相比于传统写法沈略了get，

在mybatis中一般不要加对象名，因为parametertype中有对象名



使用对象的某个属性作为查询条件

vo.name



resultmap 用于字段名和表名不一致的情况

````xml
<resultMap type="mybatis01.Account" id="map1">
<id column="id" property="ddid"/>
<result column="name"  property="name"/>
<result column="money"  property="money"/>
</resultMap>
<select id="findall" resultMap="map1">
select * from account
</select>

````

resulttype： 类的全雷鸣

resultmap： 在xml中定义的一个map，主要用于列明和表的列民不一致的情况

也可以用于多表操作





动态sql



## selectkey 增加细节

原来的方式新增一个列，获取不到增长的id，因为id是自动增长的，使用selectkey 可以解决这个问题

```xml

<insert id="save" parameterType="mybatis01.Account">
<selectKey keyProperty="id" keyColumn="id" order="AFTER" resultType="java.lang.Integer">
select last_insert_id();
</selectKey>
insert into account(name,money)  values(#{name},#{money})
</insert>
```

```java
	//使用代理对象执行方法
	
	Account account=new Account();
	;
	account.setMoney(Long.parseLong("8888"));
	
	account.setName("dalaoban");
	System.out.println("执行之前");
	System.out.println(account);
	accountdao.save(account);
	
	sqlsession.commit();
	System.out.println("执行之后");
//会发现account的id变成了最后新增的那个id了
	System.out.println(account);
		//释放资源
	
```





//读取properties文件

```java
 <properties resource="jdbc.properties">
 <!-- 引入外部配置文件 -->
 </properties>
 
   
   <property name="url" value="${url}"/>
<property name="driver" value="${driver}"/>
<property name="username" value="${username}"/>
<property name="password" value="${password}"/>
   
```

如果properties的属性有前缀，就要在${}加前缀



Resource :值为路径名



## 给domain起别名

```xml
!-- 用于指定domain的别名 -->
 <typeAliases>
 <typeAlias type="mybatis01.Account" alias="account"/>
   

 </typeAliases>
 
```

就可以在mapper.xml中写别名

```xml

<insert id="save" parameterType="account">
```



```xml
    <!-- 用于给包起别名，该包下面的所有类不区分大小写，并且类名就是别名 -->

 <typeAliases>
 <package name="mybatis01"/>
 </typeAliases>

```

这个包下所有的domain就 可以不区分大小写，用类名作为别名了。

但这能是domain





## mappers中的package

```xml
 <mappers>
<!-- 以后也不要写mapper ，resource， class，package可以指定对应的xml，他会扫描这个包下的所有配置-->
<package name="mybatis01"/>
 </mappers>
```





## mybatis的连接池

- 分类
- POOLED：连接池
- UNPOOLED：非连接池
- JNDI:Tomcat提供

````xml
 <dataSource type="POOLED">
   <!-- 有三个属性
POOLED：连接池
UNPOOLED：非连接池
JNDI:Tomcat提供
-->
</dataSource>
````

POOLED 用的是pooleddatasource 类



源码分析:pooleddatasource 类

````java

    while (conn == null) {//如果conn==null
      synchronized (state) {
        if (!state.idleConnections.isEmpty()) {//如果空闲池不为空
          // Pool has available connection
          conn = state.idleConnections.remove(0);//从空闲池中获取一个连接
          if (log.isDebugEnabled()) {
            log.debug("Checked out connection " + conn.getRealHashCode() + " from pool.");
          }
        } else {
          // Pool does not have available connection
          if (state.activeConnections.size() < poolMaximumActiveConnections) {//如果活动的连接小于池的最大的活动数
            // Can create new connection
            conn = new PooledConnection(dataSource.getConnection(), this);//从活动池中获取一个连接
            if (log.isDebugEnabled()) {
              log.debug("Created connection " + conn.getRealHashCode() + ".");
            }
          } else {//都不行就从活动池的最老的连接作为新的连接
            // Cannot create new connection
            PooledConnection oldestActiveConnection = state.activeConnections.get(0);
            long longestCheckoutTime = oldestActiveConnection.getCheckoutTime();
            if (longestCheckoutTime > poolMaximumCheckoutTime) {
              // Can claim overdue connection
              state.claimedOverdueConnectionCount++;
              state.accumulatedCheckoutTimeOfOverdueConnections += longestCheckoutTime;
              state.accumulatedCheckoutTime += longestCheckoutTime;
              state.activeConnections.remove(oldestActiveConnection);
              if (!oldestActiveConnection.getRealConnection().getAutoCommit()) {
                try {
                  oldestActiveConnection.getRealConnection().rollback();
                } catch (SQLException e) {
                  log.debug("Bad connection. Could not roll back");
                }  
              }
````

对比：pooled和unpooled

pooled类型连接池：当sqlsession.close() 时log打印的内容

````
DEBUG [main] - Closing JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@5d20e46]
DEBUG [main] - Returned connection 97652294 to pool.
````



unpooled

````
DEBUG [main] - Closing JDBC Connection [com.mysql.cj.jdbc.ConnectionImpl@58fdd99]
````



//可以看到pooled和unpooled的区别就是，pooled将用完的连接放回池里,

而unpooled没有放回。



UNPOOLED用的是UNPOOLEDDataSource类：底层就是原来的jdbc，当连接用完后就关闭连接



## 多表操作

- 一对一  

- 往entity中加一个user对象属性

  - ```xml
    <resultMap type="account" id="map">
            <result property="id" column="id"/>
            <result property="uid" column="uid"/>
            <result property="money" column="money"/>  
    <!-- column表示通过这个表的uid去查-->    
    <association property="user" javaType="user" column="uid">
      
      <id column="id"  property="userId"/>
            <result property="userName" column="userName"/>
      
            </association>
       </resultMap>
            
    ```

- 一对多

- 先往entity中加一个list<account>对象属性

  - ```xml
     <resultMap type="user" id="map">
            <id column="id" property="userId"/>
            <result column="userName" property="userName"/>
               <result column="userAddress" property="userAddress"/>
                  <result column="userSex" property="userSex"/>
                     <result column="userBirthday" property="userBirthday"/>
       <!-- column表示通过这个表的uid去查-->
       <!-- oftype表示对应的哪个其他表-->
                      <collection property="list" column="id" ofType="account">
                      <id property="id" column="aid"/>
           				 <result column="uid" property="uid"/>
                       <result column="money" property="money"/>
                      </collection>
            
            </resultMap>
    ```

  - 多对多，新建一个中间表，拆分成两个一对多

- 总结：一对一用association  

- 一对多用collection

- 注意标签里面的属性（重点）





## 延时加载和及时加载

- 一对一 ：用及时加载
- 一对多：用延时加载





````xml
<!--配置-->
  <settings>
 <setting name="lazyLoadingEnabled" value="true"/>
 <setting name="aggressiveLazyLoading" value="true"/>
 </settings>
````





|                       | true \| false                                                | true          |       |
| --------------------- | ------------------------------------------------------------ | ------------- | ----- |
| lazyLoadingEnabled    | 延迟加载的全局开关。当开启时，所有关联对象都会延迟加载。 特定关联关系中可通过设置 `fetchType` 属性来覆盖该项的开关状态。 | true \| false | false |
| aggressiveLazyLoading | 开启时，任一方法的调用都会加载该对象的所有延迟加载属性。 否则，每个延迟加载属性会按需加载（参考 `lazyLoadTriggerMethods`)。 | true \| false |       |



select指向user的findbyid方法，

````xml
  <association property="user" javaType="user" column="uid" select="com.baidu.dao.IUserDao.findbyid">
        </association>
````

一对多也是用select关键字

```xml
<collection property="list" column="id" ofType="account" select="com.baidu.dao.IAccountDao.findbyid">
                  <id property="id" column="aid"/>
       				 <result column="uid" property="uid"/>
                   <result column="money" property="money"/>
                  </collection>
        
```



tips ：一对一可以不用上面的策略，用resultmap封装结果，直接查询，



## mybatis中的缓存

Mybatis中的缓存
	什么是缓存
		存在于内存中的临时数据。
	为什么使用缓存
		减少和数据库的交互次数，提高执行效率。
	什么样的数据能使用缓存，什么样的数据不能使用
		适用于缓存：
			经常查询并且不经常改变的。
			数据的正确与否对最终结果影响不大的。
		不适用于缓存：
			经常改变的数据
			数据的正确与否对最终结果影响很大的。
			例如：商品的库存，银行的汇率，股市的牌价。



### 一级缓存：

sqlsession的一级缓存，当调用了sqlsession的commit,update,clearcache()(增删改)操作会将原来的一级缓存清空

只要sqlsession不进行增删改同一个sqlsession中查询的内容，在sqlsession中是共享的，当在调用查询的方法时，就会从缓存中拿数据。



```java
	User user = id.findbyid(41);
	//sqlssession.close();//关闭sqlsession会清空一级缓存
//sqlsession=sqlssessionfactory.opensession();
	User user2 = id.findbyid(41);
	
	System.out.println(user==user2);
//可以看到log只打印了一次sql查询， user和user2指向同一个对象
//当sqlsession的一级缓存被清空了，
```



### 二级缓存

- 二级缓存不是默认打开的需要配置

  - 1.主配置文件将cacheEnabled配置true

    - ```xml
        <settings>
       <setting name="lazyLoadingEnabled" value="true"/>
       <setting name="aggressiveLazyLoading" value="true"/>
       <setting name="cacheEnabled" value="true"/>
       <setting name="cacheEnabled" value="true"/>
       </settings>
      ```

    - 

  - 2.dao配置文件配置cache

    - ```xml
        <cache/>
      ```

    - 

  - 3.usecache="true"

    - ````xml
         <select id="findall" resultMap="map"  useCache="true">
              
              select * from account 
              </select>
      ````

    - 

  - ```java
    	InputStream ii= Resources.getResourceAsStream("mybatis.xml");
    		SqlSessionFactoryBuilder buider=new SqlSessionFactoryBuilder();
    		SqlSessionFactory factory= buider.build(ii);
    	SqlSession session=	factory.openSession();
    	SqlSession session2=	factory.openSession();
    IAccountDao id=	session.getMapper(IAccountDao.class);
    
    IAccountDao id2=session2.getMapper(IAccountDao.class);
    Account account = id.findaccountbyid(1);
    
    session.close();
    
    Account account2 = id2.findaccountbyid(1);
     //返回false，存的不是一个对象，而是数据
    System.out.println(account==account2);
    
    session2.close();
    
    ```

  - 4.实体类中要实现序列化接口

==二级缓存是在sqlsessionfactory中在每个sqlsession中是共享的，当sqlsession第一次查一个数据时，会以数据的形式存在于sqlsessionfactory中，而不是以对象的形式存在于其中，不同的sqlsession==

可以对其访问，得到的结果不是同一个对象



## 注解开发

@Insert:实现新增

 @Update:实现更新 

@Delete:实现删除

 @Select:实现查询 

@Result:实现结果集封装

 @Results:可以与@Result 一起使用，封装多个结果集

 @ResultMap:实现引用@Results 定义的封装 

@One:实现一对一结果集封装

 @Many:实现一对多结果集封装 

@SelectProvider: 实现动态SQL映射 

@CacheNamespace:实现注解二级缓存的使用



代码：（多对多）

```java
@Select("select * from user")@Results(value = {        @Result(id = true,column = "id",property = "id"),        @Result(column = "username",property = "username"),        @Result(column = "address",property = "address"),        @Result(column = "sex",property = "sex"),        @Result(column = "birthday",property = "birthday"),        //下面的column表示根据id去查        @Result(property = "roles",column = "id",                many=@Many(select = "com.itheima.dao.IRoleDao.findrolebyid",                        fetchType= FetchType.LAZY))})
```





注意：

同一个dao下注解不能和xml同时使用

用起来很简单，推荐使用。

一对一一般是及时加载

一对多是延时加载





@Results相当于xml中的resultmap，当pojo的字段和mybatis中的字段不一致时，可以用，





技巧：

当不知道该注解有什么属性，可以看源码，就知道很多信息