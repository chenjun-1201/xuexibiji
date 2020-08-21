[TOP]





## 1介绍（理论部分）

Spring从3.1开始定义了org.springframework.cache.Cache

|               注解                | 作用                         |
| :-------------------------------: | :--------------------------- |
|            @Cacheable             | 用于添加一个缓存一般用于查询 |
|             @CachePut             | 用于更新                     |
| @CacheConfig(cacheNames ={"dep"}) | 配置全局缓存名               |
|           @CacheEvict：           | 缓存清除                     |
|                                   |                              |

步骤1:在springboot主配置文件上配置@EnableCaching

```java

@CacheConfig(cacheNames ={"dep"})
@Service
public class DepartmentService {
    @Autowired
    private IDepartmentDao  iDepartmentDao;

    public  void insert(Department department){

        iDepartmentDao.insert(department);
    }
  /*
  2、去Cache中查找缓存的内容，使用一个key，默认就是方法的参数；
     *      key是按照某种策略生成的；默认是使用keyGenerator生成的，默认使用SimpleKeyGenerator生成key；
     *          SimpleKeyGenerator生成key的默认策略；
     *                  如果没有参数；key=new SimpleKey()；
     *                  如果有一个参数：key=参数的值
     *                  如果有多个参数：key=new SimpleKey(params)；
     
     
     *   3、没有查到缓存就调用目标方法；
     *   4、将目标方法返回的结果，放进缓存中
     核心：
     *      1）、使用CacheManager【ConcurrentMapCacheManager】按照名字得到Cache【ConcurrentMapCache】组件
     *      2）、key使用keyGenerator生成的，默认是SimpleKeyGenerator
     *
     
     几个属性：
     *      cacheNames/value：指定缓存组件的名字;将方法的返回结果放在哪个缓存中，是数组的方式，可以指定多个缓存；
     *
     *      key：缓存数据使用的key；可以用它来指定。默认是使用方法参数的值  1-方法的返回值
     *              编写SpEL； #i d;参数id的值   #a0  #p0  #root.args[0]
     *              getEmp[2]
     *
     *      keyGenerator：key的生成器；可以自己指定key的生成器的组件id
     *              key/keyGenerator：二选一使用;
     *
     *
     *      cacheManager：指定缓存管理器；或者cacheResolver指定获取解析器
     *
     *      condition：指定符合条件的情况下才缓存；
     *              ,condition = "#id>0"
     *          condition = "#a0>1"：第一个参数的值》1的时候才进行缓存
     *
     *      unless:否定缓存；当unless指定的条件为true，方法的返回值就不会被缓存；可以获取到结果进行判断
     *              unless = "#result == null"
     *              unless = "#a0==2":如果第一个参数的值是2，结果不缓存；
     *      sync：是否使用异步模式
     */
  
  
  
  
  
    //Cacheable默认是参数id的值作为key
  //默认的作用时机：调用方法前（先从缓存中查，如果没有就添加）
    @Cacheable(/*value = {"dep"}*/)
    public Department findbyid(int id){
        Department department = iDepartmentDao.findbyid(id);
        return department;
    }
    //CachePut方法执行时机：先执行内部的方法再执行更新缓存
  
    //如果不指定key，CachePut默认以department作为key，跟上面的id不同，所以不能更新缓存
  
   /**
     * @CachePut：既调用方法，又更新缓存数据；同步更新缓存
     * 修改了数据库的某个数据，同时更新缓存；
     * 运行时机：
     *  1、先调用目标方法
     *  2、将目标方法的结果缓存起来
     *
     * 测试步骤：
     *  1、查询1号员工；查到的结果会放在缓存中；
     *          key：1  value：departmentName：张三
     *  2、以后查询还是之前的结果
     *  3、更新1号员工；【lastName:zhangsan；gender:0】
     *          将方法的返回值也放进缓存了；
     *          key：传入的deparement对象  值：返回的deparement对象；
     *  4、查询1号员工？
     *      应该是更新后的员工；
     *          key = "#deparement.id":使用传入的参数的员工id；
     *          key = "#result.id"：使用返回后的id
     *             @Cacheable的key是不能用#result
     *      为什么是没更新前的？【1号员工没有在缓存中更新】
     *
     */
    //解决方法：,key = "#department.id"
    @CachePut(value ={"dep"},key = "#department.id")
    public  Department update(Department department){
        iDepartmentDao.updatedepartment(department);
        return department;

    }

    /**
     * @CacheEvict：缓存清除
     *  key：指定要清除的数据
     *  allEntries = true：指定清除这个缓存中所有的数据
     *  beforeInvocation = false：缓存的清除是否在方法之前执行
     *      默认代表缓存清除操作是在方法执行之后执行;如果出现异常缓存就不会清除
     *
     *  beforeInvocation = true：
     *      代表清除缓存操作是在方法运行之前执行，无论方法是否出现异常，缓存都清除
     *
     *
     */
    @CacheEvict(value = "{dep}")
    public void delete(int id){
       // iDepartmentDao.delete(id);

    }

    @Caching(
            cacheable = {@Cacheable(/*value = {"dep"},*/key = "#departmentname")}
            ,put = {@CachePut(value = {"dep"},key = "#result.id")
                    ,@CachePut(value = {"dep"},key = "#result.departmentName")
    }

    )
    public  Department fidbyname(String departmentname){
        Department department = iDepartmentDao.finduserbyname(departmentname);
        System.out.println(department.getDepartmentName());
        return department;
    }


}

```

原理：

- ![Alt text](/Users/mac/Desktop/mytextworkspace/屏幕快照 2020-07-17 21.06.42.png)

```java
  /*   运行流程：
     
    * 将方法的运行结果进行缓存；以后再要相同的数据，直接从缓存中获取，不用调用方法；
     * CacheManager管理多个Cache组件的，对缓存的真正CRUD操作在Cache组件中，每一个缓存组件有自己唯一一个名字；
     *

     *
    
     *
     */
```

 * 原理：
     *   1、自动配置类；CacheAutoConfiguration
     *   2、缓存的配置类
     *   org.springframework.boot.autoconfigure.cache.GenericCacheConfiguration
     *   org.springframework.boot.autoconfigure.cache.JCacheCacheConfiguration
     *   org.springframework.boot.autoconfigure.cache.EhCacheCacheConfiguration
     *   org.springframework.boot.autoconfigure.cache.HazelcastCacheConfiguration
     *   org.springframework.boot.autoconfigure.cache.InfinispanCacheConfiguration
     *   org.springframework.boot.autoconfigure.cache.CouchbaseCacheConfiguration
     *   org.springframework.boot.autoconfigure.cache.RedisCacheConfiguration
     *   org.springframework.boot.autoconfigure.cache.CaffeineCacheConfiguration
     *   org.springframework.boot.autoconfigure.cache.GuavaCacheConfiguration
     *   org.springframework.boot.autoconfigure.cache.SimpleCacheConfiguration【默认】
     *   org.springframework.boot.autoconfigure.cache.NoOpCacheConfiguration
     *   3、哪个配置类默认生效：<u>**SimpleCacheConfiguration**</u>；
     *
     *   4、给容器中注册了一个CacheManager：ConcurrentMapCacheManager
     *   5、可以获取和创建ConcurrentMapCache类型的缓存组件；他的作用将数据保存在ConcurrentMap中；

## 2.使用redis缓存

## 使用步骤：

### 1，配application.properties,在主程序加@EnableCaching

```yml
  redis:
    host: 192.168.0.9
```

### 2引入依赖：

```xml
   <dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-redis</artifactId>
            <version>1.4.7.RELEASE</version>
        </dependency>
```

### 3测试redistemplate

```java

@SpringBootTest
class DemoiiApplicationTests {

    @Qualifier("redisTemplate")
    @Autowired
private RedisTemplate redisTemplate2;

    @Autowired
    private RedisTemplate<Object ,Department> depredisTemplate;

    @Autowired
    private StringRedisTemplate redisTemplate;

@Autowired
ApplicationContext ca;

    @Autowired
    SqlSessionFactory sqlSessionFactory;

    @Autowired
    private IDepartmentDao iDepartmentDao;
    @Test
    void contextLoads2() throws SQLException {
//        ApplicationContext applicationContext=new ClassPathXmlApplicationContext();
        SqlSession sqlSession = sqlSessionFactory.openSession();
        IDepartmentDao dao = sqlSession.getMapper(IDepartmentDao.class);
        System.out.println(dao);
        List<Department> l = dao.findAll();
        for (Department i:l
             ) {
            System.out.println(i.getDepartmentName());
        }
    }

    @Test
    void contextLoads3() throws SQLException {
//        ApplicationContext applicationContext=new ClassPathXmlApplicationContext();
        SqlSession sqlSession = sqlSessionFactory.openSession();
        IDepartmentDao dao = sqlSession.getMapper(IDepartmentDao.class);
//        System.out.println(dao);
        Department department = new Department();
        department.setId(3);
        department.setDepartmentName("部门3");
        dao.updatedepartment(department);
    }

    @Test//测试redistemplate
                public void hhh(){



        /**
         * Redis常见的五大数据类型
         *  String（字符串）、List（列表）、Set（集合）、Hash（散列）、ZSet（有序集合）
         *  stringRedisTemplate.opsForValue()[String（字符串）]
         *  stringRedisTemplate.opsForList()[List（列表）]
         *  stringRedisTemplate.opsForSet()[Set（集合）]
         *  stringRedisTemplate.opsForHash()[Hash（散列）]
         *  stringRedisTemplate.opsForZSet()[ZSet（有序集合）]
         */
//redisTemplate.opsForValue().append("mmsg","helloworkd");
//        	String msg = redisTemplate.opsForValue().get("mmsg");
//	System.out.println(msg);
           redisTemplate.opsForList().leftPush("mylist","jhjj");
//        redisTemplate.opsForList().leftPush("mylist","1");
//        redisTemplate.opsForList().leftPush("mylist","2");


    }

    @Test
    public void tt(){
        Department department = iDepartmentDao.findbyid(1);

        //默认如果保存对象，使用jdk序列化机制，序列化后的数据保存到redis中
//        redisTemplate2.opsForValue().set("h",department);
        //1、将数据以json的方式保存
        //(1)自己将对象转为json
        //(2)redisTemplate默认的序列化规则；改变默认的序列化规则；
        depredisTemplate.opsForValue().set("ppp",department);

    }




}

```

配置使用自己客户端：

```java

@Configuration
class RedisMyRedisConfig  {
//    @Bean
//    publi


    //配置自己的Redistemplate，修改默认的序列化器，使其序列化成json格式。
    @Bean
    public RedisTemplate<Object, Department> redisTemplate(RedisConnectionFactory redisConnectionFactory) throws UnknownHostException {
        RedisTemplate<Object,Department > template = new RedisTemplate();
        template.setConnectionFactory(redisConnectionFactory);
        template.setDefaultSerializer(new Jackson2JsonRedisSerializer<Department>(Department.class));
        return template;
    }


    //自定义cachemanager，使用json格式的序列化器。
    @Bean
    public CacheManager cacheManager(RedisConnectionFactory factory){
        RedisCacheConfiguration cacheConfiguration = RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ofDays(1))
                .disableCachingNullValues()
                .serializeValuesWith(RedisSerializationContext.SerializationPair.fromSerializer(new GenericJackson2JsonRedisSerializer()));
        return RedisCacheManager.builder(factory).cacheDefaults(cacheConfiguration).build();}
}

```



### 原理介绍：

引入Redis的相关依赖，配置相关配置，在application.yml配置debug=true，查看

CacheConfiguration发现只有RedisCacheConfiguration匹配。该类详细请尚未搜索。

*RedisCacheConfiguration*是核心配置类。

