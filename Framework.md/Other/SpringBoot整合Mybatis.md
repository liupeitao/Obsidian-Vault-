# SpringBoot整合Mybatis

#### 1. 引入依赖

```xml
<dependency>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-starter-jdbc</artifactId>
</dependency>  

<dependency>
            <groupId>org.mybatis.spring.boot</groupId>
            <artifactId>mybatis-spring-boot-starter</artifactId>
            <version>2.1.1</version>
 </dependency>
```

> 数据源依赖和 jdbc 依赖也添加到配置文件中（不添加，将会使用默认数据源和 jdbc 配置

#### 2.  application.properties 配置

​     Spring Boot 整合 MyBatis 时几个比较需要注意的配置参数：

- **mybatis.config-location**

  配置 `mybatis-config.xml` 路径，`mybatis-config.xml` 中配置 MyBatis 基础属性，如果项目中配置了 `mybatis-config.xml` 文件需要设置该参数。
- **mybatis.mapper-locations**

  配置 Mapper 文件对应的 XML 文件路径。
- **mybatis.type-aliases-package**

  配置项目中实体类包路径

```xml
mybatis.config-location=classpath:mybatis-config.xml
mybatis.mapper-locations=classpath:mapper/*Dao.xml
mybatis.type-aliases-package=com.lou.springboot.entity
```

我们只配置 mapper-locations 即可，最终的 `application.properties` 文件如下：

```properties
# datasource config
spring.datasource.url=jdbc:mysql://localhost:3306/lou_springboot?useUnicode=true&characterEncoding=utf8&autoReconnect=true&useSSL=false
spring.datasource.driver-class-name=com.mysql.cj.jdbc.Driver
spring.datasource.username=root
spring.datasource.password=
mybatis.mapper-locations=classpath:mapper/*Dao.xml
```

....

#### 3. 创建 Mapper 接口的映射文件

> mapper文件目录(resources/mapper目录  Mapper 接口)

1. Mapper 接口的对应关系，比如该示例中，需要将 `UserDao.xml` 的与对应的 `UserDao` 接口类之间的关系定义出来：

```xml
       <mapper namespace="com.lou.springboot.dao.UserDao">
```

2. 之后，配置表结构和实体类的对应关系：

   ```xml
       <resultMap type="com.lou.springboot.entity.User" id="UserResult">
           <result property="id" column="id"/>
           <result property="name" column="name"/>
           <result property="password" column="password"/>
       </resultMap>
   ```
3. 最后，针对对应的接口方法，编写具体的 SQL 语句，最终的 `UserDao.xml` 文件如下：

   ```xml
   <?xml version="1.0" encoding="UTF-8"?>
   <!DOCTYPE mapper PUBLIC "-//mybatis.org//DTD Mapper 3.0//EN" "http://mybatis.org/dtd/mybatis-3-mapper.dtd">
       <mapper namespace="com.lou.springboot.dao.UserDao">
       <resultMap type="com.lou.springboot.entity.User" id="UserResult">
           <result property="id" column="id"/>
           <result property="name" column="name"/>
           <result property="password" column="password"/>
       </resultMap>
       <select id="findAllUsers" resultMap="UserResult">
           select id,name,password from tb_user
           order by id desc
       </select>
       <insert id="insertUser" parameterType="com.lou.springboot.entity.User">
           insert into tb_user(name,password)
           values(#{name},#{password})
       </insert>
       <update id="updUser" parameterType="com.lou.springboot.entity.User">
           update tb_user
           set
           name=#{name},password=#{password}
           where id=#{id}
       </update>
       <delete id="delUser" parameterType="int">
           delete from tb_user where id=#{id}
       </delete>
   </mapper>
   ```
