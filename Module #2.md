# Module #2. REST API Basics

## Spring

### IoC & DI. What is the difference? Other IoC implementations.
[*Статья на Хабре*](https://habr.com/ru/post/131993/)
**Inversion of Control** (инверсия управления) — это некий абстрактный принцип, набор рекомендаций для написания слабо связанного кода. Суть которого в том, что каждый компонент системы должен быть как можно более изолированным от других, не полагаясь в своей работе на детали конкретной реализации других компонентов.
**Dependency Injection** (внедрение зависимостей) — это одна из реализаций этого принципа (помимо этого есть еще *[Factory Method](https://en.wikipedia.org/wiki/Factory_(object-oriented_programming)), [Service Locator](https://en.wikipedia.org/wiki/Service_locator_pattern)*).
**IoC-контейнер** — это какая-то библиотека, фреймворк, программа если хотите, которая позволит вам упростить и автоматизировать написание кода с использованием данного подхода на столько, на сколько это возможно. 

### How can we configure Spring context?
[*Baeldung article*](https://www.baeldung.com/spring-application-context)
1. **Java-based configuration.** Since Spring 3.0 Java-based configuration is available. Java configuration typically uses `@Bean`-annotated methods within a `@Configuration` class. The `@Bean` annotation on a method indicates that the method creates a Spring bean. Moreover, a class annotated with` @Configuration` indicates that it contains Spring bean configurations.
2. **Annotation-based configuration**. Spring 2.5 introduced annotation-based configuration as the first step to enable bean configurations in Java.In this approach, we first enable annotation-based configuration via XML configuration. Then we use a set of annotations on our Java classes, methods, constructors, or fields to configure beans. Some examples of these annotations are `@Component`, `@Controller`, `@Service`, `@Repository`, `@Autowired`, and `@Qualifier`.
```xml
<context:annotation-config/>
<context:component-scan base-package="com.baeldung.applicationcontext"/>
```

3. **XML-based configuration.** This is the traditional way of configuring beans in Spring. In this approach, we do all bean mappings in an XML configuration file.

```xml
<?xml version="1.0" encoding="UTF-8"?>
<beans xmlns="http://www.springframework.org/schema/beans"
  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance"
  xsi:schemaLocation="
    http://www.springframework.org/schema/beans 
    http://www.springframework.org/schema/beans/spring-beans.xsd">
	  
  <bean id="accountService" class="com.baeldung.applicationcontext.AccountService">
    <constructor-arg name="accountRepository" ref="accountRepository" />
  </bean>
	
  <bean id="accountRepository" class="com.baeldung.applicationcontext.AccountRepository" />
</beans>
```

### Spring Bean lifecycle.

### Injection with annotations. What types exist? Advantages and disadvantages of those types? How did you choose the injection approach?

### What is the difference between Repository, Component, Service and Controller annotations?

### Spring Bean scopes. What scopes exist out of the box? Is it possible to create a custom scope?

### How add several configurations in application (db, connection pool settings) and choose one of them at startup?