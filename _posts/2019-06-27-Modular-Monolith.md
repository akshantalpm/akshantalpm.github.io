---
layout: post
title: Modular Monolith - A Step Towards Microservice
---
<link rel="stylesheet" href="https://gist-assets.github.com/assets/embed-b67021dc07195830cc157f7720b938fb.css">

Let’s start with Monolithic Architectures, a traditional way of designing systems/applications. In software engineering, a monolithic application describes a software application which is designed without modularity, tightly coupled application. A single tier application which contains code for User Interface, Business Logic, Database Access, External Integrations, etc. All business domain residing in one single repository.

Monolith applications are simple to develop, deploy and test but it also comes up with drawbacks. Some of them are
* Maintenance — If Application is too large and complex to understand entirely, it is challenging to make changes fast and correctly.
* Scaling - Monolithic applications can also be challenging to scale when different modules have conflicting resource requirements.
* Adaption to new technologies - Monolithic applications have difficulty to adopting new and advance technologies. Since changes in languages or frameworks affect an entire application.

When monolithic applications become unmaintainable, organisations see Microservices as a silver bullet. And that is a problem.

Today Microservices are everywhere. It’s an architectural style that structures an application as a collection of services that are organized around business capabilities, independently deployable, loosely coupled and easily scalable.

With enumerate advantage of using microservices there are also few drawbacks, one of them being deployment.  Deploying one monolith is very different from deploying different microservices. Monitoring and debugging them becomes a difficult job and adds additional cost and complexity.

The other biggest challenge while designing microservice is identifying the boundaries, right set of bounded context. Even experienced architects with good domain knowledge have difficulty in identifying boundaries at the beginning. Choosing microservice boundaries is an architecturally significant decision with costly ramifications when done wrong. Where as Module boundaries in a modular application are easier to change. Refactoring across modules is typically supported by the type-system and the compiler. Modular monolith helps you figure out the right set of boundaries and acts as a gateway towards microservice architecture.

Modular monolith, as the name suggests is a collection of modules, a set of loosely coupled, well encapsulated modular components with well-defined interfaces. Each module has one responsibility. The domain is distributed across different modules handling specific business functionality.

In modular applications, code is organized independently and can be easily swapped in or out. You have more control of your codebase. Spread out distribution of code allows for better maintainability in the long run. You can adopt different frameworks for different modules as per your needs.

```When you choose a framework, you make a large, long term commitment. You sign up to learn about the framework’s various inner workings and strange behaviors. You also sign up to a period of ineffectiveness whilst you’re getting to grips with things. If the framework turns out to be the wrong bet, you lose a lot. But if you pick and choose from libraries, you can afford to replace one part of your front end stack whilst retaining the rest. — Jimmy Breck-McKye, The State of JavaScript in 2015```

Similarly, while working with modular monolith if you want to improve piece of code it can be easily done without breaking other modules. If you want to try out different frameworks that can be done easily.

As compared to microservices, we don’t have to deploy a module as a service until we need to scale it. Modules can be deployed as libraries, plugins etc., resulting into one file that can be very easily deployed, monitored and debugged. As long as we follow a good modular structure inside the monolith, we can easily extract a service.

The typical downside of modular monolith is that it requires discipline from developers. It's very easy to use classes and methods of different modules that are not the entry points to the module, so discipline of using only the entry point to module is required.
  

--------------

### Designing Module

Modularity in software development can be boiled down into three guiding principles:
* Strong encapsulation: hide implementation details inside components, leading to low coupling between different parts. Teams can work in isolation on decoupled parts of the system.
* Well-defined interfaces: well-defined and stable APIs between components are a must. A component can be replaced by any implementation that conforms to the interface specification.
* Explicit dependencies: having a modular system means distinct components must work together. You'd better have a good way of expressing (and verifying) their relationships.

Each module created in the application has it's own domain models and database tables which makes it absolutely independent of the other modules and it hides the details of the internal implementation.
If you share the models or tables then you are coupling your modules and will loose the greatest benefit of teams working on them independently without worrying about how other module evolves. 

Modules are now part of the programming languages and platforms as a first-class construct. Even the Java platform itself has been modularized using the new Java module system. Many module systems allow you to express your dependencies on other modules. When these dependencies are violated, the module system will not allow it.

---------------

### Creating our application

Let’s start by creating a simple application using Play framework. There is no strict definition in Play of what a module is or isn’t - a module could be just a library that provides some helper methods to help you do something, or it could be a full framework providing complex functionality such as user management.
After creating new play project called **e-commerce** the project will have following layout

```
    e-commerce
     |___ app
     |___ conf
     |___ it
     |___ project
     |___ test
     |___ build.sbt
     |___ Module.scala
```
Any e-commerce application has standard features like payment, search, reviews-ratings, etc. We can develop them as different modules.
The common code required for different modules can be extracted as a library.
Module can either be a Play project or a simple sbt project depending on your requirement. 

```
 e-commerce
     |___ app
     |___ conf
     |___ it
     |___ project
     |___ test
     |___ build.sbt
     |___ modules
            |____ common
            |       |___ src
            |             |___ main
            |                   |___ java
            |                   |___ scala
            |             |___ test
            |                   |___ java
            |                   |___ scala
            |____ payment
            |       |___ app
            |       |___ conf
            |       |___ it
            |       |___ project
            |       |___ test
            |       |___ build.sbt
            |       |___ PaymentModule.scala
            |
            |____ search
                    |___ app
                    |___ conf
                    |___ it
                    |___ project
                    |___ test
                    |___ build.sbt
                    |___ SearchModule.scala
``` 

You can make your application depend on a module or a library project. Just add another sbt project definition in your build file:

<script src="https://gist.github.com/akshantalpm/2e265fc86b487c3a858af4a8d0c88d05.js"></script>

To experiment with this yourself, I encourage you to clone my [GitHub project](https://github.com/akshantalpm/e-commerce).


 
