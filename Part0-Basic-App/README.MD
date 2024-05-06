# Basic Reliable Web App Demo

In this quick demo, we will explore reliable web app patterns. These are design strategies used in web development to ensure the robustness, scalability, and efficiency of web applications. We will use a simple project to demonstrate core reliable web app patterns such as caching, resiliency, and fault tolerance.

- For caching, the project employs the cache-aside pattern and utilizes the Caffeine library to manage the cache effectively.
- To improve system reliability, resiliency patterns such as circuit breakers and retries are implemented, enhancing the application's fault tolerance.

## Prerequisites

- Java 17
- Maven 3.6+

## Installation

1. Navigate to the project directory:

```bash
cd Part0-Basic-App/demo
```

2. Install dependencies:

```bash
mvn clean install
```

## Usage

Start the application using Maven:

```bash
mvn spring-boot:run
```

You can then access the API at `http://localhost:8080/product/1`.

### Example API Call

To retrieve a product by its ID, you can use the following curl command:

```bash
curl http://localhost:8080/product/1
```

## Running the Tests

To run the automated tests for this system, use the following command:

```bash
mvn test
```

These tests verify the functionality of all components, ensuring that the application behaves as expected.

## Failure Mode

The application includes a feature to simulate failures, useful for testing its resilience capabilities such as the circuit breaker and retry mechanisms. This simulation can be controlled directly from a web browser, making it easy to demonstrate or test the effects of these patterns.

### Enabling Failures

To simulate failures in the system, which will trigger the circuit breaker or retry logic, you can activate failure mode using the following URL:

```plaintext
http://localhost:8080/configure/failure?fail=true
```

### Disabling Failures

To return the system to normal operation, disable the failure mode using this URL:

```plaintext
http://localhost:8080/configure/failure?fail=false
```

## conclusion

This project provides a simple demonstration of reliable web app patterns in action. By exploring the code and running the application, you can gain a better understanding of how these patterns can enhance the reliability and performance of web applications.

## Next Up

Next, we will deploy the RWA reference application on Azure --> [Part 1 - Tooling](../Part1-Tooling/README.md) 
