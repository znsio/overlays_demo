# Specmatic Overlays Demo

This repository demonstrates the power of overlays (and the support by Specmatic) - a feature that allows you to extend and modify OpenAPI specifications without changing the original contract. This is particularly useful when you need to:

- Add common headers across multiple endpoints
- Extend security requirements
- Add new parameters for monitoring or tracing
- Modify specifications for different environments

## Overview

The demo consists of three key files:
- `employees.yaml` - The base OpenAPI specification for an employee service
- `employees_overlay.yaml` - An overlay that adds required headers for API gateway integration
- `specmatic.yaml` - Configuration file for Specmatic

The overlay adds three important headers to the POST /employees endpoint:
- X-Correlation-ID: For request tracking
- X-Gateway-Token: For API gateway authentication
- X-Request-ID: For request tracing

## Documentation

For a complete understanding of Specmatic overlays, please refer to the [official documentation](https://specmatic.io/documentation/contract_tests.html#overlays).

## Running the Demo

You can run this demo using Docker, Specmatic Jar or Specmatic Node package. The demo involves two steps:
1. Starting the stub server
2. Running the tests to see overlay validation in action

### Using Specmatic Jar
#### Step 1: Start the stub server
```shell
java -jar specmatic.jar stub
```

#### Step 2: Start the test
```shell
java -jar specmatic.jar test --port=9000
```


### Using Docker
#### Step 1: Start the stub server
```shell
docker run -p="9000:9000" -v="./specmatic.yaml:/usr/src/app/specmatic.yaml" -v="./employees.yaml:/usr/src/app/employees.yaml" -v="./employees_overlay.yaml:/usr/src/app/employees_overlay.yaml" znsio/specmatic stub
```


#### Step 2: Run the tests
```shell
docker run -v="./specmatic.yaml:/usr/src/app/specmatic.yaml" -v="./employees.yaml:/usr/src/app/employees.yaml" -v="./employees_overlay.yaml:/usr/src/app/employees_overlay.yaml" znsio/specmatic test --port=9000 --host=host.docker.internal
```

### Using NPX
#### Step 1: Install Specmatic Node package:
```shell
npm install -g specmatic
```

#### Step 2: Start the stub server:
```shell
npx specmatic stub 
```

#### Step 3: In another terminal, run the tests:
```bash
npx specmatic test 
```

## What to Expect

When you run the tests, you'll notice that:

* Specmatic start the `stub` server starts and loads both the base contract and overlay
* Specmatic loads both the base ccontract and overlay for `test` also, and then requests with all required headers are used for testing.
* Test requests without the required headers (added by overlay) will fail

This demonstrates how overlays can enforce additional requirements without modifying the base contract.