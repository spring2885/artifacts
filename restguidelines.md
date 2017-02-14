# Java Rest API best practices. Guidelines with Spring Boot

This page has been extended by [@spring2885](https://github.com/spring2885) with permission from [@melphi](https://github.com/melphi)

### Run the application

To run from **maven**:
```shell
./mvnw compile
./mvnw spring-boot:run
```

### Folder Structure

The _spring2885/backend_ project structure follows these **guidelines**:
```
server
|-- mvnw
|-- mvnw.cmd
|-- pom.xml
|-- run.sh
|-- src/main/java/org/spring2885
                          |-- model
                          |-- server
                                |-- ServerApplication.java
                                |-- api
                                    ...
                                    |-- exceptions
                                    |-- utils
                                |-- db
                                    |-- model
                                    |-- service
                                        |-- person
                                        |-- search
                                |-- mail
                                |-- web
|-- src/main/resources/
                    |-- application-properties
                    |-- api-docs
|-- src/test/java/org/spring2885/
                            |-- server
                                |-- api
                                |-- db
                                    |-- model
                                    |-- service
                                        |-- search
                                    |-- utils
```

### Container

The application container configuration, for example the startup class, security and persistence configuration, web filters, etc. 

`ServerApplication.java` is the entry point which starts the application.

### Module

Application is split in modules. Think of a module of as a separated domain. For example, _user module_ contains all the classes related to _user_ operations. 

When another module, for example _company module_, needs to comunicate with the _user module_, it will use the _UserService interface_. Ideally, the application should be designed in a way in which modules can be split in separated applications following microservice principles.

A common module contains base classes used by other modules.

### Domain

The domain contains _interfaces_, _dto_ and _entities_.

Dto, which stands for Data Transfer Object, are objects returned by the service layer and serialised to Json.

Entities are the object to database mapping, which can use for example Hibernate or Spring Data _annotations_.

Interfaces are used to enforce compile time checks while converting Entities to Dto. This way if a new field is introduced, the compiler can be used to check that all the related objects contain the required fields.

A **best practice** is to structure the Dto as an _immutable object_ as follows:

```java
public class UserPrivateDto implements UserPrivate {
  private String id;

  private String email;

  private String fullName;

  private String phoneNumber;

  public UserPrivateDto() {
    // Intentionally empty.
  }

  public UserPrivateDto(UserPrivate user) {
    this(user.getId(), user.getEmail(), user.getFullName(), user.getPhoneNumber());
  }

  public UserPrivateDto(String id, String email, String fullName, String phoneNumber) {
    this.id = id;
    this.email = email;
    this.fullName = fullName;
    this.phoneNumber = phoneNumber;
  }

  @Override
  public String getId() {
    return id;
  }

  @Override
  public String getFullName() {
    return this.fullName;
  }

  @Override
  public String getPhoneNumber() {
    return this.phoneNumber;
  }

  @Override
  public String getEmail() {
    return this.email;
  }
} 
```

We can have a version of Dto for public data and for private data or compose various domain objects in one in order to produce the required JSON output.

### Dao

Dao stands for _Data Access Object_ and contains the interfaces to get and store objects in the database. It returns entitiy objects but not dto, which are returned by the service layer. The Dao depends on the persistence implemetation used, for example Hibernate or MongoDB.

In **Spring Boot** a Dao could be implemented following this **guideline**.

```java
@Component
public class UserDaoImpl extends AbstractDao<UserEntity> implements UserDao {
  @Autowired
  public UserDaoImpl(MongoTemplate mongoTemplate) {
    super(mongoTemplate);
  }  
 ```
 ```java
  @Override
  public UserPrivate createUser(UserRegistration userRegistration) {
    // TODO: Implement this method.
  }
} 
```

### Service

Service classes implement all the the business logic. They connect to the Dao layer via dao interfaces. Also, they return Dto objects which will be serialised from the rest layer as Json objects.

This is an example wich follows some **best API practices**:

```java
@Component
public class UserServiceImpl implements UserService {
  private static final Logger LOGGER = LoggerFactory.getLogger(UserServiceImpl.class);
  
  private UserDao userDao;

  @Autowired
  public UserServiceImpl(UserDao userDao) {
    this.userDao = userDao;
  }

  @Override
  public UserRegistrationResponseDto registerUser(UserRegistrationDto userRegistrationDto)
      throws UserRegistrationException {
    validateUserRegistration(userRegistrationDto);
    try {
      userDao.createUser(userRegistrationDto);
      return new UserRegistrationResponseDto();
    } catch (Exception e) {
      throw new UserRegistrationException(e.getMessage(), e);
    }
  }
}
```

### Rest

Rest layer contains the **REST API** controllers. They link to the service layer via service interfaces. A rest layer should never implement any business logic; just the endpoint mapping, request parameters, and security.

```java
@Api(tags = "users")
@RestController
@RequestMapping(value = "/users")
public class UserController {
  private UserService userService;

  @Autowired
  public UserController(UserService userService) {
    this.userService = userService;
  }

  @ApiOperation(value = "Registers a new user.", tags = "users")
  @RequestMapping(path = "/register", method = RequestMethod.POST)
  public UserRegistrationResponseDto registerUser(
      @RequestBody UserRegistrationDto userRegistrationDto) throws UserRegistrationException {
    return userService.registerUser(userRegistrationDto);
  }
}
```

### Exception and Util

It is a good practice to use your own custom exceptions, which can be defined in our exception package.

Also, exception messages should be encoded with a prefix. This way the user interface can translate all the messages starting with prefix. The error messages and other utility classes can be stored in the util package.
