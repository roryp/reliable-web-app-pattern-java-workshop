
# Reliable Web App Demo

![Reliable](reliable.gif)

## Description

This project is a Java-based web application that offers a RESTful API for managing products. It utilizes Spring Boot for backend operations and includes features such as retrieving products by their IDs.

The application also integrates automated tests using JUnit and Mockito to ensure the reliability and correctness of its functionality.

For caching, the project employs the cache-aside pattern and utilizes the Caffeine library to effectively manage the cache.

To enhance system reliability, resiliency patterns such as circuit breakers and retries are implemented, improving the application's fault tolerance.

## Prerequisites

- Java 17
- Maven 3.6+

## Installation

1. Clone the repository:

```bash
git clone https://github.com/roryp/patterndemo.git
```

2. Navigate to the project directory:

```bash
cd patterndemo
```

3. Install dependencies:

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

To simulate failures in the system, which will trigger the circuit breaker or retry logic, you can activate the failure mode using the following URL:

```plaintext
http://localhost:8080/configure/failure?fail=true
```

### Disabling Failures

To return the system to normal operation, disable the failure mode using this URL:

```plaintext
http://localhost:8080/configure/failure?fail=false
```
