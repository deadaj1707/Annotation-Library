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
       })
       @GetMapping("/{id}")
       public Product getProductById(@PathVariable String id) {
           // Method logic to fetch product by id
           return productService.getProductById(id);
       }
   }
   ```

3. **Usage wuth requestIdentifier**

   ```example2
   import com.aditya.task.annotation.CacheUser;
   import com.aditya.task.annotation.RequestIdentifier;
   import org.springframework.web.bind.annotation.*;
   
   @RestController
   @RequestMapping("/users")
   public class UserController {
   
       @CacheUser(key = "UserCache", parameterMappings = {
               @ParameterMapping(parameterName = "user",requestIndentifier="username")
       })
       @PostMapping("/create")
       public User createUser(@RequestBody User user) {
           // Method logic to create user
           return userService.createUser(user);
       }
   }

4. **Usage with multiple parameters**

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
       })
       @GetMapping("/{id}/{type}")
       public Product getProductByIdAndType(@PathVariable String id, @PathVariable String type) {
           // Method logic to fetch product by id and type
           return productService.getProductByIdAndType(id, type);
       }
   }
   ```





   

   
