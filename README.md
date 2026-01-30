# Spring Cloud Gateway Server WebFlux

A Spring Boot application demonstrating Spring Cloud Gateway Server WebFlux with various route predicate examples.

## 📋 Table of Contents

- [Overview](#overview)
- [Prerequisites](#prerequisites)
- [Getting Started](#getting-started)
- [Configuration Guide](#configuration-guide)
- [Running the Application](#running-the-application)
- [Route Predicates](#route-predicates)
- [Testing](#testing)
- [Troubleshooting](#troubleshooting)
- [References](#references)
- [Example Outputs](#example-outputs)

## 🎯 Overview

This project demonstrates how to configure and use Spring Cloud Gateway Server WebFlux, a standalone reactive gateway server built on Spring WebFlux. The gateway provides routing, filtering, and load balancing capabilities for microservices architectures.

### Key Features

- Reactive gateway server using Spring WebFlux
- Multiple route predicate examples
- Actuator endpoints for monitoring
- Comprehensive configuration examples

### Technology Stack

- **Spring Boot:** 4.0.2
- **Spring Cloud:** 2025.1.0
- **Java:** 25
- **Build Tool:** Gradle
- **Gateway:** Spring Cloud Gateway Server WebFlux

## 📦 Prerequisites

Before you begin, ensure you have the following installed:

- **Java 25** or higher
- **Gradle** (or use the included Gradle wrapper)
- **IDE** (IntelliJ IDEA, Eclipse, or VS Code recommended)

## 🚀 Getting Started

### 1. Clone the Repository

```bash
git clone <repository-url>
cd Webflux-Gateway
```

### 2. Build the Project

Using Gradle wrapper:

```bash
./gradlew build
```

Or using system Gradle:

```bash
gradle build
```

### 3. Run the Application

```bash
./gradlew bootRun
```

Or build and run the JAR:

```bash
./gradlew build
java -jar build/libs/demo-gateway-0.0.1-SNAPSHOT.jar
```

The application will start on `http://localhost:8080` by default.

## ⚙️ Configuration Guide

### ⚠️ Important Configuration Note

**CRITICAL:** When using Spring Cloud Gateway Server WebFlux (`spring-cloud-starter-gateway-server-webflux`), routes **MUST** be configured under:

```yaml
spring:
  cloud:
    gateway:
      server:
        webflux:
          routes:
            - id: my_route
              uri: https://example.org
              predicates:
                - Path=/api/**
```

**NOT** under `spring.cloud.gateway.routes` (this is for the traditional gateway library).

### Configuration Files

The project includes multiple configuration profiles:

- **`application.yml`** - Main configuration with active profile
- **`application-doc.yml`** - Example with incorrect path (for reference)
- **`application-new.yml`** - Correct configuration with proper path

### Setting Active Profile

To use a specific profile, set it in `application.yml`:

```yaml
spring:
  profiles:
    active: new  # or 'doc' for the example configuration
```

Or via command line:

```bash
./gradlew bootRun --args='--spring.profiles.active=new'
```

### Actuator Endpoints

The application exposes the following Actuator endpoints:

- `/actuator/health` - Health check
- `/actuator/info` - Application information
- `/actuator/metrics` - Application metrics
- `/actuator/prometheus` - Prometheus metrics
- `/actuator/gateway` - Gateway routes information

Access them at: `http://localhost:8080/actuator/{endpoint}`

## 🛣️ Route Predicates

The project includes examples of various route predicates. Here are some common patterns:

### Time-Based Predicates

```yaml
# After - Matches requests after specified datetime
- id: after_route
  uri: https://example.org
  predicates:
    - After=2017-01-20T17:42:47.789-07:00[America/Denver]

# Before - Matches requests before specified datetime
- id: before_route
  uri: https://example.org
  predicates:
    - Before=2025-12-31T23:59:59.999-07:00[America/Denver]

# Between - Matches requests between two datetimes
- id: between_route
  uri: https://example.org
  predicates:
    - Between=2017-01-20T17:42:47.789-07:00[America/Denver], 2025-12-31T23:59:59.999-07:00[America/Denver]
```

### Path Predicates

```yaml
# Path matching with segments
- id: path_route
  uri: https://example.org
  predicates:
    - Path=/red/{segment},/blue/{segment}

# Path with wildcard
- id: path_wildcard_route
  uri: https://example.org
  predicates:
    - Path=/foo/**
```

### Method Predicates

```yaml
# Match specific HTTP method
- id: method_get_route
  uri: https://example.org
  predicates:
    - Method=GET
```

### Header & Cookie Predicates

```yaml
# Header matching
- id: header_route
  uri: https://example.org
  predicates:
    - Header=X-Request-Id, \d+

# Cookie matching
- id: cookie_route
  uri: https://example.org
  predicates:
    - Cookie=chocolate, ch.p
```

### Query Parameter Predicates

```yaml
# Simple query parameter
- id: query_route_simple
  uri: https://example.org
  predicates:
    - Query=green

# Query parameter with regex
- id: query_route_regexp
  uri: https://example.org
  predicates:
    - Query=red, gree.
```

### Combined Predicates

```yaml
# Multiple conditions (logical AND)
- id: combined_route
  uri: https://example.org
  predicates:
    - Method=GET
    - Path=/api/**
    - Header=X-Request-Id, \d+
    - Query=version, v\d+
```

For more examples, see `application-new.yml` or `application-doc.yml`.

## 🧪 Testing

### Run Tests

```bash
./gradlew test
```

### Manual Testing

1. Start the application:
   ```bash
   ./gradlew bootRun
   ```

2. Test a route (example):
   ```bash
   curl http://localhost:8080/api/test
   ```

3. Check gateway routes:
   ```bash
   curl http://localhost:8080/actuator/gateway/routes
   ```

4. Check health:
   ```bash
   curl http://localhost:8080/actuator/health
   ```

## 🔧 Troubleshooting

### Routes Not Working

**Problem:** Routes are not being matched or requests are not being routed.

**Solution:** Ensure you're using the correct configuration path:
- ✅ Correct: `spring.cloud.gateway.server.webflux.routes`
- ❌ Incorrect: `spring.cloud.gateway.routes`

### Application Won't Start

1. Check Java version:
   ```bash
   java -version
   ```
   Should be Java 25 or higher.

2. Check if port 8080 is available:
   ```bash
   lsof -i :8080
   ```

3. Review logs for errors:
   ```bash
   ./gradlew bootRun
   ```

### Configuration Not Loading

1. Verify the active profile is set correctly in `application.yml`
2. Ensure YAML syntax is correct (proper indentation)
3. Check that the configuration file is in `src/main/resources/`

## 📚 References

### Official Documentation

- [Spring Boot Documentation](https://docs.spring.io/spring-boot/4.0.2/reference/htmlsingle/)
- [Spring Cloud Gateway Documentation](https://docs.spring.io/spring-cloud-gateway/reference/spring-cloud-gateway.html)
- [Spring Reactive Web](https://docs.spring.io/spring-boot/4.0.2/reference/web/reactive.html)
- [Spring Boot Actuator](https://docs.spring.io/spring-boot/4.0.2/reference/actuator/index.html)

### Guides

- [Building a Reactive RESTful Web Service](https://spring.io/guides/gs/reactive-rest-service/)
- [Building a RESTful Web Service with Spring Boot Actuator](https://spring.io/guides/gs/actuator-service/)
- [Using Spring Cloud Gateway](https://github.com/spring-cloud-samples/spring-cloud-gateway-sample)

### Build Tools

- [Gradle Documentation](https://docs.gradle.org)
- [Spring Boot Gradle Plugin](https://docs.spring.io/spring-boot/4.0.2/gradle-plugin)

## 📝 Notes

- The package name `com.gateway.demo-gateway` was changed to `com.gateway.demo_gateway` due to Java naming conventions (hyphens are not allowed in package names).
- This project demonstrates the correct configuration path for Spring Cloud Gateway Server WebFlux, which differs from the traditional Spring Cloud Gateway library.

## 🤝 Contributing

Feel free to submit issues and enhancement requests!

## 📄 License

This project is for demonstration purposes.

---

**Happy Routing! 🚀**

## 📊 Example Outputs

### Profile: doc

```bash
curl http://localhost:8080/actuator/gateway/routes
```

```json
[]
```

### Profile: new

```bash
curl http://localhost:8080/actuator/gateway/routes
```

```json
[
  {
    "predicate": "(RouteDefinitionRouteLocator$$Lambda/0x00000000844b8250 && After: 2017-01-20T17:42:47.789-07:00[America/Denver])",
    "route_id": "after_route",
    "filters": [],
    "uri": "https://example.org:443",
    "order": 0
  },
  {
    "predicate": "(RouteDefinitionRouteLocator$$Lambda/0x00000000844b8250 && Before: 2025-12-31T23:59:59.999-07:00[America/Denver])",
    "route_id": "before_route",
    "filters": [],
    "uri": "https://example.org:443",
    "order": 0
  },
  {
    "predicate": "(RouteDefinitionRouteLocator$$Lambda/0x00000000844b8250 && Between: 2017-01-20T17:42:47.789-07:00[America/Denver] and 2025-12-31T23:59:59.999-07:00[America/Denver])",
    "route_id": "between_route",
    "filters": [],
    "uri": "https://example.org:443",
    "order": 0
  },
  {
    "predicate": "(RouteDefinitionRouteLocator$$Lambda/0x00000000844b8250 && Cookie: name=chocolate regexp=ch.p)",
    "route_id": "cookie_route",
    "filters": [],
    "uri": "https://example.org:443",
    "order": 0
  },
  {
    "predicate": "(RouteDefinitionRouteLocator$$Lambda/0x00000000844b8250 && Header: X-Request-Id regexp=\\d+)",
    "route_id": "header_route",
    "filters": [],
    "uri": "https://example.org:443",
    "order": 0
  },
  {
    "predicate": "(RouteDefinitionRouteLocator$$Lambda/0x00000000844b8250 && Hosts: [**.somehost.org, **.anotherhost.org])",
    "route_id": "host_route",
    "filters": [],
    "uri": "https://example.org:443",
    "order": 0
  },
  {
    "predicate": "(RouteDefinitionRouteLocator$$Lambda/0x00000000844b8250 && Methods: [GET])",
    "route_id": "method_get_route",
    "filters": [],
    "uri": "https://example.org:443",
    "order": 0
  },
  {
    "predicate": "(RouteDefinitionRouteLocator$$Lambda/0x00000000844b8250 && Methods: [POST])",
    "route_id": "method_post_route",
    "filters": [],
    "uri": "https://example.org:443",
    "order": 0
  },
  {
    "predicate": "(RouteDefinitionRouteLocator$$Lambda/0x00000000844b8250 && Paths: [/red/{segment}, /blue/{segment}], match trailing slash: true)",
    "route_id": "path_route",
    "filters": [],
    "uri": "https://example.org:443",
    "order": 0
  },
  {
    "predicate": "(RouteDefinitionRouteLocator$$Lambda/0x00000000844b8250 && Paths: [/foo/**], match trailing slash: true)",
    "route_id": "path_wildcard_route",
    "filters": [],
    "uri": "https://example.org:443",
    "order": 0
  },
  {
    "predicate": "(RouteDefinitionRouteLocator$$Lambda/0x00000000844b8250 && Query: param=green regexp=null)",
    "route_id": "query_route_simple",
    "filters": [],
    "uri": "https://example.org:443",
    "order": 0
  },
  {
    "predicate": "(RouteDefinitionRouteLocator$$Lambda/0x00000000844b8250 && Query: param=red regexp=gree.)",
    "route_id": "query_route_regexp",
    "filters": [],
    "uri": "https://example.org:443",
    "order": 0
  },
  {
    "predicate": "(RouteDefinitionRouteLocator$$Lambda/0x00000000844b8250 && RemoteAddrs: [192.168.1.1/24])",
    "route_id": "remoteaddr_route",
    "filters": [],
    "uri": "https://example.org:443",
    "order": 0
  },
  {
    "predicate": "(RouteDefinitionRouteLocator$$Lambda/0x00000000844b8250 && XForwardedRemoteAddrRoutePredicateFactory$$Lambda/0x00000000844c0238)",
    "route_id": "xforwarded_remoteaddr_route",
    "filters": [],
    "uri": "https://example.org:443",
    "order": 0
  },
  {
    "predicate": "(RouteDefinitionRouteLocator$$Lambda/0x00000000844b8250 && Weight: group1 8)",
    "route_id": "weight_high",
    "filters": [],
    "uri": "https://weighthigh.org:443",
    "order": 0
  },
  {
    "predicate": "(RouteDefinitionRouteLocator$$Lambda/0x00000000844b8250 && Weight: group1 2)",
    "route_id": "weight_low",
    "filters": [],
    "uri": "https://weightlow.org:443",
    "order": 0
  },
  {
    "predicate": "((((RouteDefinitionRouteLocator$$Lambda/0x00000000844b8250 && Methods: [GET]) && Paths: [/api/**], match trailing slash: true) && Header: X-Request-Id regexp=\\d+) && Query: param=version regexp=v\\d+)",
    "route_id": "combined_route",
    "filters": [],
    "uri": "https://example.org:443",
    "order": 0
  }
]
```

