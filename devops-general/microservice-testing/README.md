---
description: Testing best practice
---

# Microservice Testing

Automated testing is one of the most important things that a software company needs in order to be successful. Properly tested code operates in a consistent and dependable manner which generates customer trust. A software system with no tests, or tests that aren’t thorough enough, will suffer from outages and regressions that disappoint customers.

Although best practices for testing a monolithic application have been fairly well established there are still challenges that arise when testing a distributed system such as a microservices architecture. Developers have to ensure that components have consistent internal behavior, while also ensuring that each individual component will integrate properly with other components. Distributed systems require additional tests for failure conditions such as networking errors that prevent one component from communicating with another component. For some distributed systems the overall size of the system makes it hard to run a full end to end test so developers need strategies that allow them to test portions of the overall architecture independently while still ensuring interoperability.

This article introduces some of the fundamentals concepts for creating thorough test coverage for distributed systems, and discusses how to build a testing mindset into the development process to ensure testing concepts are employed effectively.
