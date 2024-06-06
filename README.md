# CacheUser Annotation Library

The `CacheUser` annotation library provides an easy way to use caching in your Spring Boot applications with Redis. This guide will help you get started with adding caching to your services.


## Features

- Simple annotation for caching using Redis.
- Compatible with Spring Boot.
- Flexible parameter mapping for cache keys.


## Getting Started

### Prerequisites

- Java 17 or later.
- Maven 3.6.3 or later.
- Spring Boot 2.5.0 or later.
- Redis server.

### Installation

1. **Add the Dependency**

   Add the following dependency to your `pom.xml` file:

   ```xml
   <dependency>
       <groupId>com.aditya</groupId>
       <artifactId>task</artifactId>
       <version>1.0.1-SNAPSHOT</version>
   </dependency>
   ```

### Setup

1. **Redis Configuration**

   Configure Redis in your application.properties or application.yml:

   ```properties
   # Redis configuration
   spring.redis.host=$(hostUrl)
   spring.redis.port=$(redisPort)
   spring.redis.password=$(redisPassword)
   ```


## Structure

The `Cacheuser` annotation is designed to provide caching capabilities for methods within the application. Here's a breakdown of its structure and the significance of each argument:

- **Retention Policy**: `@Retention(RetentionPolicy.RUNTIME)`
  - This line indicates that the annotation information should be retained at runtime, allowing it to be accessible via reflection.

- **Target**: `@Target(ElementType.METHOD)`
  - Specifies that the annotation can only be applied to methods. Methods annotated with `@Cacheuser` will be eligible for caching.

- **Annotation Attributes**:

  - `key()`
    - This attribute denotes the cache key associated with the annotated method. It is a mandatory parameter and must be provided when using the `@Cacheuser` annotation.

  - `parameterMappings()`
    - An array of `ParameterMapping` objects. This attribute allows you to specify parameter mappings if the cached data depends on method parameters. It defaults to an empty array if not provided.

  - `ttl()`
    - This attribute indicates the time-to-live (TTL) for cached data in seconds. It is optional and has a default value of 3600 seconds (1 hour). You can adjust this value based on your caching requirements.

```
@Retention(RetentionPolicy.RUNTIME)
@Target(ElementType.METHOD)
public @interface Cacheuser {
    String key();
    ParameterMapping[] parameterMappings() default {}; // Array of ParameterMapping objects
    long ttl() default 3600; // Optional TTL with default value
}
```


## Usage

1. **Annotate Your Methods**
   
   - Use the @CacheUser annotation on methods where you want to enable caching.
   - The key attribute defines the cache key prefix, and parameterMappings allows you to map method parameters to the cache key.
   - When passing objects as arguments, you can use parameterName and requestIdentifiers to specify which fields should be used to uniquely identify requests.

2. **Usage with parameterName**

   ```example
   import com.aditya.task.annotation.CacheUser;
   import com.aditya.task.annotation.ParameterMapping;
   import org.springframework.web.bind.annotation.*;
   
   @RestController
   @RequestMapping("/products")
   public class ProductController {
   
       @CacheUser(key = "ProductCache", parameterMappings = {
               @ParameterMapping(parameterName = "id")
       }, ttl = 600)
       @GetMapping("/{id}")
       public Product getProductById(@PathVariable String id) {
           // Method logic to fetch product by id
           return productService.getProductById(id);
       }
   }
   ```

3. **Usage with requestIdentifier**

   Alternatively, developers can pass arguments as objects of a designated class. The relevant field of the class intended to serve as the key in caching is specified within the requestIdentifier,
   while the object's name is defined in the parameterName.

   ```example2
   import com.aditya.task.annotation.CacheUser;
   import com.aditya.task.annotation.RequestIdentifier;
   import org.springframework.web.bind.annotation.*;
   
   @RestController
   @RequestMapping("/users")
   public class UserController {
   
       @CacheUser(key = "UserCache", parameterMappings = {
               @ParameterMapping(parameterName = "user",requestIndentifier="username")
       }, ttl = 120)
       @PostMapping("/create")
       public User createUser(@RequestBody User user) {
           // Method logic to create user
           return userService.createUser(user);
       }
   }
   ```

5. **Usage with multiple parameters**

   Furthermore, it's possible to pass multiple parameters to generate the cache key. This can be achieved by providing two parameterNames within the parameterMappings.

   ```example3
   import com.aditya.task.annotation.CacheUser;
   import com.aditya.task.annotation.ParameterMapping;
   import org.springframework.web.bind.annotation.*;
   
   @RestController
   @RequestMapping("/products")
   public class ProductController {
   
       @CacheUser(key = "ProductCache", parameterMappings = {
               @ParameterMapping(parameterName = "id"),
               @ParameterMapping(parameterName = "type")
       }, ttl = 120)
       @GetMapping("/{id}/{type}")
       public Product getProductByIdAndType(@PathVariable String id, @PathVariable String type) {
           // Method logic to fetch product by id and type
           return productService.getProductByIdAndType(id, type);
       }
   }
   ```


## Troubleshooting

1. **Redis configuration fail**

   If Redis fails to connect then the method runs smoothly as expected but caching is not done with logging of the error as:
   
   ```xml
   "Failed to connect to Redis server: " + $errorMessage
   ```

2. **Parameter name invalid**

   If parameter name is given incorrectly then method resumes its execuion but caching is stopped as key cannot be generated





   

   
