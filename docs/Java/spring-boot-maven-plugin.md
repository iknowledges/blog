# Spring Boot项目编译后无法扫描到子包中的注解

## 具体配置

可执行jar包的maven插件配置：

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <executions>
                <execution>
                    <goals>
                        <goal>repackage</goal>
                    </goals>
                </execution>
            </executions>
        </plugin>
    </plugins>
</build>
```

被依赖maven子模块插件配置：

```xml
<build>
    <plugins>
        <plugin>
            <groupId>org.springframework.boot</groupId>
            <artifactId>spring-boot-maven-plugin</artifactId>
            <configuration>
                <classifier>exec</classifier>
            </configuration>
        </plugin>
    </plugins>
</build>
```

### 参数说明

#### spring-boot-maven-plugin的goals：

- spring-boot:build-image：使用构建包将一个应用程序打包成一个OCI镜像。
- spring-boot:build-info：生成Actuator使用的构建信息文件build-info.properties。
- spring-boot:repackage：默认goal。执行mvn package之后，再次打包生成可执行的jar/war文件，同时保留后缀为.original的文件。打包生成的jar或者war包可以在命令行使用jar -jar来执行，使用layout=NONE也可以简单的打包有嵌套依赖的jar(没有主类，所以无法执行)。
- spring-boot:run：运行Spring Boot应用。
- spring-boot:start：在mvn integration-test阶段，进行Spring Boot应用生命周期的管理。启动Spring应用程序，和run目标不同，该目标不会阻塞，并且允许其他目标来操作应用程序。这个目标通常是在应用程序集成测试套件开始之前和停止之后的继承测试脚本中使用。集成spring boot应用程序到集成测试阶段，从而使应用程序在集成测试程序之前启动。
- spring-boot:stop：在mvn integration-test阶段，进行Spring Boot应用生命周期的管理，停止使用start目标启动的spring应用程序，通常在测试套件完成后被调用。集成spring boot应用程序到集成测试阶段，从而使应用程序在集成测试程序之前启动。

#### classifier

classifier用于指定repackage打包后的文件后缀，如果没有设置，主包就是repackage打包生成的文件。如果设置了，主包就会打包源代码，repackage会打包成一个以classifier为后缀的文件。

#### 参考链接

[Spring Boot Maven Plugin Documentation](https://docs.spring.io/spring-boot/docs/2.3.0.RELEASE/maven-plugin/reference/html/)