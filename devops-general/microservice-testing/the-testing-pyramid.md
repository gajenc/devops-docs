---
description: Meet the testing pyramid and learn how to build a testing culture
---

# The Testing Pyramid

In order to build a thorough test framework we need to start with the concept of the “test pyramid.” This concept was developed by [Mike Cohn](https://en.wikipedia.org/wiki/Mike_Cohn) at a time when most software testing was done using manipulation of the software’s interface. Quality assurance engineers would manually interact with the software UI, or write automated macros that interacted with the UI. This type of testing often failed to detect underlying issues in the code. In this context the concept of the test pyramid was important because it popularized the idea that such UI tests should be just the tip of a pyramid of different layers of tests.

The goal of having multiple layers of tests is to catch different types of issues. Each type of test focuses on a different layer of the overall software system, and verifies behavior within that scope. For a distributed system with a client and backend the tests can be organized into the following layers:

![](https://miro.medium.com/max/2000/1\*bu11aPXn--adybsX9gPn4Q.png)

### Unit Tests <a href="adad" id="adad"></a>

Tests that verify the behavior of class methods or functions inside a service. These tests take a code file from the service’s codebase, and execute its functions or class methods using different inputs, while verifying the output of the functions for each input.

### Integration Tests <a href="7021" id="7021"></a>

Tests that verify the external behavior of a single service. The test framework starts an instance of the service, and then uses the service’s external interface to execute the business logic of the service in the same way that an external service would integrate with the tested service. For example, a REST API service would be tested by making HTTP requests to the service.

### End to End Tests <a href="602b" id="602b"></a>

Tests that verify the behavior of multiple services that communicate with each other. These tests are accomplished by running multiple services in an isolated environment where they can actually communicate with each other. End to end tests initiate a network request to a service, such as a REST API, then verify that other dependent backend services have updated in response to the input to the first service.

### UI Tests <a href="5f5a" id="5f5a"></a>

Tests that verify the behavior of an entire platform by starting from the client user interface. This tests both the client’s logic as well as the backend system, to verify that the client is able to communicate with the backend system and that state is updated and persisted properly between the client and the backend.

## Build a Testing Culture <a href="be9b" id="be9b"></a>

In order for tests to be effective they must be more than just a sidebar in the development process. Tests must be an integral part of the development and release pipeline and tests must be empowered to stop the release of failing code.

* Code that fails tests should never be merged into your code repository.
* Code that fails tests should never be released to customers.

Each layer of tests, starting from unit tests and progressing to UI tests, builds upon the last layer to test the system at a higher level of abstraction:

![](https://miro.medium.com/max/2000/1\*a7eQW7gEBzXEZW1wY_txGA.png)

Engineers must have the right mindset when it comes to tests. They are often responsible for writing tests as well as the features, so they have a lot of control over the quality and effectiveness of tests. If testing isn’t taken seriously software engineers will create rudimentary tests that fail to exercise all the edge cases of the features, or they will be tempted to take shortcuts to get “coverage” with passing tests that don’t really test anything.

The goal of testing isn’t to make passing tests. The goal is to make consistent, high quality software that delights customers. Tests ensure that software quality starts out high and stays high as features are added or changes are made to existing features.

This means that test failures need to be treated seriously. Tests can’t just be changed so that they pass. Any flaky tests that pass sometimes but fail other times need to be treated with extreme suspicion. If a bug is found in the product this means there is missing test coverage that needs to be filled. If engineers are aware of missing test coverage they need to ensure that they are given the time and/or resources to fix the issue.

The responsibility of creating a thorough testing culture doesn’t just lie with engineers. Product managers also need to be aware of the testing process and involved in it. If they are demanding that new features be built and released faster than engineers are able to develop good tests for those features then quality will suffer, and issues will start to make their way down the testing pipeline until they eventually reach customers and impact customer satisfaction.

## Conclusion <a href="a082" id="a082"></a>

Creating a thorough test framework for a distributed application requires that multiple layers of testing be employed. Quality assurance based on the client UI isn’t enough to catch all the types of bugs that may arise. Instead software engineers must develop a testing culture that incorporates automated testing into all aspects of the development and release pipeline. This includes writing unit tests for classes and functions, integration tests for services, end to end tests for entire systems, and UI tests to exercise both the client and the backend together.

