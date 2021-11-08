# Module #3. REST API Advanced

## Spring Boot

### What is Spring Boot? Name advantages and disadvantages of Spring Boot.

Despite the advantages of Spring Framework, authors decided to provide developers with some utilities that automate the configuration procedure and speed up the process of creating and deploying Spring applications. These utilities were combined under the general name of Spring Boot.

While the Spring Framework focuses on providing flexibility, Spring Boot seeks to reduce code length and simplify web application development. By leveraging annotation and boilerplate configuration, Spring Boot reduces the time it takes to develop applications. This capability helps you create standalone applications with less or almost no configuration overhead.

#### Ease of Dependency Management

To speed up the dependency management process, Spring Boot implicitly packages the required third-party dependencies for each type of Spring-based application and provides them through so-called starter packages (`spring-boot-starter-web`, `spring-boot-starter-data-jpa`, etc.).

Starter packages are collections of handy dependency descriptors that you can include in your application. They allow you to get a universal solution for all Spring-related technologies, removing the necessity to search for code examples and load the required dependency descriptors from them.

#### Automatic Сonfiguration

One of the advantages of Spring Boot, that is worth mentioning is automatic configuration of the application.

After choosing a suitable starter package, Spring Boot will try to automatically configure your Spring application based on the jar dependencies you added. For example, if you add `spring-boot-starter-web`, Spring Boot will automatically configure registered beans such as `DispatcherServlet`, `ResourceHandlers`, and `MessageSource`.

If you are using `spring-boot-starter-jdbc`, Spring Boot automatically registers the DataSource, EntityManagerFactory, and TransactionManager beans and reads the database connection information from the `application.properties` file. If you are not going to use a database and do not provide any details about connecting manually, Spring Boot will automatically configure the database in the memory without any additional configuration on your part (if you have H2 or HSQL libraries). Automatic configuration can be completely overridden at any time by using user preferences.

#### Native Support for Application Server – Servlet Container

Every Spring Boot application includes an embedded web server. Developers no longer have to worry about setting up a servlet container and deploying an application to it. The application can now run itself as an executable jar file using the built-in server. If you need to use a separate HTTP server, simply exclude the default dependencies. Spring Boot provides separate starter packages for different HTTP servers.

#### Disadvantages of Spring Boot

- Lack of control. Spring Boot creates a lot of unused dependencies, resulting in a large deployment file;
- The complex and time-consuming process of converting a legacy or an existing Spring project to a Spring Boot application;
- Not suitable for large-scale projects. Although it’s great for working with microservices, many developers claim that Spring Boot is not suitable for building monolithic applications.

### What are the ways to create Spring Boot application do you know?

1. Using [spring.io](http://spring.io/) initializer
2. Using Eclipse or any similar IDE and Maven simple project
3. Using Spring Tool Suite
4. Using CLI

```bash
$ spring init --dependencies=web,data-jpa my-project
```

### What Spring Boot annotations do you know?

#### `@SpringBootApplication`

We use this annotation to mark the main class of a Spring Boot application:

```java
@SpringBootApplication
class Application {
    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

`@SpringBootApplication` encapsulates `@Configuration`, `@EnableAutoConfiguration`, and `@ComponentScan` annotations with their default attributes.

#### `@EnableAutoConfiguration`

`@EnableAutoConfiguration`, as its name says, enables auto-configuration. It means that Spring Boot looks for auto-configuration beans on its classpath and automatically applies them.
Note, that we have to use this annotation with `@Configuration`.

#### `@ConditionalOnClass` and `@ConditionalOnMissingClass`

Using these conditions, Spring will only use the marked auto-configuration bean if the class in the annotation's argument is present/absent:

```java
@Configuration
@ConditionalOnClass(DataSource.class)
class MySQLAutoconfiguration {
    //...
}
```

#### `@ConditionalOnBean` and `@ConditionalOnMissingBean`

We can use these annotations when we want to define conditions based on the presence or absence of a specific bean:

```java
@Bean
@ConditionalOnBean(name = "dataSource")
LocalContainerEntityManagerFactoryBean entityManagerFactory() {
    // ...
}
```

#### And more conditional annotations...

#### @Conditional

For even more complex conditions, we can create a class evaluating the custom condition. We tell Spring to use this custom condition with `@Conditional`:

```java
@Conditional(HibernateCondition.class)
Properties additionalProperties() {
    //...
}
```

#### What are the Spring Boot properties?

Spring Boot provides various properties that can be configured in the `application.properties` file. The properties have default values. We can set a property(s) for the Spring Boot application. Spring Boot also allows us to define our own property if required.

The `application.properties` file allows us to run an application in a different environment. In short, we can use the `application.properties` file to:

- Configure the Spring Boot framework
- Override default Spring Boot values
- Define our application custom configuration properties

## What are the Spring Boot starters? Name some of them and their purpose.

Before Spring Boot was introduced, Spring Developers used to spend a lot of time on Dependency management. **Spring Boot Starters** were introduced to solve this problem so that the developers can spend more time on actual code than Dependencies. Spring Boot Starters are dependency descriptors that can be added under the <dependencies> section in pom.xml. There are around 50+ Spring Boot Starters for different Spring and related technologies. These starters give all the dependencies under a single name.

Spring Boot Starter consists of:

1. An auto-configure class for our library along with a properties class for custom configuration.
2. A starter *pom* to bring in the dependencies of the library and the autoconfigure project.

### What is Spring Boot auto-configuration?

**Spring Boot auto-configuration** attempts to automatically configure your Spring application based on the jar dependencies that you have added.

### What is Spring Boot Actuator used for?

**Spring Boot Actuator** is a sub-project of the Spring Boot Framework. It includes a number of additional features that help us to monitor and manage the Spring Boot application. It contains the actuator endpoints (the place where the resources live). We can use HTTP and JMX endpoints to manage and monitor the Spring Boot application. If we want to get production-ready features in an application, we should use the Spring Boot actuator.

### How to build and run spring boot application

#### a. What's included in spring boot starter?

1. An auto-configure class for our library along with a properties class for custom configuration.
2. A starter *pom* to bring in the dependencies of the library and the autoconfigure project.

#### b. How to run spring boot application from command line?

With Gradle:

```
$ ./gradlew bootRun
```

With Maven:

```
$ mvn spring-boot:run
```

With java:

```
$ java -jar path/to/app fully.qualified.package.Application
```

#### c. Where do the properties place when building WAR?

While in runnable Spring Boot jars they are put to `BOOT-INF/classes` , in `.war` archives they are put under the `WEB-INF/classes` path.

#### d. Where is the servlet container in Spring Boot and in normal Spring application

Unlike plain Spring applications where you would have to package everything in a `.war` archive and then deploy it to a servlet container running on your machine, Spring Boot Applications come with an embedded servlet container/server (a bunch of the in fact, but default id Tomcat). That way you can compile your.

## REST

### Why we should support filtering, sorting, and pagination in RESTful APIs?

Without pagination, a simple search could return millions or even billions of hits causing extraneous network traffic. We need to support filtering, sorting and searching for our APIs to more flexible.

### What are filtering, sorting, and pagination best practices can you name?

Filtering:

1. Using LHS brackets with an operator: `price[lte]=200` `status[ne]=past`
2. Using RHS colon with an operator: `price=lte:200` `status=ne:past`
3. Using Lucene syntax or ElasticSearch Simple Query strings directly: `q=price:<200` `q=-status:past`

### What is Richardson Maturity Model? Name the Levels of the Model and explain them.

The RMM can be employed to determine how well a Web service architecture adheres to REST principles. It categorizes a Web API into four levels (from 0 to 3) with each higher level corresponding to a more complete adherence to REST design. The next level also contains all the characteristics of the previous one.

#### Level 0 : The Swamp of POX

The lowest level of the model describes a Web API with a single URI (typically POST over HTTP) accepting all the range of operations supported by the service. Resources in this form cannot be well-defined. Messaging is done in XML, JSON, or other text formats. These are typical RPC POX and many SOAP services.

#### Level 1 : Resources

Introduces resources and allows to make requests to individual URIs (still all typically POST) for separate actions instead of exposing one universal endpoint (API). The API resources are still generalized but it is possible to identify the scope of each one.
Level One design is not RESTful, yet it is organizing the API in the direction of becoming one.

#### Level 2 : HTTP verbs

The system starts making use of HTTP Verbs. This allows to further specialize the resource and thus narrow down the functionality of each individual operation with the service. The principle separation at this level consists in splitting a given resource into two — one request for obtaining data only (GET), the other for modifying the data (POST). Further granularization is also possible. GET requests only fetch data, POST/PUT calls introduce new and update existing data, DELETE requests remove or otherwise invalidate previous actions. One drawback of providing a distributed service with more than GET and POST per resource might be growing complication of using such a system.

#### Level 3 : Hypermedia controls

The last level introduces the hypermedia representation. Also called HATEOAS (Hypermedia As The Engine of Application State), these are elements embedded in the response messages of resources which allow to establish a relation between individual data entities returned from and pass to the APIs. For instance, a GET request to a hotel reservation system might return a number of available rooms along with hypermedia links (these would be html hyperlink controls in the early days of the model) allowing to book specific rooms.

### What is HATEOAS? Why do we need it?

The term **HATEOAS** stands for the phrase **H**ypermedia **A**s **T**he **E**ngine **O**f **A**pplication **S**tate. To understand this further, we first need to understand the meaning of Hypermedia.

Typically, when we perform a REST request, we only get the data and not any actions around it. This is where HATEOAS comes in the fill in the gap. A HATEOAS request allows you to not only send the data but also specify the related actions.

The single most important reason for HATEOAS is **loose coupling**. If a consumer of a REST service needs to hard-code all the resource URLs, then it is tightly coupled with your service implementation. Instead, if you return the URLs, it could use for the actions, then it is loosely coupled. There is no tight dependency on the URI structure, as it is specified and used from the response.

### What are the basic principles and alternatives for Rest Assured?

Rest assured is java library **for testing Restful Web services**. It can be used to test XML & JSON based web services. It supports GET, POST, PUT, PATCH, DELETE, OPTIONS and HEAD requests and can be used to validate and verify the response of these requests.

As for alternatives we can do the tests manually using Java's http support or use Spring's `MockMvc` but it doesn't allow for testing all the real network connection since it forwards calls directly to `DispathcerServlet` that is wrapped inside of the mock.

### What type of testing is done with Rest Assured?

Integration test.

## JPA

[https://www.baeldung.com/learn-jpa-hibernate](https://www.baeldung.com/learn-jpa-hibernate)

### What is the Java Persistence API?

As a specification, the [Java Persistence API](https://jcp.org/en/jsr/detail?id=338) is concerned with *persistence*, which loosely means any mechanism by which Java objects outlive the application process that created them. Not all Java objects need to be persisted, but most applications persist key business objects. The JPA specification lets you define which objects should be persisted, and how **those objects should be persisted in your Java applications.

By itself, JPA is not a tool or framework; rather, it defines a set of concepts that can be implemented by any tool or framework. While JPA's object-relational mapping (ORM) model was originally based on [Hibernate](http://hibernate.org/orm/), it has since evolved. Likewise, while JPA was originally intended for use with relational/SQL databases, some JPA implementations have been extended for use with NoSQL datastores.

### What is the object-relational mapping?

**Object-relational mapping (ORM)** is a technique that creates a layer between the language and the database, helping programmers work with data without the OOP paradigm.

### What are the advantages and disadvantages of JPA?

#### Advantages of JPA

JPA is standards-based. A growing number of providers are looking to provide JPA implementations in the near future.

It provides the best features in Hibernate and TopLink.

It can be used with Java SE and Java EE applications, using EJB containers, or not.

#### Drawbacks of JPA

- JPA is a specification rather than a product. You need a provider to provide an implementation to gain the benefits of these standards-based APIs.
- Composite Key: This, in my opinion, is the biggest headache of the JPA developers. When we map a composite key we are adding a huge complexity to the project when we need to persist or find a object in the database. When you use composite key several problems will might happen, and some of these problems could be implementation bugs.
- Legacy Database: A project that has a lot of business rules in the database can be a problem when wee need to invoke StoredProcedures or Functions.
- Artifact size: The artifact size will increase a lot if you are using the Hibernate implementation. The Hibernate uses a lot of dependencies that will increase the size of the generated jar/war/ear. The artifact size can be a problem if the developer needs to do a deploy in several remote servers with a low Internet band (or a slow upload). Imagine a project that in each new release it is necessary to update 10 customers servers across the country. Problems with slow upload, corrupted file and loss of Internet can happen making the dev/ops team to lose more time.
- Generated SQL: One of the JPA advantages is the database portability, but to use this portability advantage you need to use the JPQL/HQL *language*. This advantage can became a disadvantage when the generated query has a poor performance and it does not use the table index that was created to optimize the queries.
- Complex Query: That are projects that has several queries with a high level of complexity using database resources like: SUM, MAX, MIN, COUNT, HAVING, etc. If you combine those resources the JPA performance might drop and not use the table indexes, or you will not be able to use a specific database resource that could solve this problem.
- Framework complexity: To create a CRUD with JPA is very simples, but problems will appear when we start to use entities relationships, inheritance, cache, PersistenceUnit manipulation, PersistenceContext with several entities, etc. A development team without a developer with a good JPA experience will lose a lot of time with JPA ‘*rules*‘.
- Slow processing and a lot of RAM memory occupied: There are moments that JPA will lose performance at report processing, inserting a lot of entities or problems with a transaction that is opened for a long time.

### What id generation strategies can you name? Can you describe their pros and cons?

#### AUTO Generation

If we're using the default generation type, the persistence provider will determine values based on the type of the primary key attribute. This type can be numerical or UUID.

For numeric values, the generation is based on a sequence or table generator, while UUID values will use the UUIDGenerator.

#### IDENTITY Generation

This type of generation relies on the IdentityGenerator which expects values generated by an identity column in the database, meaning they are auto-incremented.
One thing to note is that IDENTITY generation disables batch updates.

#### SEQUENCE Generation

To use a sequence-based id, Hibernate provides the SequenceStyleGenerator class.

This generator uses sequences if they're supported by our database, and switches to table generation if they aren't.

To customize the sequence name, we can use the @GenericGenerator annotation with SequenceStyleGenerator strategy.

SEQUENCE is the generation type recommended by the Hibernate documentation.

The generated values are unique per sequence. If you don't specify a sequence name, Hibernate will re-use the same hibernate_sequence for different types.

#### TABLE Generation

The TableGenerator uses an underlying database table that holds segments of identifier generation values.

The disadvantage of this method is that it doesn't scale well and can negatively affect performance.

To sum up, these four generation types will result in similar values being generated but use different database mechanisms.

### What JPA inheritance types can you name? Can you describe them?

#### `@MappedSuperclass`

Using the `@MappedSuperclass` strategy, inheritance is only evident in the class, but not the entity model. If we're using this strategy, ancestors cannot contain associations with other entities.

Single Table

The Single Table strategy creates one table for each class hierarchy. This is also the default strategy chosen by JPA if we don't specify one explicitly. The identifier of the entities is defined in the super-class.

#### Joined Table

Using this strategy, each class in the hierarchy is mapped to its table. The only column which repeatedly appears in all the tables is the identifier, which will be used for joining them when needed. The disadvantage of this inheritance mapping method is that retrieving entities requires joins between tables, which can result in lower perform``ance for large numbers of records.

#### Table per Class

The Table Per Class strategy maps each entity to its table which contains all the properties of the entity, including the ones inherited. The resulting schema is similar to the one using `@MappedSuperclass`, but unlike it, table per class will indeed define entities for parent classes, allowing associations and polymorphic queries as a result.

### What fetch strategies do you know? What is the default? How to reproduce `LazyInitializationException`?

The FetchType method defines two strategies for fetching data from the database:

- `FetchType.EAGER`. The persistence provider must load the related annotated field or property. This is the default behavior for `@Basic`, `@ManyToOne`, and `@OneToOne` annotated fields.
- `FetchType.LAZY`. The persistence provider should load data when it is first accessed, but can be loaded eagerly. This is the default behavior for `@OneToMany`, `@ManyToMany` and `@ElementCollection`annotated fields.

#### `LazyInitializationException`

Hibernate throws the `LazyInitializationException` when it needs to initialize a lazily fetched association to another entity without an active session context. That’s usually the case if you try to use an uninitialized association in your client application or web layer.

```java
EntityManager em = emf.createEntityManager();
em.getTransaction().begin();

TypedQuery<Author> q = em.createQuery(
        "SELECT a FROM Author a",
        Author.class);
List<Author> authors = q.getResultList();
em.getTransaction().commit();
em.close();

for (Author author : authors) {
  List<Book> books = author.getBooks();
  log.info("... the next line will throw LazyInitializationException ...");
  books.size();
}
```

The database query returns an `Author` entity with a lazily fetched association to the books this author has written. Hibernate initializes the books attributes with its own List implementation, which handles the lazy loading. When you try to access an element in that List or call a method that operates on its elements, Hibernate’s List implementation recognizes that no active session is available and throws a `LazyInitializationException`.

### What cascade types do you know? Is it possible to insert an entity using cascade?

#### ALL

`CascadeType.ALL` propagates all operations — including Hibernate-specific ones — from a parent to a child entity.

#### PERSIST

The persist operation makes a transient instance persistent. `CascadeType.PERSIST` propagates the persist operation from a parent to a child entity. When we save the parent entity, the related entity will also get saved.

#### MERGE

The merge operation copies the state of the given object onto the persistent object with the same identifier. `CascadeType.MERGE` propagates the merge operation from a parent to a child entity.

#### REMOVE

As the name suggests, the remove operation removes the row corresponding to the entity from the database and also from the persistent context.

`CascadeType.REMOVE` propagates the remove operation from parent to child entity. Similar to JPA's `CascadeType.REMOVE`, we have `CascadeType.DELETE`, which is specific to Hibernate. There is no difference between the two.

#### REFRESH

Refresh operations **reread the value of a given instance from the database.** In some cases, we may change an instance after persisting in the database, but later we need to undo those changes.

In that kind of scenario, this may be useful. When we use this operation with `CascadeType.REFRESH`, the child entity also gets reloaded from the database whenever the parent entity is refreshed.

Example:

```java
@Test
public void whenParentRefreshedThenChildRefreshed() {
    Person person = buildPerson("devender");
    Address address = buildAddress(person);
    person.setAddresses(Arrays.asList(address));
    session.persist(person);
    session.flush();
    person.setName("Devender Kumar");
    address.setHouseNumber(24);
    session.refresh(person);
    
    assertThat(person.getName()).isEqualTo("devender");
    assertThat(address.getHouseNumber()).isEqualTo(23);
}
```

#### DETACH

The detach operation removes the entity from the persistent context. When we use `CascadeType.DETACH`, the child entity will also get removed from the persistent context.

### When do we need property `orphalRemoval`?

```java
@Entity
public class OrderRequest {

    @Id
    @GeneratedValue(strategy = GenerationType.AUTO)
    private Long id;

    @OneToOne(cascade = { CascadeType.REMOVE, CascadeType.PERSIST })
    private ShipmentInfo shipmentInfo;

    @OneToMany(orphanRemoval = true, cascade = CascadeType.PERSIST, mappedBy = "orderRequest")
    private List<LineItem> lineItems;

		...

    public void removeLineItem(LineItem lineItem) {
        lineItems.remove(lineItem);
    }
}
```

`orphalRemoval` usage is to **delete orphaned entities from the database**. An entity that is no longer attached to its parent is the definition of being an orphan.

For example, an `OrderRequest` has a collection of `LineItem` **objects where **we use the `@OneToMany` **annotation to identify the relationship*.* This is where we also set the `orphanRemoval` **attribute to `true`. To detach a `LineItem` from an `OrderRequest`, we can use the `removeLineItem` method that we previously created.

### What entity states do you know?

#### 1. New (Transient) state

An object that is newly created and has never been associated with JPA *Persistence Context* (hibernate session) is considered to be in the **New (Transient)** state. The data of objects in this state is not stored in the database.

#### 2. Persistent (JPA managed) state

An Object that is associated with *persistence context* (hibernate session) is in Persistent state. Any changes made to objects in this state are automatically propagated to databases without manually invoking `persist`/`merge`/`remove`.

#### 3. Detached (unmanaged) state

An Object becomes detached when the currently running *Persistence Context* is closed. Any changes made to detached objects are no longer automatically propagated to the database.

#### 4. Removed object state

As the name suggests, removed objects are deleted from the database. JPA provides `entityManager.remove(object);`method to remove an entity from the database.

### What is the difference between methods `find()` and `getReference()`?

`find()` is the most common method of fetching entities:

```java
Game game = entityManager.find(Game.class, 1L);
```

This method initializes the entity when we request it.

Similar to the `find()` method, `getReference()` is also another way to retrieve entities:

```java
Game game = entityManager.getReference(Game.class, 1L);
```

However, the object returned is **an entity proxy that only has the primary key field initialized**. The other fields remain unset unless we lazily request them.

Example use cases for `getReference()` would be updating entity fields, deleting entities, updating entity relations because these operations do not require reading the fileds with `SELECT` query, bu `find()` does that anyways.

### What query types do you know? Can you name their pros and cons?

#### `Query`, written in Java Persistence Query Language (JPQL) syntax

A Query is similar in syntax to SQL, and it's generally used to perform CRUD operations:

```java
public UserEntitygetUserByIdWithPlainQuery(Long id) {
    Query jpqlQuery = getEntityManager().createQuery("SELECT u FROM UserEntity u WHERE u.id=:id");
    jpqlQuery.setParameter("id", id);
		return (UserEntity) jpqlQuery.getSingleResult();
}
```

This `Query` retrieves the matching record from the *users* table and also maps it to the `UserEntity` object.

**Pros:**

- When we create a query using *EntityManager*, we can build dynamic query strings
- Queries are written in JPQL, so they're portable

**Cons:**

- For a dynamic query, it may be compiled into a native SQL statement multiple times depending on the **[query plan cache](https://www.baeldung.com/hibernate-query-plan-cache)**
- The queries may scatter into various Java classes and they're mixed in with Java code. Therefore, it could be difficult to maintain if a project contains many queries

#### `TypedQuery`

We need to pay attention to the *return* statement in our previous example. JPA can't deduce what the `Query` result type will be, and, as a result, we have to cast.

**JPA provides a special `Query` sub-type known as a `TypedQuery<T>`*.*** This is always preferred if we know our `Query` **result type beforehand. Additionally, it makes our code much more reliable and easier to test.

Let's see a `TypedQuery<T>` alternative, compared to our first example:

```java
public UserEntitygetUserByIdWithTypedQuery(Long id) {
	  TypedQuery<UserEntity> typedQuery = getEntityManager().createQuery("SELECT u FROM UserEntity u WHERE u.id=:id", UserEntity.class);
    typedQuery.setParameter("id", id);
		return typedQuery.getSingleResult();
}
```

This way, **we get stronger typing for free,** avoiding possible casting exceptions down the road.

#### `NamedQuery`

While we can dynamically define a `Query` on specific methods, they can eventually grow into a hard-to-maintain codebase. What if we could keep general usage queries in one centralized, easy-to-read place?

JPA's also got us covered on this with another *Query* sub-type known as a `NamedQuery`. We can define named queries in `orm.xml` or a properties file.

Also, we can define `NamedQuery` on the `Entity` class itself, providing a centralized, quick and easy way to read and find an `Entity`‘s related queries.

All `NamedQueries` must have a unique name.

Let's see how we can add a `NamedQuery` to our `UserEntity` class:

```java
@Table(name = "users")
@Entity
@NamedQuery(name = "UserEntity.findByUserId", query = "SELECT u FROM UserEntity u WHERE u.id=:userId")
publicclassUserEntity {

    @Id
		private Long id;
		private String name;
    
		...
}
```

Using a `NamedQuery` is very simple:

```java
public UserEntitygetUserByIdWithNamedQuery(Long id) {
    Query namedQuery = getEntityManager().createNamedQuery("UserEntity.findByUserId");
    namedQuery.setParameter("userId", id);
		return (UserEntity) namedQuery.getSingleResult();
}
```

**Pros:**

- `NamedQueries` are compiled and validated when the persistence unit is loaded. That is to say, they're compiled only once
- We can centralize `NamedQueries` to make them easier to maintain – for example, in `orm.xml`, in properties files, or on `@Entity` classes

**Cons:**

- `NamedQueries` are always static
- `NamedQueries` can be referenced in Spring Data JPA repositories. However, **[dynamic sorting](https://www.baeldung.com/spring-data-sorting#sorting-with-spring-data)** is not supported

#### `NativeQuery`, written in plain SQL syntax

A `NativeQuery` is simply an SQL query. These allow us to unleash the full power of our database, as we can use proprietary features not available in JPQL-restricted syntax.

This comes at a cost. We lose the database portability of our application with *NativeQuery* because our JPA provider can't abstract specific details from the database implementation or vendor anymore.

```java
public UserEntitygetUserByIdWithNativeQuery(Long id) {
    Query nativeQuery = getEntityManager().createNativeQuery("SELECT * FROM users WHERE id=:userId", UserEntity.class);
    nativeQuery.setParameter("userId", id);
		return (UserEntity) nativeQuery.getSingleResult();
}
```

We must always consider if a `NativeQuery` is the only option. Most of the time, a good JPQL `Query` can fulfill our needs and most importantly, maintain a level of abstraction from the actual database implementation.

Using `NativeQuery` doesn't necessarily mean locking ourselves to one specific database vendor. After all, if our queries don't use proprietary SQL commands and are using only a standard SQL syntax, switching providers should not be an issue.

**Pros:**

- As our queries get complex, sometimes the JPA-generated SQL statements aren't the most optimized. In this case, we can use `NativeQueries` to make the queries more efficient
- `NativeQueries` allow us to use database vendor-specific features. Sometimes, those features can give our queries better performance

**Cons:**

- Vendor-specific features can bring convenience and better performance, but we pay for that benefit by losing the portability from one database to another

#### Criteria API `CriteriaQuery<T>`, constructed programmatically via different methods

[Criteria API queries](https://www.baeldung.com/hibernate-criteria-queries) are programmatically-built, type-safe queries – somewhat similar to JPQL queries in syntax:

```java
public UserEntitygetUserByIdWithCriteriaQuery(Long id) {
    CriteriaBuilder criteriaBuilder = getEntityManager().getCriteriaBuilder();
    CriteriaQuery<UserEntity> criteriaQuery = criteriaBuilder.createQuery(UserEntity.class);
    Root<UserEntity> userRoot = criteriaQuery.from(UserEntity.class);
		UserEntity queryResult = 
					getEntityManager().createQuery(criteriaQuery.select(userRoot)
					      .where(criteriaBuilder.equal(userRoot.get("id"), id)))
					      .getSingleResult();
		return queryResult;
}
```

It can be daunting to use *Criteria* API queries first-hand, but they can be a great choice when we need to add dynamic query elements or when coupled with the [JPA *Metamodel*.](https://www.baeldung.com/hibernate-criteria-queries-metamodel)

### Can we update an entity without calling `merge()` method?

JPA and Hibernate make it very easy to update a managed entity. If your entity is in the lifecycle state *managed*, e.g. because you fetched it with a [JPQL query](https://thorben-janssen.com/jpql/) or the *find* method of the *EntityManager*, you just need to change the values of your entity attributes.

```java
em = efm.createEntityManager();
em.getTrasaction().begin();
a = em.find(Author.class, a.getId());
a.setFirstName("Thorben");
log.info("Before commit");
em.getTransaction().commit();
em.close();
```

When Hibernate decides to flush the persistence context, the dirty checking mechanism will detect the change and perform the required SQL UPDATE statement.