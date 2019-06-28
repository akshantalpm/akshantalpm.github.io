---
layout: post
title: Modular Monolith - A step towards Microservice
---
<link rel="stylesheet" href="https://gist-assets.github.com/assets/embed-b67021dc07195830cc157f7720b938fb.css">

Today Microservices are everywhere. It’s an architectural style that structures an application as a collection of services that are organized around business capabilities, independently deployable, loosely coupled and easily scalable.

One of the biggest challenges while designing microservice is identifying the boundaries, right set of bounded context. Even experienced architects with good domain knowledge have difficulty in identifying boundaries at the beginning. Choosing microservice boundaries is an architecturally significant decision with costly ramifications when done wrong. Where as Module boundaries in a modular application are easier to change. Refactoring across modules is typically supported by the type-system and the compiler. Modular monolith helps you figure out the right set of boundaries and acts as a gateway towards microservice architecture.

Modular monolith, as the name suggests is a collection of modules, a set of loosely coupled, well encapsulated modular components with well-defined interfaces. We don’t have to deploy a module as a service until we need to scale it. Modules can be deployed as libraries, plugins etc., resulting into one file that can be very easily deployed, monitored and debugged. As long as we follow a good modular structure inside the monolith, we can easily extract a service.

--------------

### Designing Module

Modularity in software development can be boiled down into three guiding principles:
* Strong encapsulation: hide implementation details inside components, leading to low coupling between different parts. Teams can work in isolation on decoupled parts of the system.
* Well-defined interfaces: well-defined and stable APIs between components are a must. A component can be replaced by any implementation that conforms to the interface specification.
* Explicit dependencies: having a modular system means distinct components must work together. You'd better have a good way of expressing (and verifying) their relationships.

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


 
