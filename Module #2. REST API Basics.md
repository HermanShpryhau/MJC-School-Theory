# Module #2. REST API Basics

## Spring

### IoC & DI. What is the difference? Other IoC implementations.
> [Baeldung article](https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring)

**Inversion of Control** is a principle in software engineering which transfers the control of objects or portions of a program to a container or framework. We most often use it in the context of object-oriented programming.

In contrast with traditional programming, in which our custom code makes calls to a library, IoC enables a framework to take control of the flow of a program and make calls to our custom code. To enable this, frameworks use abstractions with additional behavior built in. If we want to add our own behavior, we need to extend the classes of the framework or plugin our own classes.

The advantages of this architecture are:

- decoupling the execution of a task from its implementation
- making it easier to switch between different implementations
- greater modularity of a program
- greater ease in testing a program by isolating a component or mocking its dependencies, and allowing components to communicate through contracts

We can achieve Inversion of Control through various mechanisms such as: *[Strategy design pattern](https://en.wikipedia.org/wiki/Strategy_pattern)*, *[Service Locator pattern](https://ru.wikipedia.org/wiki/Локатор_служб)*, *Factory pattern*, and *Dependency Injection*.

**Dependency injection** is a pattern we can use to implement IoC, where the control being inverted is setting an object's dependencies. Connecting objects with other objects, or “injecting” objects into other objects, is done by an assembler rather than by the objects themselves.

An IoC container is a common characteristic of frameworks that implement IoC. In the Spring framework, the interface ApplicationContext represents the IoC container. The Spring container is responsible for instantiating, configuring and assembling objects known as beans, as well as managing their life cycles.

### How can we configure Spring context?
> [*Baeldung article*](https://www.baeldung.com/spring-application-context)

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

`ApplicationContext` implementations:
- `AnnotationConfigApplicationContext` — Loads a Spring application context from one or more Java-based configuration classes
- `AnnotationConfigWebApplicationContext` — Loads a Spring web application context from one or more Java-based configuration classes
- `ClassPathXmlApplicationContext` — Loads a context definition from one or more XML files located in the classpath, treating context-definition files as class- path resources
- `FileSystemXmlApplicationContext` — Loads a context definition from one or more XML files in the filesystem
- `XmlWebApplicationContext` — Loads context definitions from one or more XML files contained in a web application

### Spring Bean lifecycle.

![](media/bean-lifecycle.png)\

1. Spring instantiates the bean.
2. Spring injects values and bean references into the bean’s properties.
3. If the bean implements BeanNameAware, Spring passes the bean’s ID to the `setBeanName()` method.
4. If the bean implements `BeanFactoryAware`, Spring calls the `setBeanFactory()` method, passing in the bean factory itself.
5. If the bean implements `ApplicationContextAware`, Spring calls the `setApplicationContext()` method, passing in a reference to the enclosing application context.
6. If the bean implements the `BeanPostProcessor` interface, Spring calls its `postProcessBeforeInitialization()` method.
7. If the bean implements the `InitializingBean` interface, Spring calls its `afterPropertiesSet()` method. Similarly, if the bean was declared with an **init-method**, then the specified initialization method is called.
8. If the bean implements `BeanPostProcessor`, Spring calls its `postProcessAfterInitialization()` method.
9. At this point, the bean is ready to be used by the application and remains in the application context until the application context is destroyed.
10. If the bean implements the `DisposableBean` interface, Spring calls its `destroy()` method. Likewise, if the bean was declared with a destroy-method, the specified method is called.


### Injection with annotations. What types exist? Advantages and disadvantages of those types? How did you choose the injection approach?

> [Baeldung article](https://www.baeldung.com/inversion-control-and-dependency-injection-in-spring)

#### Constructor-Based Dependency Injection
In the case of constructor-based dependency injection, the container will invoke a constructor with arguments each representing a dependency we want to set.

Spring resolves each argument primarily by type, followed by name of the attribute, and index for disambiguation.

```java
public class Store {
    private Item item;
    
    @Autowired
    public Store(Item item) {
        this.item = item;
    }
}
```

#### Setter-Based Dependency Injection
For setter-based DI, the container will call setter methods of our class after invoking a no-argument constructor or no-argument static factory method to instantiate the bean. 
It is possible to combine constructor-based and setter-based types of injection for the same bean. The Spring documentation recommends using constructor-based injection for mandatory dependencies, and setter-based injection for optional ones.
```java
public class Store {
    private Item item;
    
    public Store() {}
    
    @Autowired
    public setItem(Item item) {
        this.item = item;
    }
}
```

#### Field-Based Dependency Injection
In case of Field-Based DI, we can inject the dependencies by marking them with an `@Autowired` annotation:
```java
public class Store {
    @Autowired
    private Item item; 
}
```

Drawbacks of filed-based DI:
1. This method uses reflection to inject the dependencies, which is costlier than constructor-based or setter-based injection.
2. It's really easy to keep adding multiple dependencies using this approach. If we were using constructor injection, having multiple arguments would make us think that the class does more than one thing, which can violate the Single Responsibility Principle.

### What is the difference between `@Repository`, `@Component`, `@Service` and `@Controller` annotations?
`@Repository` and `@Service` are just aliases for `@Component` and only act as specialization for `@Component`.

`@Controller` is also a specialization for `@Component` but is processed differently under the hood. As a consequence annotations such as `@RequestMapping` will only work with `@Controller`s.

### Spring Bean scopes. What scopes exist out of the box? Is it possible to create a custom scope?
> [Baeldung article](https://www.baeldung.com/spring-bean-scopes)

The latest version of the Spring framework defines 6 types of scopes:
1. singleton
2. prototype
3. request
4. session
5. application
6. websocket

The last four scopes mentioned, request, session, application and websocket, are only available in a web-aware application.

The scope of bean is defined by `@Scope` annotation:
```java
@Bean
@Scope("prototype")
// @Scope(value = ConfigurableBeanFactory.SCOPE_PROTOTYPE)
public ExampleBean exampleBean() {
    return new ExampleBean();
}
```

#### Singleton Scope
When we define a bean with the **singleton scope**, the container creates a single instance of that bean; all requests for that bean name will return the same object, which is cached. Any modifications to the object will be reflected in all references to the bean. This scope is the default value if no other scope is specified.

#### Prototype Scope
A bean with the **prototype scope** will return a different instance every time it is requested from the container.

#### Request Scope
The **request scope** creates a bean instance for a single HTTP request.

#### Session Scope
The **session scope** creates a bean instance for an HTTP session.

#### Application Scope
The **application scope** creates the bean instance for the lifecycle of a **Servlet Context**.

This is similar to the singleton scope, but there is a very important difference with regard to the scope of the bean.

When beans are application scoped, the same instance of the bean is shared across multiple servlet-based applications running in the same **Servlet Context**, while singleton scoped beans are scoped to a single application context only.

#### WebSocket Scope
When first accessed, **WebSocket scoped** beans are stored in the WebSocket session attributes. The same instance of the bean is then returned whenever that bean is accessed during the entire WebSocket session. We can also say that it exhibits singleton behavior, but limited to a WebSocket session only.

### How to add several configurations in application (db, connection pool settings) and choose one of them at startup?
> [Baeldung article](https://www.baeldung.com/spring-profiles)

Profiles allow us to map our beans to different profiles — for example, dev, test, and prod.

The `@Profile` annotation is used to map the bean to that particular profile; the annotation simply takes the names of one (or multiple) profiles.
```java
@Component
@Profile("dev")
public class DevDatasourceConfig
```

Profiles can be set in different ways. Here are some of them:

#### Via `WebApplicationInitializer`
```java
@Configuration
public class MyWebApplicationInitializer 
  implements WebApplicationInitializer {

    @Override
    public void onStartup(ServletContext servletContext) throws ServletException {
 
        servletContext.setInitParameter(
          "spring.profiles.active", "dev");
    }
}
```

#### Via ConfigurableEnvironment
```java
@Autowired
private ConfigurableEnvironment env;
...
env.setActiveProfiles("someProfile");
```

#### As context Parameter in web.xml
```xml
<context-param>
    <param-name>spring.profiles.active</param-name>
    <param-value>dev</param-value>
</context-param>
```

#### As JVM System Parameter
```
-Dspring.profiles.active=dev
```
