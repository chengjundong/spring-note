# Spring 5 Indexer加速Application启动过程
引入indexer包可以在build spring-boot JAR的时候生成一个`spring.components`文件。其中会列出所有需要初始化的bean，这样在容器启动的时候，可以直接按名字初始化这些bean放入context中，无需基于注解扫描，从而节省启动速度。  
[如何创建spring.components文件](https://stackoverflow.com/questions/47254907/how-can-i-create-a-spring-5-component-index)
```
# sample spring.components文件
jd.cheng.anotherone.AnotherBean=jd.cheng.anotherone.AnotherBean
jd.cheng.jaredwebflux.WebAutoConfiguration=org.springframework.stereotype.Component
jd.cheng.jaredwebflux.WebConfiguration=org.springframework.stereotype.Component
```
# 解析@Configuration
参考`org.springframework.context.annotation.ConfigurationClassParser`，在Spring beanFactory启动的时候，使用这个parser读取所有的configuration class，继而解析其中的bean

# 使用@EnableAutoConfiguration及spring.factories代替@SpringBootApplication
- 被`@EnableAutoConfiguration`注解的类，可以作为`SpringApplication.run`的第一入参
- 通过`spring.factories`文件，可以指定auto configuration读取哪个configuration class
```
# sample spring.factories文件；需要放在main/resources/META-INF下
org.springframework.boot.autoconfigure.EnableAutoConfiguration=\
jd.cheng.jaredwebflux.WebAutoConfiguration
```
