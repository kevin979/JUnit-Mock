---
description: 常用註解
---

# Annotation

Junit提供的註解非常的多，但絕大多數的註解是需要完整測試劇本才有其作用，片段式的單元測試其實用的到註解並不多，此處也僅介紹常用的註解。

* @BeforeAll -> 測試啟動後，只執行一次且其Method必須為static，此註解的執行時間點在於Spring boot tomcat啟動前，如果有類別是在Spring boot tomcat啟動後才實體化的，那就不能放入此Method中進行操作，否則會出現錯誤。

```java
private static String name;

@BeforeAll
static void beforeTest() {
    name = "TEST";
    System.out.println("BeforeAll");
}

@Test
void test() {
    System.out.println(name);
}
```

```java
//執行結果
> Task :compileJava UP-TO-DATE
> Task :processResources UP-TO-DATE
> Task :classes UP-TO-DATE
> Task :compileTestJava
> Task :processTestResources NO-SOURCE
> Task :testClasses
Connected to the target VM, address: 'localhost:65305', transport: 'socket'
> Task :test
14:15:04.848 [Test worker] DEBUG org.springframework.test.context.BootstrapUtils - Instantiating CacheAwareContextLoaderDelegate from class [org.springframework.test.context.cache.DefaultCacheAwareContextLoaderDelegate]
14:15:04.866 [Test worker] DEBUG org.springframework.test.context.BootstrapUtils - Instantiating BootstrapContext using constructor [public org.springframework.test.context.support.DefaultBootstrapContext(java.lang.Class,org.springframework.test.context.CacheAwareContextLoaderDelegate)]
14:15:04.913 [Test worker] DEBUG org.springframework.test.context.BootstrapUtils - Instantiating TestContextBootstrapper for test class [com.example.jpa.test.JUnitExample] from class [org.springframework.boot.test.context.SpringBootTestContextBootstrapper]
14:15:04.934 [Test worker] INFO org.springframework.boot.test.context.SpringBootTestContextBootstrapper - Neither @ContextConfiguration nor @ContextHierarchy found for test class [com.example.jpa.test.JUnitExample], using SpringBootContextLoader
14:15:04.940 [Test worker] DEBUG org.springframework.test.context.support.AbstractContextLoader - Did not detect default resource location for test class [com.example.jpa.test.JUnitExample]: class path resource [com/example/jpa/test/JUnitExample-context.xml] does not exist
14:15:04.940 [Test worker] DEBUG org.springframework.test.context.support.AbstractContextLoader - Did not detect default resource location for test class [com.example.jpa.test.JUnitExample]: class path resource [com/example/jpa/test/JUnitExampleContext.groovy] does not exist
14:15:04.941 [Test worker] INFO org.springframework.test.context.support.AbstractContextLoader - Could not detect default resource locations for test class [com.example.jpa.test.JUnitExample]: no resource found for suffixes {-context.xml, Context.groovy}.
14:15:04.941 [Test worker] INFO org.springframework.test.context.support.AnnotationConfigContextLoaderUtils - Could not detect default configuration classes for test class [com.example.jpa.test.JUnitExample]: JUnitExample does not declare any static, non-private, non-final, nested classes annotated with @Configuration.
14:15:05.006 [Test worker] DEBUG org.springframework.test.context.support.ActiveProfilesUtils - Could not find an 'annotation declaring class' for annotation type [org.springframework.test.context.ActiveProfiles] and class [com.example.jpa.test.JUnitExample]
14:15:05.106 [Test worker] DEBUG org.springframework.context.annotation.ClassPathScanningCandidateComponentProvider - Identified candidate component class: file [D:\workspace\jpa\build\classes\java\main\com\example\jpa\JpaApplication.class]
14:15:05.107 [Test worker] INFO org.springframework.boot.test.context.SpringBootTestContextBootstrapper - Found @SpringBootConfiguration com.example.jpa.JpaApplication for test class com.example.jpa.test.JUnitExample
14:15:05.285 [Test worker] DEBUG org.springframework.boot.test.context.SpringBootTestContextBootstrapper - @TestExecutionListeners is not present for class [com.example.jpa.test.JUnitExample]: using defaults.
14:15:05.286 [Test worker] INFO org.springframework.boot.test.context.SpringBootTestContextBootstrapper - Loaded default TestExecutionListener class names from location [META-INF/spring.factories]: [org.springframework.boot.test.autoconfigure.restdocs.RestDocsTestExecutionListener, org.springframework.boot.test.autoconfigure.web.client.MockRestServiceServerResetTestExecutionListener, org.springframework.boot.test.autoconfigure.web.servlet.MockMvcPrintOnlyOnFailureTestExecutionListener, org.springframework.boot.test.autoconfigure.web.servlet.WebDriverTestExecutionListener, org.springframework.boot.test.autoconfigure.webservices.client.MockWebServiceServerTestExecutionListener, org.springframework.boot.test.mock.mockito.MockitoTestExecutionListener, org.springframework.boot.test.mock.mockito.ResetMocksTestExecutionListener, org.springframework.test.context.web.ServletTestExecutionListener, org.springframework.test.context.support.DirtiesContextBeforeModesTestExecutionListener, org.springframework.test.context.event.ApplicationEventsTestExecutionListener, org.springframework.test.context.support.DependencyInjectionTestExecutionListener, org.springframework.test.context.support.DirtiesContextTestExecutionListener, org.springframework.test.context.transaction.TransactionalTestExecutionListener, org.springframework.test.context.jdbc.SqlScriptsTestExecutionListener, org.springframework.test.context.event.EventPublishingTestExecutionListener]
14:15:05.299 [Test worker] DEBUG org.springframework.boot.test.context.SpringBootTestContextBootstrapper - Skipping candidate TestExecutionListener [org.springframework.test.context.web.ServletTestExecutionListener] due to a missing dependency. Specify custom listener classes or make the default listener classes and their required dependencies available. Offending class: [javax/servlet/ServletContext]
14:15:05.315 [Test worker] INFO org.springframework.boot.test.context.SpringBootTestContextBootstrapper - Using TestExecutionListeners: [org.springframework.test.context.support.DirtiesContextBeforeModesTestExecutionListener@7dd712e8, org.springframework.test.context.event.ApplicationEventsTestExecutionListener@2c282004, org.springframework.boot.test.mock.mockito.MockitoTestExecutionListener@22ee2d0, org.springframework.boot.test.autoconfigure.SpringBootDependencyInjectionTestExecutionListener@7bfc3126, org.springframework.test.context.support.DirtiesContextTestExecutionListener@3e792ce3, org.springframework.test.context.transaction.TransactionalTestExecutionListener@53bc1328, org.springframework.test.context.jdbc.SqlScriptsTestExecutionListener@26f143ed, org.springframework.test.context.event.EventPublishingTestExecutionListener@3c1e3314, org.springframework.boot.test.autoconfigure.restdocs.RestDocsTestExecutionListener@4b770e40, org.springframework.boot.test.autoconfigure.web.client.MockRestServiceServerResetTestExecutionListener@78e16155, org.springframework.boot.test.autoconfigure.web.servlet.MockMvcPrintOnlyOnFailureTestExecutionListener@54a3ab8f, org.springframework.boot.test.autoconfigure.web.servlet.WebDriverTestExecutionListener@1968a49c, org.springframework.boot.test.autoconfigure.webservices.client.MockWebServiceServerTestExecutionListener@6a1ebcff, org.springframework.boot.test.mock.mockito.ResetMocksTestExecutionListener@19868320]
14:15:05.320 [Test worker] DEBUG org.springframework.test.context.support.AbstractDirtiesContextTestExecutionListener - Before test class: context [DefaultTestContext@6bd51ed8 testClass = JUnitExample, testInstance = [null], testMethod = [null], testException = [null], mergedContextConfiguration = [MergedContextConfiguration@61e3a1fd testClass = JUnitExample, locations = '{}', classes = '{class com.example.jpa.JpaApplication}', contextInitializerClasses = '[]', activeProfiles = '{}', propertySourceLocations = '{}', propertySourceProperties = '{org.springframework.boot.test.context.SpringBootTestContextBootstrapper=true}', contextCustomizers = set[org.springframework.boot.test.autoconfigure.actuate.metrics.MetricsExportContextCustomizerFactory$DisableMetricExportContextCustomizer@2e61d218, org.springframework.boot.test.autoconfigure.properties.PropertyMappingContextCustomizer@0, org.springframework.boot.test.autoconfigure.web.servlet.WebDriverContextCustomizerFactory$Customizer@6c5945a7, org.springframework.boot.test.context.filter.ExcludeFilterContextCustomizer@78365cfa, org.springframework.boot.test.json.DuplicateJsonObjectContextCustomizerFactory$DuplicateJsonObjectContextCustomizer@52de51b6, org.springframework.boot.test.mock.mockito.MockitoContextCustomizer@0, org.springframework.boot.test.web.client.TestRestTemplateContextCustomizer@c5ee75e, org.springframework.boot.test.context.SpringBootTestArgs@1, org.springframework.boot.test.context.SpringBootTestWebEnvironment@387a8303], contextLoader = 'org.springframework.boot.test.context.SpringBootContextLoader', parent = [null]], attributes = map[[empty]]], class annotated with @DirtiesContext [false] with mode [null].
BeforeAll
14:15:05.346 [Test worker] DEBUG org.springframework.test.context.support.DependencyInjectionTestExecutionListener - Performing dependency injection for test context [[DefaultTestContext@6bd51ed8 testClass = JUnitExample, testInstance = com.example.jpa.test.JUnitExample@282308c3, testMethod = [null], testException = [null], mergedContextConfiguration = [MergedContextConfiguration@61e3a1fd testClass = JUnitExample, locations = '{}', classes = '{class com.example.jpa.JpaApplication}', contextInitializerClasses = '[]', activeProfiles = '{}', propertySourceLocations = '{}', propertySourceProperties = '{org.springframework.boot.test.context.SpringBootTestContextBootstrapper=true}', contextCustomizers = set[org.springframework.boot.test.autoconfigure.actuate.metrics.MetricsExportContextCustomizerFactory$DisableMetricExportContextCustomizer@2e61d218, org.springframework.boot.test.autoconfigure.properties.PropertyMappingContextCustomizer@0, org.springframework.boot.test.autoconfigure.web.servlet.WebDriverContextCustomizerFactory$Customizer@6c5945a7, org.springframework.boot.test.context.filter.ExcludeFilterContextCustomizer@78365cfa, org.springframework.boot.test.json.DuplicateJsonObjectContextCustomizerFactory$DuplicateJsonObjectContextCustomizer@52de51b6, org.springframework.boot.test.mock.mockito.MockitoContextCustomizer@0, org.springframework.boot.test.web.client.TestRestTemplateContextCustomizer@c5ee75e, org.springframework.boot.test.context.SpringBootTestArgs@1, org.springframework.boot.test.context.SpringBootTestWebEnvironment@387a8303], contextLoader = 'org.springframework.boot.test.context.SpringBootContextLoader', parent = [null]], attributes = map['org.springframework.test.context.event.ApplicationEventsTestExecutionListener.recordApplicationEvents' -> false]]].

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::               (v2.6.13)

2024-01-31 14:15:05.846  INFO 23808 --- [    Test worker] com.example.jpa.test.JUnitExample        : Starting JUnitExample using Java 1.8.0_212-1-ojdkbuild on DESKTOP-439QHH7 with PID 23808 (started by user in D:\workspace\jpa)
2024-01-31 14:15:05.847  INFO 23808 --- [    Test worker] com.example.jpa.test.JUnitExample        : No active profile set, falling back to 1 default profile: "default"
2024-01-31 14:15:06.367  INFO 23808 --- [    Test worker] .s.d.r.c.RepositoryConfigurationDelegate : Bootstrapping Spring Data JPA repositories in DEFAULT mode.
2024-01-31 14:15:06.480  INFO 23808 --- [    Test worker] .s.d.r.c.RepositoryConfigurationDelegate : Finished Spring Data repository scanning in 96 ms. Found 2 JPA repository interfaces.
2024-01-31 14:15:07.366  INFO 23808 --- [    Test worker] o.hibernate.jpa.internal.util.LogHelper  : HHH000204: Processing PersistenceUnitInfo [name: default]
2024-01-31 14:15:07.479  INFO 23808 --- [    Test worker] org.hibernate.Version                    : HHH000412: Hibernate ORM core version 5.6.12.Final
2024-01-31 14:15:07.793  INFO 23808 --- [    Test worker] o.hibernate.annotations.common.Version   : HCANN000001: Hibernate Commons Annotations {5.1.2.Final}
2024-01-31 14:15:08.079  INFO 23808 --- [    Test worker] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Starting...
2024-01-31 14:15:08.466  INFO 23808 --- [    Test worker] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Start completed.
2024-01-31 14:15:08.502  INFO 23808 --- [    Test worker] org.hibernate.dialect.Dialect            : HHH000400: Using dialect: org.hibernate.dialect.OracleDialect
2024-01-31 14:15:08.511  WARN 23808 --- [    Test worker] org.hibernate.dialect.Oracle9Dialect     : HHH000063: The Oracle9Dialect dialect has been deprecated; use either Oracle9iDialect or Oracle10gDialect instead
2024-01-31 14:15:08.513  WARN 23808 --- [    Test worker] org.hibernate.dialect.OracleDialect      : HHH000064: The OracleDialect dialect has been deprecated; use Oracle8iDialect instead
2024-01-31 14:15:09.453  INFO 23808 --- [    Test worker] o.h.e.t.j.p.i.JtaPlatformInitiator       : HHH000490: Using JtaPlatform implementation: [org.hibernate.engine.transaction.jta.platform.internal.NoJtaPlatform]
2024-01-31 14:15:09.595  INFO 23808 --- [    Test worker] j.LocalContainerEntityManagerFactoryBean : Initialized JPA EntityManagerFactory for persistence unit 'default'
2024-01-31 14:15:09.661  INFO 23808 --- [    Test worker] org.hibernate.dialect.Dialect            : HHH000400: Using dialect: org.hibernate.dialect.OracleDialect
2024-01-31 14:15:09.661  WARN 23808 --- [    Test worker] org.hibernate.dialect.Oracle9Dialect     : HHH000063: The Oracle9Dialect dialect has been deprecated; use either Oracle9iDialect or Oracle10gDialect instead
2024-01-31 14:15:09.661  WARN 23808 --- [    Test worker] org.hibernate.dialect.OracleDialect      : HHH000064: The OracleDialect dialect has been deprecated; use Oracle8iDialect instead
2024-01-31 14:15:09.701  INFO 23808 --- [    Test worker] o.h.e.t.j.p.i.JtaPlatformInitiator       : HHH000490: Using JtaPlatform implementation: [org.hibernate.engine.transaction.jta.platform.internal.NoJtaPlatform]
2024-01-31 14:15:10.510  INFO 23808 --- [    Test worker] com.example.jpa.test.JUnitExample        : Started JUnitExample in 5.123 seconds (JVM running for 7.336)
TEST
2024-01-31 14:15:10.643  INFO 23808 --- [ionShutdownHook] j.LocalContainerEntityManagerFactoryBean : Closing JPA EntityManagerFactory for persistence unit 'default'
2024-01-31 14:15:10.645  INFO 23808 --- [ionShutdownHook] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Shutdown initiated...
Disconnected from the target VM, address: 'localhost:65305', transport: 'socket'
2024-01-31 14:15:10.655  INFO 23808 --- [ionShutdownHook] com.zaxxer.hikari.HikariDataSource       : HikariPool-1 - Shutdown completed.
BUILD SUCCESSFUL in 8s
4 actionable tasks: 2 executed, 2 up-to-date
下午 02:15:10: Task execution finished ':test --tests "com.example.jpa.test.JUnitExample.test"'.
```

* @BeforeEach -> 一個類別會有多個測試Method，每次執行測試Method前都會執行，如果測試類別有10個測試Method，則BeforeEach 會執行10次。

```java
private static double random;

@BeforeEach
void beforeTest() {
    random = Math.random() * 10;
    System.out.println("BeforeEach");
}

@Test
void test1() {
    System.out.println(random);
}

@Test
void test2() {
    System.out.println(random);
}

@Test
void test3() {
    System.out.println(random);
}

@Test
void test4() {
    System.out.println(random);
}

@Test
void test5() {
    System.out.println(random);
}
```

```
// 測試結果
BeforeEach
8.595271474430671
BeforeEach
4.86336018579757
BeforeEach
4.081459081930232
BeforeEach
1.7680885052569795
BeforeEach
2.895808532936668
```

* @Test -> 測試的Method
* @AfterEach -> 與BeforeEach相反，是在每次測試結束都會執行的

```java
private static double random;

@BeforeEach
void beforeTest() {
    random = Math.random() * 10;
    System.out.println("BeforeEach");
}

@Test
void test1() {
    System.out.println(random);
}

@Test
void test2() {
    System.out.println(random);
}

@Test
void test3() {
    System.out.println(random);
}

@Test
void test4() {
    System.out.println(random);
}

@Test
void test5() {
    System.out.println(random);
}

@AfterEach
void afterTest() {
    System.out.println("AfterEach");
}
```

```
// 執行結果
BeforeEach
5.652479716443688
AfterEach
BeforeEach
7.9988811994014295
AfterEach
BeforeEach
5.96406849728603
AfterEach
BeforeEach
5.7326976478298
AfterEach
BeforeEach
8.669542531191864
AfterEach
```

* @AfterAll -> 與BeforeAll相反，是在完成測試後執行，執行時間點是Spring boot tomcat結束後，如果有使用到Spring boot創建的實例或API會無法在此執行。
* @Disabled -> 忽略此測試

```java
@BeforeEach
void beforeTest() {
    random = Math.random() * 10;
}

@Test
void test1() {
    System.out.println("test1 " + random);
}

@Test
@Disabled
void test2() {
    System.out.println("test2 " + random);
}

@Test
void test3() {
    System.out.println("test3 " + random);
}
```

```
// 測試結果
test1 0.7596629014667788
test3 8.353693387860018
```

* @RepeatedTest -> 重複測試(次數)

```java
private static double random;

@BeforeEach
void beforeTest() {
    random = Math.random() * 10;
}

@RepeatedTest(5)
void test() {
    System.out.println("test " + random);
}
```

```
// 測試結果
test 3.5294566756765064
test 1.4711828402301308
test 1.6154928015357228
test 6.064142652662073
test 0.09481082507417082
```
