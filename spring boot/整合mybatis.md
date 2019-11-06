### 1.配置pom文件，添加依赖

```xml
    <dependency>
        <groupId>org.mybatis.spring.boot</groupId>
        <artifactId>mybatis-spring-boot-starter</artifactId>
        <version>1.1.1</version>
    </dependency>
```

### 2.配置方式

- 注解形式配置

  - application.properties配置

    ```properties
    #zhu注解形式扫描实体类路径
    mybatis.type-aliases-package=cn.yang.core.entity
    ```

  - 启动方法配置

    ```java
    @SpringBootApplication
    @MapperScan("cn.yang.core.dao") //mybatis mapper接口文件所在包路径
    public class Application {
        public static void main(String[] args) {
            SpringApplication.run(Application.class, args);
        }
    }
    ```

- XML形式配置

  - application.properties配置

    ```properties
    mybatis.config-locations=classpath:mybatis-config.xml
    mybatis.mppper-locations=classpath:cn/yang/core/mapper
    ```

    

  - mybatis-config.xml配置文件配置

    ```xml
    <?xml version="1.0" encoding="utf-8"?>  
    <!DOCTYPE configuration PUBLIC "-//mybatis.org//DTD Config 3.0//EN" 
    "http://mybatis.org/dtd/mybatis-3-config.dtd">
    <configuration>
        <settings>
            <!-- 这个配置使全局的映射器启用或禁用缓存 -->  
            <setting name="cacheEnabled" value="false" />
            <!-- 全局启用或禁用延迟加载。当禁用时，所有关联对象都会即时加载 -->  
            <setting name="lazyLoadingEnabled" value="true" />
            <!-- 当启用时，有延迟加载属性的对象在被调用时将会完全加载任意属性。否则，每种属性将会按需要加载 -->  
            <setting name="aggressiveLazyLoading" value="true" />
            <!-- 允许或不允许多种结果集从一个单独的语句中返回（需要适合的驱动） -->  
            <setting name="multipleResultSetsEnabled" value="true" />
            <!-- 使用列标签代替列名。不同的驱动在这方便表现不同。参考驱动文档或充分测试两种方法来决定所使用的驱动 -->  
            <setting name="useColumnLabel" value="true" />
            <!-- 允许JDBC支持生成的键。需要适合的驱动。如果设置为true则这个设置强制生成的键被使用，尽管一些驱动拒绝兼容但仍然有效（比如Derby） -->  
            <setting name="useGeneratedKeys" value="true" />
            <!-- 指定MyBatis如何自动映射列到字段/属性。PARTIAL只会自动映射简单，没有嵌套的结果。FULL会自动映射任意复杂的结果（嵌套的或其他情况） -->  
            <setting name="autoMappingBehavior" value="PARTIAL" />
            <!--当检测出未知列（或未知属性）时，如何处理，默认情况下没有任何提示，这在测试的时候很不方便，不容易找到错误。
            NONE : 不做任何处理 (默认值)
            WARNING : 警告日志形式的详细信息
            FAILING : 映射失败，抛出异常和详细信息
            -->
            <setting name="autoMappingUnknownColumnBehavior" value="WARNING"/>
            <!-- 配置默认的执行器。SIMPLE执行器没有什么特别之处。REUSE执行器重用预处理语句。BATCH执行器重用语句和批量更新 -->  
            <setting name="defaultExecutorType" value="SIMPLE" />
            <!-- 设置超时时间，它决定驱动等待一个数据库响应的时间 -->  
            <setting name="defaultStatementTimeout" value="25000" />
            <!--设置查询返回值数量，可以被查询数值覆盖  -->
            <setting name="defaultFetchSize" value="100"/>
            <!-- 允许在嵌套语句中使用分页-->
            <setting name="safeRowBoundsEnabled" value="false"/>
            <!--是否开启自动驼峰命名规则映射，即从经典数据库列名 A_COLUMN 到经典 Java 属性名 aColumn 的类似映射。-->
            <setting name="mapUnderscoreToCamelCase" value="false"/>
            <!--MyBatis利用本地缓存机制(Local Cache)防止循环引用(circular references)和加速重复嵌套查询。 默认值为 SESSION，这种情况下会缓存一个会话中执行的所有查询。 若设置值为 STATEMENT，本地会话仅用在语句执行上，对相同 SqlSession 的不同调用将不会共享数据。-->
            <setting name="localCacheScope" value="SESSION"/>
            <!-- 当没有为参数提供特定的 JDBC 类型时，为空值指定 JDBC 类型。 某些驱动需要指定列的 JDBC 类型，多数情况直接用一般类型即可，比如 NULL、VARCHAR、OTHER。-->
            <setting name="jdbcTypeForNull" value="OTHER"/>
            <!-- 指定哪个对象的方法触发一次延迟加载。-->
            <setting name="lazyLoadTriggerMethods" value="equals,clone,hashCode,toString"/>
        </settings>
        <typeAliases>
            <typeAlias alias="Integer" type="java.lang.Integer" />
            <typeAlias alias="Long" type="java.lang.Long" />
            <typeAlias alias="HashMap" type="java.util.HashMap" />
            <typeAlias alias="LinkedHashMap" type="java.util.LinkedHashMap" />
            <typeAlias alias="ArrayList" type="java.util.ArrayList" />
            <typeAlias alias="LinkedList" type="java.util.LinkedList" />
        </typeAliases>
    </configuration>
    ```

  - Mapper.xml配置

    ```xml
    <mapper namespace="com.test.springboot.mybatis.dao.UserMapper">
    
        <!-- 用于一对多演示的Address类映射关系 -->
        <resultMap id="AddressResultMap" type="com.test.springboot.mybatis.bean.AddressPo">
            <id column="address_id" jdbcType="INTEGER" property="id" />
            <result column="address" jdbcType="VARCHAR" property="address" />
            <result column="user_id" jdbcType="VARCHAR" property="user_id" />
        </resultMap>
    
        <!-- 用户实体类映射关系 -->
        <resultMap id="UserResultMap" type="com.test.springboot.mybatis.bean.UserPo">
            <id column="id" jdbcType="VARCHAR" property="id" />
            <result column="username" jdbcType="VARCHAR" property="username" />
            <result column="password" jdbcType="VARCHAR" property="password" />
            <result column="realname" jdbcType="VARCHAR" property="realname" />
            <result column="company" jdbcType="VARCHAR" property="company" />
            <result column="job" jdbcType="VARCHAR" property="job" />
            <result column="salt" jdbcType="VARCHAR" property="salt" />
            <result column="status" jdbcType="TINYINT" property="status" />
            <result column="time" jdbcType="BIGINT" property="time" />
            <!-- 一对多映射 -->
            <collection property="addressList" resultMap="AddressResultMap"/>
        </resultMap>
    
        <sql id="Base_Column_List" >
            id, username, password, realname, company, job, salt, status, time
        </sql>
    
        <!-- 对应于接口文件中的 queryList方法，注意：这里的select id需要与接口文件中的方法名一致，否则会找不到 -->
        <select id="queryList" resultMap="UserResultMap">
            SELECT 
                <include refid="Base_Column_List"/>
            FROM auth_user
            <where>
                <if test="username != null">
                    username = #{username}
                </if>
                <if test="job != null">
                    AND job = #{job}
                </if>
            </where>
        </select>
    
        <!-- 一对多 多表联查 -->
        <select id="queryByUsername" resultMap="UserResultMap">
            SELECT
                u.id, u.username as aaa, u.password, u.realname, u.company, u.job, u.salt, u.status, u.time, a.address, a.id as address_id, a.user_id 
            FROM 
                auth_user 
            AS u LEFT JOIN auth_address AS a ON u.id = a.user_id WHERE u.username = #{username}
        </select>
    
    </mapper>
    ```

  - Mapper接口文件的编写

    ```java
    public interface UserMapper {
    
        @Select("select * from auth_user")
        public List<UserPo> queryAll();
    
        @Select("select * from auth_user where id = #{id}")
        public UserPo queryById(String id);
    
        @Insert("INSERT INTO auth_user(id, username, password) VALUES(#{id}, #{username}, #{password})")
        public int insert(UserPo user);
    
        @Delete("DELETE FROM auth_user WHERE id = #{id}")
        public int delete(String id);
    
        @Update("UPDATE auth_user SET username=#{username}, password=#{password}, realname=#{realname} WHERE id =#{id}")
        public int update(UserPo user);
    
        public List<UserPo> queryList(@Param("username")String username, @Param("job") String job);
    
        public UserPo queryByUsername(@Param("username")String username);  
    }
    ```

    

    

​           