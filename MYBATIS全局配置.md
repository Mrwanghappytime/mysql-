# MYBATIS

#### 1.mybatis pom. xml

```xml
<dependency>
       <groupId>org.mybatis</groupId>
       <artifactId>mybatis</artifactId>
       <version>3.4.4</version>
 </dependency>
 <dependency>
     <groupId>mysql</groupId>
      <artifactId>mysql-connector-java</artifactId>
         <version>8.0.12</version>
</dependency>
```

### 2.mybatis 全局配置文

```xml
#配置文件开头
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE configuration
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-config.dtd">
<configuration>
	#properties下属两种配置方式，配置文件中的配置优先级高
    <properties resource="">
    	<properties name="" value=""/>
        <properties name="" value=""/>
    </properties>
    #setting用于配置mybatis的一些行为
    <setting>
        #全局缓存开启或者关闭
    	<setting name="cacheEnabled" value="true"/>
        #延时加载是否开启
        <setting name="lazyLoadingEnabled" value="true"/>
        #是否允许单个语句返回多个结果集
        <setting name="multipleResultSetsEnabled" value="true"/>
        #列相关
        <setting name="useColumnLabel" value="true"/>
        #允许 JDBC 支持自动生成主键，需要数据库驱动支持。如果设置为 true，将强制使用自动生成主键。尽管一些数据库驱动不支持此特性，但仍可正常工作（如 Derby）
        <setting name="useGeneratedKeys" value="false"/>
        #指定 MyBatis 应如何自动映射列到字段或属性。 NONE 表示关闭自动映射；PARTIAL 只会自动映射没有定义嵌套结果映射的字段。 FULL 会自动映射任何复杂的结果集（无论是否嵌套）。
        <setting name="autoMappingBehavior" value="PARTIAL"/>
        #指定发现自动映射目标未知列（或未知属性类型）的行为。
NONE: 不做任何反应
WARNING: 输出警告日志（'org.apache.ibatis.session.AutoMappingUnknownColumnBehavior' 的日志等级必须设置为 WARN）
FAILING: 映射失败 (抛出 SqlSessionException)
        <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
        #配置默认的执行器。SIMPLE 就是普通的执行器；REUSE 执行器会重用预处理语句（PreparedStatement）； BATCH 执行器不仅重用语句还会执行批量更新。
        <setting name="defaultExecutorType" value="SIMPLE"/>
        #请求默认超时时间
        <setting name="defaultStatementTimeout" value="25"/>
        #为驱动的结果集获取数量（fetchSize）设置一个建议值。此参数只可以在查询设置中被覆盖。
         <setting name="defaultFetchSize" value="100"/>      
    </setting>
    
    #类型别名(为XML配置类执行一个简单的名字)
    <typeAliases>
  			<typeAlias alias="Author" type="domain.blog.Author"/>
  			<typeAlias alias="Blog" type="domain.blog.Blog"/>
  			<typeAlias alias="Comment" type="domain.blog.Comment"/>
 		 	<typeAlias alias="Post" type="domain.blog.Post"/>
  			<typeAlias alias="Section" type="domain.blog.Section"/>
 			<typeAlias alias="Tag" type="domain.blog.Tag"/>
	</typeAliases>
    #类型处理器 （结果集处理器，将数据库类型转换为对应的java类型）
   	<objectFactory type="org.mybatis.example.ExampleObjectFactory">
  		<property name="someProperty" value="100"/>
	</objectFactory>
    #插件(plugins)有个分页插件需进行考虑，后续配置
    
    #映射器（mapper）
    <mappers>
    	#使用文件
        <mapper resource="Mapper.xml文件位置"></mapper>
        <mapper class="对应的接口"></mapper>
        <mapper name="包名，包下的所有都会被注册为mapper"></mapper>
    </mappers>
</configuration>
```

### 3.mapper配置文件结构

```xml
#配置文件約束
<?xml version="1.0" encoding="UTF-8" ?>
<!DOCTYPE mapper
        PUBLIC "-//mybatis.org//DTD Config 3.0//EN"
        "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
<mapper>
	#cache 配置缓存相关
    <cache></cache>
    
    #cache-ref 引用其他命名空间的缓存配置
    <cache-ref></cache-ref>
    
    #resultMap 描述如何从数据库结果集中加载对象，是最复杂的最强大的参数
    <resultMap></resultMap>
    
    #sql 可被引用的sql语句段
    <sql></sql>
    
    #insert 插入
    
    #update 更新
    
    #delete 删除
    <delete></delete>
    
    
    
    #select 查询 id在命名空间中唯一的标识符，可以被用来引用这条语句。 parameterType将会传入这条语句的参数的类全限定名或别名。这个属性是可选的，因为 MyBatis 可以通过类型处理器（TypeHandler）推断出具体传入语句的参数，默认值为未设置（unset）。resultType期望从这条语句中返回结果的类全限定名或别名。 注意，如果返回的是集合，那应该设置为集合包含的类型，而不是集合本身的类型。 resultType 和 resultMap 之间只能同时使用一个。
    	对外部 resultMap 的命名引用。结果映射是 MyBatis 最强大的特性，如果你对其理解透彻，许多复杂的映射问题都能迎刃而解。 resultType 和 resultMap 之间只能同时使用一个。
    flushCache将其设置为 true 后，只要语句被调用，都会导致本地缓存和二级缓存被清空，默认值：false
    useCache将其设置为 true 后，将会导致本条语句的结果被二级缓存缓存起来，默认值：对 select 元素为 true。
    timeout这个设置是在抛出异常之前，驱动程序等待数据库返回请求结果的秒数。默认值为未设置（unset）（依赖数据库驱动）。
    fetchSize这是一个给驱动的建议值，尝试让驱动程序每次批量返回的结果行数等于这个设置值。 默认值为未设置（unset）（依赖驱动）。
    statementType	可选 STATEMENT，PREPARED 或 CALLABLE。这会让 MyBatis 分别使用 Statement，PreparedStatement 或 CallableStatement，默认值：PREPARED。
    resultSetTypeFORWARD_ONLY，SCROLL_SENSITIVE, SCROLL_INSENSITIVE 或 DEFAULT（等价于 unset） 中的一个，默认值为 unset （依赖数据库驱动）。
    databaseId如果配置了数据库厂商标识（databaseIdProvider），MyBatis 会加载所有不带 databaseId 或匹配当前 databaseId 的语句；如果带和不带的语句都有，则不带的会被忽略。
    resultOrdered这个设置仅针对嵌套结果 select 语句：如果为 true，将会假设包含了嵌套结果集或是分组，当返回一个主结果行时，就不会产生对前面结果集的引用。 这就使得在获取嵌套结果集的时候不至于内存不够用。默认值：false。
    resultSets这个设置仅适用于多结果集的情况。它将列出语句执行后返回的结果集并赋予每个结果集一个名称，多个名称之间以逗号分隔
    <select id="" parameterType="" resultType="" resultMap="" flushCache="" useCache="" timeout="" statementType="" resultSetType=""> </select>
    
    
</mapper>
```

### 4.简单demo

```java
import dao.CityDao;
import dao.LanguageDao;
import org.apache.ibatis.io.Resources;
import org.apache.ibatis.session.Configuration;
import org.apache.ibatis.session.SqlSession;
import org.apache.ibatis.session.SqlSessionFactory;
import org.apache.ibatis.session.SqlSessionFactoryBuilder;
import po.City;
import po.Language;

import java.io.IOException;
import java.io.InputStream;
import java.util.List;

public class Main {
    public static void main(String[] args) throws IOException {
        String resource = "mybatis-config.xml";
        InputStream stream = Resources.getResourceAsStream(resource);
        SqlSessionFactory sqlSessionFactory = new SqlSessionFactoryBuilder().build(stream);

        SqlSession sqlSession = sqlSessionFactory.openSession();

        Configuration configuration= sqlSession.getConfiguration();

        CityDao cityDao = sqlSession.getMapper(CityDao.class);

        LanguageDao languageDao = sqlSession.getMapper(LanguageDao.class);

        List<Language> languages = languageDao.getLanguageSize(10);

        System.out.println(languages.size() + " " + languages);

        List<City> list = cityDao.getCityByIdSize(5);

        System.out.println(list.size() + " " + list);

        sqlSession.close();

        stream.close();
    }
}
```

### 5.从demo到内部原理

1. SqlSessionFactory

   ```txt
   sqlSessionFacoty接口（两个子类）此处使用DefaultSqlSessionFactory子类，此子类有一个Configuration属性（配置相关，配置内容与全局配置文件一一对应，我猜 由SqlSessionFactoryBuilder创建而来，解析xml文件得到）
   ```

2. Configuration属性

   ```
   public class Configuration {
       protected Environment environment; // 对应环境属性
       protected boolean safeRowBoundsEnabled;  //
       protected boolean safeResultHandlerEnabled;
       protected boolean mapUnderscoreToCamelCase;
       protected boolean aggressiveLazyLoading;
       protected boolean multipleResultSetsEnabled;
       protected boolean useGeneratedKeys;
       protected boolean useColumnLabel;
       protected boolean cacheEnabled;
       protected boolean callSettersOnNulls;
       protected boolean useActualParamName;
       protected boolean returnInstanceForEmptyRow;
       protected String logPrefix;
       protected Class<? extends Log> logImpl;
       protected Class<? extends VFS> vfsImpl;
       protected LocalCacheScope localCacheScope;
       protected JdbcType jdbcTypeForNull;
       protected Set<String> lazyLoadTriggerMethods;
       protected Integer defaultStatementTimeout;
       protected Integer defaultFetchSize;
       protected ExecutorType defaultExecutorType;
       protected AutoMappingBehavior autoMappingBehavior;
       protected AutoMappingUnknownColumnBehavior autoMappingUnknownColumnBehavior;
       protected Properties variables;
       protected ReflectorFactory reflectorFactory;
       protected ObjectFactory objectFactory;
       protected ObjectWrapperFactory objectWrapperFactory;
       protected boolean lazyLoadingEnabled;
       protected ProxyFactory proxyFactory;
       protected String databaseId;
       protected Class<?> configurationFactory;
       protected final MapperRegistry mapperRegistry;
       protected final InterceptorChain interceptorChain;
       protected final TypeHandlerRegistry typeHandlerRegistry;
       protected final TypeAliasRegistry typeAliasRegistry;
       protected final LanguageDriverRegistry languageRegistry;
       protected final Map<String, MappedStatement> mappedStatements;
       protected final Map<String, Cache> caches;
       protected final Map<String, ResultMap> resultMaps;
       protected final Map<String, ParameterMap> parameterMaps;
       protected final Map<String, KeyGenerator> keyGenerators;
       protected final Set<String> loadedResources;
       protected final Map<String, XNode> sqlFragments;
       protected final Collection<XMLStatementBuilder> incompleteStatements;
       protected final Collection<CacheRefResolver> incompleteCacheRefs;
       protected final Collection<ResultMapResolver> incompleteResultMaps;
       protected final Collection<MethodResolver> incompleteMethods;
       protected final Map<String, String> cacheRefMap;
   }
   ```

   ### 5.mybatis动态代理

   ```
   //1.被代理的类，自己写的dao
   例如UserDao
   //2.InvocationHandler 类 MapperProxy类
   //3.invoke接口的实现
   //4.final 关键字：当修饰类表示无法被继承 修饰方法表示不能被重写  修饰变量如果是基础类型表示不能被改写，对象表示指向的对象不能改变
   ```

   ### 6.mybatis中的resultmap

   ```
   
   ```

   

   

   

   ### 7.数据库6大范式

   ```tcl
   5NF ∈ 4NF ∈ BCNF ∈ 3NF ∈ 2NF ∈ 1NF
   1.第一范式（1NF）
   表设计一列只能有单一属性，不能有多个属性，即数据项不可再分
   2.第二范式（2NF）
   ```

   ### 8.mybatis缓存

   ```
   1.
   ```

   



