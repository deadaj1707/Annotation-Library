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

   ### Usage

   
