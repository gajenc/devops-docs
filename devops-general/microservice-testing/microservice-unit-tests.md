---
description: How to build and use unit tests for distributed systems
---

# Microservice - Unit Tests

This article zooms in on the unit test layer of the testing pyramid and explores ways to build effective unit tests for distributed systems such as microservice deployments.

## Identify the Test Boundaries <a href="4afa" id="4afa"></a>

For any test to be effective you first have to find the boundaries of that test. The goal of the test is to verify all behavior inside the “black box” of the test boundaries by manipulating the inputs to the black box, and verifying that the correct output is produced by the black box for each set of inputs.

In a unit test the black box that is being tested is a function or class method. The goal is to test the behavior of that specific block of code in isolation. To understand how this works consider a basic user signup function:

![](<../../.gitbook/assets/image (43).png>)

There are some obvious input and outputs to this function. The black box of the function accepts basic user signup details as parameter inputs, and it returns outputs such as the ID of the newly created user.

But there are also some less obvious inputs to this function. The function makes two external function calls: one call to a database to insert a record, and one call to a class that is responsible for password hashing and persistence. Under some conditions the database insert condition may return an error back into the user signup function. For example maybe there is a constraint that username must be unique and the database insert fails because the username is already taken. Or perhaps the password interface is a thin wrapper around an external password microservice, and behind the scenes it makes a network call. If there was a connectivity issue or the password service was overloaded and timed out it could cause that password function to return an error.

In order to fully test the behavior of the user signup function a unit test needs to do more than just pass different user signup parameters. It also needs to reproduce the inputs that external dependencies may introduce in order to verify that this function behaves appropriately based on those inputs as well. This especially important when it comes to testing the error handling of a function.

## Test Stubs & Mocks <a href="4f3c" id="4f3c"></a>

In order to reproduce input conditions that come from side effects it is necessary to implement function stubs, also called mocks. This can be done using dependency injection or method swizzling. Both techniques allow a test framework to execute a tested function while ensuring that any calls out to an underlying function dependency are redirected to a different function that is a stub used for testing:

![](<../../.gitbook/assets/image (44).png>)

The function stub can be used to do a number of different things:

* The stub can be setup to do nothing. This is a way to speed up the execution of a particular unit test by disabling the side effects of the tested function, especially if those side effects will be tested by another dedicated unit test that runs later.
* The stub can be used to return arbitrary values to simulate responses from external functions. This is useful for testing function behavior under conditions that may be hard to replicate, such as code that handles error conditions that rarely occur or are hard to reproduce.
* The stub can be used to capture parameters that the function attempted to pass to an external function, or just record the fact that the call was made to an external function. This allows the unit test to verify expectations about what external calls the function should have attempted to make, and what parameters should have been passed.

Having good stubs is essential for unit testing a distributed system because it allows unit tests to be run while disabling or simulating any external interactions with other services. Here is a list of tools that can be used to help you develop stubs in various runtime languages:

### Node.js / JavaScript <a href="b6a8" id="b6a8"></a>

* [sinon.js](http://sinonjs.org) (Supplies both stub and spy functionality that makes it easy to build test assertions)
* [testdouble.js](https://github.com/testdouble/testdouble.js) (Stub generator with a focus on expressive object oriented API)
* [nock](https://github.com/node-nock/nock) (A library that is focused on making it easy to stub out HTTP request behavior)

### Python <a href="3a84" id="3a84"></a>

* [mock](https://pypi.python.org/pypi/mock)

### Go <a href="d1e4" id="d1e4"></a>

* [gomock](https://github.com/golang/mock)

### Java <a href="4d24" id="4d24"></a>

* [mockito](http://site.mockito.org)
* [easymock](http://easymock.org)

If you have more suggestions for test stub / mock frameworks to add to this list please comment below this article, and I’ll add on to the list.

## Unit Test Workflow <a href="5486" id="5486"></a>

The goal of unit tests is to give developers a way to rapidly verify the behavior of their code in between edits. Because the unit test are just making function calls to exercise logic flows while all external dependencies are stubbed out it is common to be able to execute and verify thousands of unit tests within a few seconds. The speed of these tests allows developers to run tests as part of their coding workflow, either directly integrated into their IDE, or by having a terminal to the side of their editor. By frequently rerunning the unit tests while editing code developers are able to identify potential issues before they even finish writing the code.

Once developers are in the habit of running and rerunning tests as they write code it is a small step up to test driven development. In this strategy developers actually write new test expectations prior to writing a new feature. The new test fails initially, until the new feature is added. The full functionality of the feature is built up as a process of repeatedly adding a failing test, then adding new feature code, then rerunning the failing test to verify that it now passes.

The role of unit tests goes beyond code development. They should also be integrated into the code merge process. Github supports status checks that work with all mainstream continuous integration servers. A common process is to make the “master” branch of your git repository a protected branch. Developers can’t commit directly to the master branch. Instead they make working commits on a feature branch forked from master. When it comes time to merge the feature branch back into the master branch Github can require that status checks pass before code can be merged.

The status checks are hooked up to providers such as [Jenkins](https://jenkins.io), [CircleCI](https://circleci.com), or [TravisCI](https://travis-ci.org), which take the code from that branch and run unit tests against it. If status checks succeed then merging is enabled, but if status checks fail Github will not allow the failing branch to be merged.

![](<../../.gitbook/assets/image (45).png>)

## Conclusion <a href="9f2b" id="9f2b"></a>

Unit tests are an important tool in the testing toolbox. In order to thoroughly unit test code that is part of a distributed system it is necessary to utilize a test framework that supports stubs that allow simulation of error conditions and other arbitrary responses from external dependencies. This allows unit tests to more thoroughly cover all the types of edge conditions that could be introduced by external components.

The next article of this series will focus on the next layer of the test pyramid: integration tests. It will also explain how to use docker to effectively test the external expectations for a service in a distributed environment.
