# 1. အရင်ဆုံး Spring Boot ဆိုတာဘာလဲ?

**Spring Boot** က Java application တွေ၊ အထူးသဖြင့် backend REST API / microservices တွေကို လွယ်လွယ်ကူကူ develop, configure, run လုပ်နိုင်အောင် Spring Framework ပေါ်မှာ တည်ဆောက်ထားတဲ့ framework ဖြစ်ပါတယ်။

ဥပမာ—

```text
Client
   ↓
HTTP Request
   ↓
Spring Boot Application
   ↓
Controller
   ↓
Service
   ↓
Repository / MyBatis
   ↓
Database
   ↓
Response
```

Spring Boot ရဲ့ အဓိကအလုပ်တွေက—

* Object တွေကို manage လုပ်ပေးတယ်
* Dependency တွေကို inject လုပ်ပေးတယ်
* Configuration တွေကို auto configure လုပ်ပေးတယ်
* Web server run ပေးတယ်
* Database connection စီမံပေးတယ်
* Transaction စီမံပေးတယ်
* Security / AOP / Logging စတာတွေနဲ့ integrate လုပ်ရလွယ်တယ်

ဒါပေမယ့် **Spring Boot ရဲ့ heart က IoC Container** လို့ပြောလို့ရပါတယ်။

---

# 2. IoC ဆိုတာဘာလဲ?

## IoC = Inversion of Control

ပုံမှန် Java programming မှာ object တစ်ခုလိုရင် ကိုယ်တိုင် create လုပ်တယ်။

```java
CustomerService service = new CustomerService();
```

ဒီမှာ—

> **Object creation ကို developer က control လုပ်နေတယ်။**

Spring မှာတော့—

```java
@Service
public class CustomerService {
}
```

ဆိုပြီး Spring ကို object creation/control လွှဲပေးတယ်။

```text
Developer
    ↓
"Spring, ဒီ class ကို manage လုပ်ပေးပါ"
    ↓
Spring Container
    ↓
CustomerService object create
    ↓
Manage lifecycle
```

ဒါကို **Inversion of Control** လို့ခေါ်တယ်။

### ရိုးရိုးပြောရရင်

> **IoC ဆိုတာ Object တွေကို ဘယ်အချိန်မှာ create လုပ်မလဲ၊ ဘယ်လို manage လုပ်မလဲဆိုတဲ့ control ကို application code ကနေ Spring Container ဆီ လွှဲပေးတာပါ။**

---

# 3. IoC Container ဆိုတာဘာလဲ?

Spring ရဲ့ IoC Container က **objects တွေကို create, configure, inject, manage** လုပ်ပေးတဲ့ container ဖြစ်ပါတယ်။

အဓိက concept က—

```text
Spring IoC Container
        │
        ├── CustomerController
        ├── CustomerService
        ├── CustomerRepository
        ├── DataSource
        ├── TransactionManager
        └── Other Beans
```

ဒီ managed objects တွေကို Spring မှာ **Bean** လို့ခေါ်တယ်။

---

# 4. Bean ဆိုတာဘာလဲ?

**Bean = Spring Container က manage လုပ်ပေးတဲ့ Java object**

ဥပမာ—

```java
@Service
public class CustomerService {

}
```

Spring application start တဲ့အချိန်မှာ Spring က—

```text
CustomerService class
       ↓
create object
       ↓
CustomerService bean
       ↓
ApplicationContext ထဲမှာ manage
```

လုပ်ပေးတယ်။

အရေးကြီးတာက—

> Java object တိုင်းဟာ Bean မဟုတ်ဘူး။

ဥပမာ—

```java
Customer customer = new Customer();
```

ဒါက ordinary Java object ပဲ။

ဒါပေမယ့်—

```java
@Service
public class CustomerService {
}
```

Spring က create/manage လုပ်ပေးတဲ့ object ဖြစ်လို့ Bean ဖြစ်တယ်။

---

# 5. ApplicationContext ဆိုတာဘာလဲ?

Spring Boot application ရဲ့ အဓိက IoC Container ကို practical level မှာ **ApplicationContext** လို့တွေ့ရတတ်တယ်။

ဥပမာ—

```java
ApplicationContext context;
```

ဒီ Context ထဲမှာ Spring managed beans တွေရှိတယ်။

Conceptually—

```text
Spring Boot Application
        ↓
ApplicationContext
        ↓
Bean Registry
        ↓
┌──────────────────────┐
│ Controller Bean      │
│ Service Bean         │
│ Repository Bean      │
│ DataSource Bean      │
│ Transaction Bean     │
└──────────────────────┘
```

Spring ကလိုအပ်တဲ့ Bean ကို ဒီ Context ထဲကနေ ရှာပြီး dependency injection လုပ်ပေးတယ်။

---

# 6. DI ဆိုတာဘာလဲ?

## DI = Dependency Injection

IoC ကို implementation လုပ်တဲ့ အဓိကနည်းလမ်းတစ်ခုက **Dependency Injection** ပါ။

ဥပမာ—

`CustomerController` က `CustomerService` လိုတယ်။

ပုံမှန် Java ဆိုရင်—

```java
public class CustomerController {

    private CustomerService service =
            new CustomerService();
}
```

Controller ကိုယ်တိုင် Service object create လုပ်နေတယ်။

Spring မှာ—

```java
@RestController
public class CustomerController {

    private final CustomerService customerService;

    public CustomerController(CustomerService customerService) {
        this.customerService = customerService;
    }
}
```

Spring က—

```text
CustomerController needs CustomerService
              ↓
Spring Container
              ↓
Find CustomerService Bean
              ↓
Inject into CustomerController
```

လုပ်ပေးတယ်။

ဒါကို **Dependency Injection** လို့ခေါ်တယ်။

---

# 7. IoC နဲ့ DI ဘာကွာလဲ?

Interview မှာ မေးနိုင်ပါတယ်။

### IoC

**Concept / principle**

> Control ကို application ကနေ Spring Container ဆီ လွှဲပေးတာ။

### DI

**Implementation technique**

> လိုအပ်တဲ့ dependency ကို Spring က object ထဲ inject လုပ်ပေးတာ။

အလွယ်မှတ်—

```text
IoC = Principle
DI  = Way to implement IoC
```

Interview answer:

> **"IoC is the principle of transferring object creation and lifecycle management to the Spring container, while Dependency Injection is one of the main techniques Spring uses to provide required dependencies to objects."**

---

# 8. Dependency ဆိုတာဘာလဲ?

Class တစ်ခုက တခြား class တစ်ခုကို အသုံးပြုနေရင် အဲဒီ class က dependency ဖြစ်တယ်။

ဥပမာ—

```java
public class CustomerService {

    private final CustomerRepository repository;

    public CustomerService(CustomerRepository repository) {
        this.repository = repository;
    }
}
```

ဒီမှာ—

```text
CustomerService
       ↓ depends on
CustomerRepository
```

`CustomerRepository` က `CustomerService` ရဲ့ dependency ဖြစ်တယ်။

---

# 9. Spring က ဘယ်လိုသိတာလဲ?

Annotations တွေက အရေးကြီးတယ်။

ဥပမာ—

```java
@Component
@Service
@Repository
@Controller
@RestController
```

ဒီ annotations တွေက Spring ကို—

> "ဒီ class ကို Spring Bean အဖြစ် manage လုပ်ပါ"

လို့ပြောတာ။

---

# 10. `@Component`

Generic Spring Bean ဖြစ်တယ်။

```java
@Component
public class EmailUtil {
}
```

Spring က `EmailUtil` object ကို create/manage လုပ်ပေးမယ်။

---

# 11. `@Service`

Business logic အတွက် သုံးတယ်။

```java
@Service
public class CustomerService {
}
```

Technically `@Service` က Spring ရဲ့ component scanning အတွက် specialized `@Component` ဖြစ်တယ်။

---

# 12. `@Repository`

Database access layer အတွက် သုံးတယ်။

```java
@Repository
public class CustomerRepository {
}
```

---

# 13. `@Controller`

MVC controller အတွက်—

```java
@Controller
public class CustomerController {
}
```

---

# 14. `@RestController`

REST API အတွက် အများဆုံးသုံးတယ်။

```java
@RestController
@RequestMapping("/customers")
public class CustomerController {
}
```

`@RestController` က conceptually—

```text
@Controller
+
@ResponseBody
```

ဖြစ်တယ်။

ဒါကြောင့် return value ကို JSON/XML response body အနေနဲ့ ပြန်ပေးလို့ရတယ်။

---

# 15. Component Scanning ဆိုတာဘာလဲ?

Spring application start တဲ့အခါ Spring က—

> "ဘယ် classes တွေကို Bean အဖြစ် register လုပ်ရမလဲ?"

ရှာတယ်။

ဥပမာ main class—

```java
@SpringBootApplication
public class Application {

    public static void main(String[] args) {
        SpringApplication.run(Application.class, args);
    }
}
```

`@SpringBootApplication` ထဲမှာ အရေးကြီးတဲ့ concepts တွေပါပါတယ်။

```text
@SpringBootApplication
        │
        ├── @Configuration
        ├── @EnableAutoConfiguration
        └── @ComponentScan
```

အဓိက ၃ ခုမှတ်ထားပါ။

---

# 16. `@ComponentScan`

Spring က component တွေကို scan လုပ်တယ်။

ဥပမာ project—

```text
com.example
   │
   ├── Application.java
   │
   ├── controller
   │      └── CustomerController
   │
   ├── service
   │      └── CustomerService
   │
   └── repository
          └── CustomerRepository
```

Application class က root package မှာရှိရင် Spring က အောက်က packages တွေကို scan လုပ်နိုင်တယ်။

```text
com.example
     ↓
Component Scan
     ↓
@Controller
@Service
@Repository
@Component
```

တွေကိုရှာပြီး Bean တွေ register လုပ်တယ်။

---

# 17. Auto Configuration ဆိုတာဘာလဲ?

Spring Boot ရဲ့ အားသာချက်ကြီးတစ်ခု။

ဥပမာ Maven dependency ထည့်တယ်—

```xml
<dependency>
    <groupId>org.springframework.boot</groupId>
    <artifactId>spring-boot-starter-web</artifactId>
</dependency>
```

Spring Boot က web application ဖြစ်တာကို သိပြီး—

```text
Web configuration
DispatcherServlet
Embedded server
JSON support
HTTP handling
```

စတာတွေကို auto configure လုပ်ပေးတယ်။

Database dependency ထည့်ရင်လည်း—

```text
DataSource
Connection Pool
Transaction related infrastructure
```

တွေကို configuration အပေါ်မူတည်ပြီး auto configure လုပ်ပေးနိုင်တယ်။

### Interview answer

> **"Spring Boot auto-configuration automatically configures common application components based on the dependencies available on the classpath and the application's configuration."**

---

# 18. Spring Boot Application Start တဲ့အချိန် ဘာဖြစ်လဲ?

ဒါက Interview မှာ အရမ်းကောင်းတဲ့ question ပါ။

```java
SpringApplication.run(Application.class, args);
```

ဒီလို run လုပ်တဲ့အခါ high-level flow က—

```text
main()
  ↓
SpringApplication.run()
  ↓
Create Spring Application
  ↓
Create ApplicationContext
  ↓
Load configuration
  ↓
Component Scan
  ↓
Auto Configuration
  ↓
Create Beans
  ↓
Dependency Injection
  ↓
Start Embedded Server
  ↓
Application Ready
```

ဥပမာ—

```text
CustomerController
       ↓
CustomerService
       ↓
CustomerRepository
```

Spring က dependency order အရလိုအပ်တဲ့ Beans တွေ create/configure လုပ်ပြီး inject လုပ်ပေးတယ်။

---

# 19. Constructor Injection

ကျွန်တော် recommend လုပ်တာက Constructor Injection ပါ။

```java
@Service
public class CustomerService {

    private final CustomerMapper customerMapper;

    public CustomerService(CustomerMapper customerMapper) {
        this.customerMapper = customerMapper;
    }
}
```

Spring က—

```text
CustomerMapper Bean
       ↓
CustomerService constructor
       ↓
customerMapper injected
```

လုပ်ပေးတယ်။

### ဘာကြောင့် Constructor Injection သုံးတာလဲ?

* Dependency မရှိရင် object create မလုပ်နိုင်
* Required dependencies ရှင်းလင်းတယ်
* `final` သုံးလို့ရတယ်
* Testing လွယ်တယ်
* Circular dependency ကို detect လုပ်ရာမှာ အထောက်အကူဖြစ်တယ်

---

# 20. Field Injection

ဒါမျိုးလည်းတွေ့ရတတ်တယ်။

```java
@Autowired
private CustomerService customerService;
```

အလုပ်လုပ်တယ်။

ဒါပေမယ့် modern Spring development မှာ **Constructor Injection ကို ပို recommend လုပ်တယ်။**

---

# 21. `@Autowired` ဘာလုပ်တာလဲ?

`@Autowired` က Spring ကို—

> "ဒီ dependency ကို Spring Container ထဲက ရှာပြီး inject လုပ်ပါ"

လို့ပြောတာ။

ဥပမာ—

```java
@Autowired
private CustomerService customerService;
```

Spring က `CustomerService` Bean ကိုရှာပြီး inject လုပ်တယ်။

Constructor injection မှာတော့ constructor parameter ကို Spring က resolve လုပ်နိုင်တဲ့အတွက် `@Autowired` မရေးဘဲလည်း ရတယ်။

---

# 22. အခု REST API တစ်ခုကို End-to-End ကြည့်မယ်

ဒီအပိုင်းကို **Interview အတွက် အဓိကမှတ်ထားပါ။**

Client က—

```http
GET /api/customers/10
```

ပို့တယ်။

Flow က—

```text
Client
  ↓
HTTP Request
  ↓
Embedded Tomcat
  ↓
DispatcherServlet
  ↓
Controller
  ↓
Service
  ↓
MyBatis Mapper
  ↓
Oracle
  ↓
MyBatis maps Result
  ↓
Service
  ↓
Controller
  ↓
Jackson
  ↓
JSON Response
  ↓
Client
```

တစ်ခုချင်းကြည့်မယ်။

---

# 23. Embedded Tomcat

Spring Boot Web application run လုပ်တဲ့အခါ embedded web server ပါလာနိုင်တယ်။

ပုံမှန်အားဖြင့် Spring Boot MVC application တွေမှာ **Tomcat** ကို အသုံးများတယ်။

ဥပမာ—

```text
http://localhost:8080
```

ကို browser/client က request ပို့တယ်။

Tomcat က HTTP request ကို လက်ခံတယ်။

---

# 24. DispatcherServlet

Spring MVC ရဲ့ **Front Controller** လို့ နားလည်လို့ရတယ်။

Request—

```text
GET /api/customers/10
```

ဝင်လာရင်—

```text
Tomcat
   ↓
DispatcherServlet
```

ပြီးတော့ ဘယ် Controller method က handle လုပ်ရမလဲဆိုတာ resolve လုပ်တယ်။

---

# 25. Controller

```java
@RestController
@RequestMapping("/api/customers")
public class CustomerController {

    private final CustomerService service;

    public CustomerController(CustomerService service) {
        this.service = service;
    }

    @GetMapping("/{id}")
    public Customer getCustomer(@PathVariable Long id) {
        return service.getById(id);
    }
}
```

Request—

```http
GET /api/customers/10
```

ဆိုရင်—

```java
@GetMapping("/{id}")
```

ကို match လုပ်တယ်။

`10` ကို—

```java
@PathVariable Long id
```

ထဲထည့်တယ်။

---

# 26. Service Layer

Controller က business logic အများကြီးမလုပ်သင့်ဘူး။

```java
@Service
public class CustomerService {

    private final CustomerMapper mapper;

    public CustomerService(CustomerMapper mapper) {
        this.mapper = mapper;
    }

    public Customer getById(Long id) {
        return mapper.findById(id);
    }
}
```

Flow—

```text
Controller
    ↓
Service
```

Service က business rules တွေ handle လုပ်နိုင်တယ်။

ဥပမာ—

```java
if (!customer.isActive()) {
    throw new CustomerInactiveException();
}
```

---

# 27. MyBatis Layer

Service က—

```java
mapper.findById(id);
```

ခေါ်တယ်။

Mapper—

```java
@Mapper
public interface CustomerMapper {

    Customer findById(Long id);
}
```

XML—

```xml
<select id="findById"
        resultType="Customer">

    SELECT *
    FROM CUSTOMER
    WHERE ID = #{id}

</select>
```

MyBatis က SQL ကို execute လုပ်တယ်။

```text
Service
   ↓
MyBatis Mapper
   ↓
SQL
   ↓
Oracle
```

---

# 28. Database Result ပြန်လာရင်?

Oracle က—

```text
ID = 10
NAME = John
EMAIL = john@gmail.com
```

ပြန်ပေးတယ်။

MyBatis က database result ကို Java object အဖြစ် map လုပ်ပေးတယ်။

```java
Customer customer
```

ပြီးတော့—

```text
MyBatis
   ↓
Customer Object
   ↓
Service
   ↓
Controller
```

---

# 29. JSON Response ဘယ်လိုဖြစ်သွားလဲ?

Controller က—

```java
return customer;
```

လုပ်တယ်။

Spring MVC က message converter တွေကနေ Java object ကို JSON အဖြစ် serialize လုပ်ပေးတယ်။ Spring Boot web setup မှာ Jackson ကို အသုံးများတယ်။

Java object—

```java
Customer {
    id = 10,
    name = "John",
    email = "john@gmail.com"
}
```

Response—

```json
{
  "id": 10,
  "name": "John",
  "email": "john@gmail.com"
}
```

ဖြစ်သွားတယ်။

---

# 30. အခု CRUD တစ်ခုလုံး

```text
                    SPRING BOOT
                         │
                         ▼
                     Controller
                         │
                         ▼
                       Service
                         │
                         ▼
                  MyBatis Mapper
                         │
                         ▼
                       Oracle
                         │
                         ▼
                  Database Result
                         │
                         ▼
                  MyBatis Mapping
                         │
                         ▼
                       Service
                         │
                         ▼
                     Controller
                         │
                         ▼
                    JSON Response
```

ဒါကို interview မှာ explain လုပ်နိုင်ရင် အရမ်းကောင်းတယ်။

---

# 31. `@Transactional` ဘယ်နေရာဝင်လာလဲ?

ဥပမာ Order တစ်ခု create လုပ်တဲ့အခါ—

```text
Create Order
      +
Update Wallet
      +
Create Transaction
```

သုံးခုလုံး success ဖြစ်ရမယ်။

```java
@Transactional
public void createOrder(Order order) {

    orderMapper.insert(order);

    walletMapper.updateBalance(...);

    transactionMapper.insert(...);
}
```

ဒီမှာ transaction boundary တစ်ခုအဖြစ် handle လုပ်ပေးတယ်။

Conceptually—

```text
BEGIN TRANSACTION

INSERT order
UPDATE wallet
INSERT transaction

       ↓

Everything OK?
    /       \
  YES       NO
   ↓         ↓
 COMMIT    ROLLBACK
```

---

# 32. `@Transactional` ဘယ်လိုအလုပ်လုပ်လဲ?

ဒီမှာ Spring ရဲ့ **AOP / Proxy** concept ဝင်လာတယ်။

အရေးကြီးပါတယ်။

သင်ရေးထားတာ—

```java
@Transactional
public void createOrder() {
    ...
}
```

Spring က method ကို ရိုးရိုးတိုက်ရိုက် execute လုပ်တာမဟုတ်ဘဲ transactional proxy/interceptor infrastructure ကနေ ဖြတ်သန်းစေတယ်။

Conceptually—

```text
Controller
   ↓
Spring Proxy
   ↓
Start Transaction
   ↓
Real Service Method
   ↓
Database operations
   ↓
Success?
   ↓
Commit
```

Exception ဖြစ်ရင် rollback rules အရ rollback လုပ်နိုင်တယ်။

---

# 33. AOP ဆိုတာဘာလဲ?

## AOP = Aspect-Oriented Programming

Application ရဲ့ main business logic မဟုတ်ပေမယ့် classes/methods အများကြီးမှာ common ဖြစ်နေတဲ့ concerns တွေကို separate လုပ်ပေးနိုင်တယ်။

ဥပမာ—

* Logging
* Transaction
* Security
* Performance monitoring

Conceptually—

```text
Business Logic
      +
Cross-cutting concerns
```

---

# 34. Spring AOP Example

```java
@Around("execution(* com.example.service.*.*(..))")
public Object logExecution(ProceedingJoinPoint joinPoint)
        throws Throwable {

    long start = System.currentTimeMillis();

    Object result = joinPoint.proceed();

    long time = System.currentTimeMillis() - start;

    System.out.println("Execution time: " + time);

    return result;
}
```

Service method အားလုံးရဲ့ execution time ကို logging လုပ်နိုင်တယ်။

---

# 35. Bean Lifecycle

Spring Bean တစ်ခု create လုပ်တဲ့အချိန်မှာ lifecycle ရှိတယ်။

Simplified flow—

```text
Bean Definition
      ↓
Instantiate Object
      ↓
Dependency Injection
      ↓
Initialization
      ↓
Bean Ready
      ↓
Application Running
      ↓
Destroy
```

ဥပမာ—

```java
@PostConstruct
public void init() {
    System.out.println("Bean initialized");
}
```

Application shutdown မှာ—

```java
@PreDestroy
public void cleanup() {
    System.out.println("Cleanup");
}
```

လို lifecycle hooks တွေရှိတယ်။

---

# 36. Singleton Bean

Spring ရဲ့ default bean scope က **Singleton** ဖြစ်တယ်။

ဥပမာ—

```java
@Service
public class CustomerService {
}
```

ပုံမှန်အားဖြင့် Spring ApplicationContext တစ်ခုအတွင်း `CustomerService` Bean instance တစ်ခုကို manage လုပ်တယ်။

```text
ApplicationContext
       │
       └── CustomerService Bean
                 ↑
          Controller A
          Controller B
          Controller C
```

အဲဒီ Bean ကို request တိုင်းမှာ အသစ် `new` လုပ်တာမျိုးမဟုတ်ဘူး။

ဒါကြောင့် singleton service ထဲမှာ shared mutable state ထည့်တဲ့အခါ **thread safety** ကို သတိထားရတယ်။

---

# 37. Stateless Service ဘာကြောင့်ကောင်းလဲ?

ဥပမာ မကောင်းတဲ့ design—

```java
@Service
public class CustomerService {

    private Customer currentCustomer;

}
```

Multiple requests တစ်ချိန်တည်းဝင်လာရင် shared state ဖြစ်သွားနိုင်တယ်။

Better—

```java
@Service
public class CustomerService {

    public Customer getById(Long id) {
        // local variables
    }
}
```

Request-specific data ကို method local variable / request scope စတာတွေနဲ့ manage လုပ်တာ ပိုလုံခြုံတယ်။

---

# 38. Spring Boot Configuration

ဥပမာ—

```properties
server.port=8080

spring.datasource.url=jdbc:oracle:thin:@localhost:1521:xe
spring.datasource.username=app
spring.datasource.password=secret
```

Spring Boot က configuration ကို ဖတ်ပြီး infrastructure တွေကို configure လုပ်တယ်။

Production မှာ password ကို source code ထဲ hard-code မလုပ်သင့်ဘူး။

---

# 39. Profiles

Environment မတူရင်—

```text
Development
Testing
Production
```

configuration မတူနိုင်တယ်။

ဥပမာ—

```text
application.yml
application-dev.yml
application-prod.yml
```

Production မှာ—

```text
Production DB
Production Redis
Production Kafka
```

Development မှာ—

```text
Local DB
Local Redis
Local Kafka
```

လိုခွဲထားနိုင်တယ်။

---

# 40. Global Exception Handling

API မှာ exception ဖြစ်ရင် Controller တိုင်းမှာ try/catch ရေးမယ့်အစား—

```java
@RestControllerAdvice
public class GlobalExceptionHandler {

    @ExceptionHandler(CustomerNotFoundException.class)
    public ResponseEntity<?> handleNotFound(
            CustomerNotFoundException e) {

        return ResponseEntity
                .status(HttpStatus.NOT_FOUND)
                .body(e.getMessage());
    }
}
```

သုံးနိုင်တယ်။

Flow—

```text
Controller
   ↓
Service
   ↓
Exception
   ↓
@RestControllerAdvice
   ↓
HTTP 404
   ↓
JSON Error Response
```

---

# 41. Validation

Request body—

```java
public class CreateCustomerRequest {

    @NotBlank
    private String name;

    @Email
    private String email;
}
```

Controller—

```java
@PostMapping
public Customer create(
        @Valid @RequestBody CreateCustomerRequest request) {

    return service.create(request);
}
```

Invalid request ဖြစ်ရင် Spring validation infrastructure က validation error ပြန်ပေးနိုင်တယ်။

---

# 42. Spring Boot + MyBatis Full Architecture

သင့် interview အတွက် ဒီ architecture ကို memorize လုပ်ထားပါ။

```text
                     CLIENT
                       │
                       │ HTTP
                       ▼
               ┌───────────────┐
               │    Tomcat     │
               └───────┬───────┘
                       │
                       ▼
               ┌───────────────┐
               │ Dispatcher    │
               │ Servlet       │
               └───────┬───────┘
                       │
                       ▼
               ┌───────────────┐
               │  Controller   │
               └───────┬───────┘
                       │
                       ▼
               ┌───────────────┐
               │    Service    │
               │ Business Logic│
               └───────┬───────┘
                       │
                       ▼
               ┌───────────────┐
               │ MyBatis Mapper│
               └───────┬───────┘
                       │
                       ▼
               ┌───────────────┐
               │    Oracle     │
               └───────────────┘
```

အပြင်ဘက်မှာ—

```text
Spring IoC Container
       │
       ├── Controller Bean
       ├── Service Bean
       ├── Mapper Bean
       ├── DataSource
       ├── Transaction Manager
       └── Other Beans
```

---

# 43. ဒီနေရာမှာ IoC က ဘယ်မှာလဲ?

အပေါ်က architecture ကို ပြန်ကြည့်ပါ။

```java
@RestController
public class CustomerController {

    private final CustomerService service;

    public CustomerController(CustomerService service) {
        this.service = service;
    }
}
```

Controller ကိုယ်တိုင်—

```java
new CustomerService()
```

မလုပ်ဘူး။

Spring က—

```text
Spring Container
      ↓
CustomerService Bean
      ↓
inject
      ↓
CustomerController
```

လုပ်ပေးတယ်။

**ဒါက IoC + DI ရဲ့ practical example ပါ။**

---

# 44. Spring Boot vs Spring Framework

Interview မှာ ဒီဟာမေးနိုင်တယ်။

### Spring Framework

Core framework ဖြစ်ပြီး—

* IoC
* DI
* AOP
* MVC
* Transaction management

စတဲ့ infrastructure တွေကို ပေးတယ်။

### Spring Boot

Spring ကို production application တည်ဆောက်ရတာ ပိုလွယ်အောင်—

* Auto Configuration
* Starter dependencies
* Embedded server
* Externalized configuration
* Production-oriented features

စတာတွေကို ပိုလွယ်ကူအောင် ပေးတယ်။

အလွယ်—

```text
Spring Framework
      ↓
Core foundation

Spring Boot
      ↓
Spring application ကို configure/run လုပ်ရတာ ပိုလွယ်စေတယ်
```

---

# 45. Spring Boot Interview မှာ ဒီ Flow ကို ပြောနိုင်ရမယ်

Interviewer က—

> **"Can you explain how a Spring Boot application works?"**

မေးရင် ဒီလိုပြောပါ။

### English interview answer

> **"When a Spring Boot application starts, `SpringApplication.run()` creates the application context. Spring performs component scanning and auto-configuration, then creates and manages the required beans and injects their dependencies.**
>
> **For a REST request, the request first reaches the embedded web server and then the DispatcherServlet. The DispatcherServlet routes the request to the appropriate controller. The controller calls the service layer, where the business logic is handled. The service then calls the repository or MyBatis mapper to access the database. The result is mapped back to a Java object, and Spring serializes it into JSON and returns it to the client.**
>
> **For operations that require transaction management, Spring can use `@Transactional`, which is implemented through Spring's proxy/AOP infrastructure."**

ဒါက **တော်တော် strong answer** ဖြစ်ပါတယ်။

---

# 46. မြန်မာလို အဓိပ္ပါယ်

> Spring Boot application စတင်တဲ့အချိန် `SpringApplication.run()` က ApplicationContext ကို create လုပ်ပေးပါတယ်။ အဲဒီနောက် component scanning နဲ့ auto-configuration လုပ်ပြီး လိုအပ်တဲ့ Beans တွေကို create/manage လုပ်ကာ dependency တွေကို inject လုပ်ပေးပါတယ်။
>
> REST request ဝင်လာတဲ့အခါ embedded server က request ကို လက်ခံပြီး DispatcherServlet ဆီရောက်ပါတယ်။ DispatcherServlet က သက်ဆိုင်ရာ Controller ကို route လုပ်ပေးပါတယ်။ Controller က Service ကိုခေါ်ပြီး business logic ကို Service layer မှာလုပ်ပါတယ်။ ပြီးရင် Service က Repository သို့မဟုတ် MyBatis Mapper ကနေ Database ကို access လုပ်ပါတယ်။
>
> Database result ပြန်လာတဲ့အခါ MyBatis က Java object အဖြစ် map လုပ်ပေးပြီး Controller ဆီပြန်ရောက်ပါတယ်။ Spring က Java object ကို JSON အဖြစ် serialize လုပ်ပြီး Client ဆီ response ပြန်ပေးပါတယ်။
>
> Transaction လိုအပ်တဲ့ operation တွေအတွက် `@Transactional` ကိုအသုံးပြုပြီး Spring ရဲ့ proxy/AOP infrastructure ကနေ transaction ကို manage လုပ်ပေးနိုင်ပါတယ်။

---

# 47. ⭐ Interview အတွက် အရေးကြီးဆုံး Mental Model

ဒီတစ်ကြောင်းကို နားလည်အောင်လုပ်ပါ—

```text
@SpringBootApplication
        ↓
SpringApplication.run()
        ↓
ApplicationContext
        ↓
Component Scan + Auto Configuration
        ↓
Create Beans
        ↓
Dependency Injection
        ↓
Embedded Server
        ↓
HTTP Request
        ↓
DispatcherServlet
        ↓
Controller
        ↓
Service
        ↓
Repository / MyBatis
        ↓
Database
        ↓
Java Object
        ↓
JSON
        ↓
HTTP Response
```

ပြီးတော့ cross-cutting features တွေက—

```text
                 Spring
                   │
       ┌───────────┼────────────┐
       ↓           ↓            ↓
   IoC / DI      AOP       Transaction
       │           │            │
     Beans      Logging    @Transactional
```

---

## 🔥 Interview မှာ ဆက်မေးနိုင်တဲ့ Questions

ဒီအပိုင်းကိုတော့ အထူးသဖြင့် ပြင်ဆင်ထားပါ—

1. **What is IoC?**
2. **What is Dependency Injection?**
3. **IoC and DI difference?**
4. **What is a Spring Bean?**
5. **What is ApplicationContext?**
6. **How does `@SpringBootApplication` work?**
7. **What is component scanning?**
8. **What is auto-configuration?**
9. **`@Component`, `@Service`, `@Repository` difference?**
10. **Why constructor injection?**
11. **What happens when Spring Boot starts?**
12. **What happens when HTTP request enters Spring Boot?**
13. **What is DispatcherServlet?**
14. **How does Controller → Service → Repository work?**
15. **How does `@Transactional` work?**
16. **What is Spring AOP?**
17. **What is Spring Bean lifecycle?**
18. **What is Singleton scope?**
19. **How does Spring handle exceptions?**
20. **Spring Boot vs Spring Framework?**
