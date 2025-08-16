# Mybatis配置多数据源

## 配置文件application.yml

```yml
spring:
  datasource:
    user:
      jdbc-url: jdbc:mysql://localhost:3306/user
      username: root
      password: test
      driver-class-name: com.mysql.cj.jdbc.Driver
    blog:
      jdbc-url: jdbc:mysql://localhost:3306/blog
      username: root
      password: test
      driver-class-name: com.mysql.cj.jdbc.Driver
```

## 数据源配置

User的配置类

```java
@Configuration
@MapperScan(basePackages = "com.demo.dao.user", sqlSessionFactoryRef = "userSqlSessionFactory", sqlSessionTemplateRef = "userSqlSessionTemplate")
public class UserDatabaseConfig {

    @Bean(name = "userDataSource")
    @ConfigurationProperties(prefix = "spring.datasource.user")
    public DataSource dataSource() {
        return DataSourceBuilder.create().build();
    }

    @Bean(name = "userSqlSessionFactory")
    public SqlSessionFactory sqlSessionFactory() throws Exception {
        SqlSessionFactoryBean bean = new SqlSessionFactoryBean();
        bean.setDataSource(dataSource());
        bean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources("classpath:mapper/user/*.xml"));
        return bean.getObject();
    }

    // MapperFactoryBean会用到sqlSessionTemplate
    @Bean(name = "userSqlSessionTemplate")
    public SqlSessionTemplate sqlSessionTemplate() throws Exception {
    return new SqlSessionTemplate(sqlSessionFactory());
    }

    @Bean(name = "userTransactionManager")
    public DataSourceTransactionManager dataSourceTransactionManager() {
        return new DataSourceTransactionManager(dataSource());
    }
}
```

Blog的配置类

```java
@Configuration
@MapperScan(basePackages = "com.demo.dao.blog", sqlSessionFactoryRef = "blogSqlSessionFactory", sqlSessionTemplateRef = "blogSqlSessionTemplate")
public class BlogDatabaseConfig {

    @Bean(name = "blogDataSource")
    @ConfigurationProperties(prefix = "spring.datasource.blog")
    public DataSource dataSource() {
        return DataSourceBuilder.create().build();
    }

    @Bean(name = "blogSqlSessionFactory")
    public SqlSessionFactory sqlSessionFactory() throws Exception {
        SqlSessionFactoryBean bean = new SqlSessionFactoryBean();
        bean.setDataSource(dataSource());
        bean.setMapperLocations(new PathMatchingResourcePatternResolver().getResources("classpath:mapper/blog/*.xml"));
        return bean.getObject();
    }

    @Bean(name = "blogSqlSessionTemplate")
    public SqlSessionTemplate sqlSessionTemplate() throws Exception {
        return new SqlSessionTemplate(sqlSessionFactory());
    }

    @Bean(name = "blogTransactionManager")
    public DataSourceTransactionManager dataSourceTransactionManager() {
        return new DataSourceTransactionManager(dataSource());
    }
}
```

#### 其他注意点

- @Transactional注解要指定事务管理器名称，如：

```java
@Transactional(value = "userTransactionManager")
```

- classpath中的xml文件要放在不同目录，否则会扫描不到。
