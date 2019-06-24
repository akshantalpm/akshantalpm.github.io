---
layout: post
title: Actor Model For IoT
---
<link rel="stylesheet" href="https://gist-assets.github.com/assets/embed-b67021dc07195830cc157f7720b938fb.css">

The Internet of Things (IoT) is composed of many nodes, often with limited capabilities. Small software components communicating via Internet protocol standards typically form a highly distributed workflow among machines with minimal human interaction. General application scenarios include sensors that monitors data such as environmental conditions. Complex applications are built using sensors and actuators, for example: home automation, and tracking of health data. These systems enable machines to upload data to Internet servers. Thus, they allow the tracking of data everywhere and anytime.

![Actor Model For IoT Image]({{ site.baseurl }}/images/actor-model-for-iot.png)

One of the main characteristics of a typical IoT system is that it involves a large number of managed devices, each of which consists of changing internal state. In many cases, these devices are primitive hardware running on some simple network protocol. Such "minimalist" requirements align well with the actor model in which breaking down business logic into minimal tasks for individual actors to handle is part of the model's underlying principle.

Actors have delivery guarantees and isolation properties that are perfect for the IoT world, making it a great tool for simulating millions of concurrently connected sensors producing real-time data. They are lightweight by design; hence they can scale without consuming excessive computing resources.

Following are the signature attributes of actors that make them suitable for IoT:

1. **Scalability**:
The Internet of Things adds a lot of challenges in how to deal with all the simultaneously connected devices producing lots of data to be retrieved, aggregated, analyzed and pushed back out the the devices while maintaining responsiveness. Challenges include managing huge bursts in traffic in receiving sensor data at peak times, processing of these large amounts of data in both batch processes and real-time, and doing massive simulations simulating real-world usage patterns. Some IoT deployments also require the back end services to manage the devices, not just absorb the data sent from the devices.

    The back-end systems managing all this needs to be able to scale on demand and be fully resilient. This is a perfect fit for reactive architectures in general and Akka in particular.

2. **Concurrency**:
   IoT application gateways are the points in a system that connect local sensors and actuators to the cloud. (Some call such gateways also concentrators, service gateways or clients.) Examples are roadside stations, onboard units during transport, or home automation gateways.

   Even if an application “only forwards data” between sensors, actuators and cloud services, several things are going on in parallel. IoT application gateways need to handle streams of events that happen in their environment and data that arrive at their interfaces. The environment produces data and requires output at its own speed. 

   Actor model deals with the above stated problem by enabling high performance concurrent processing of messages from devices via message passing. One of the advantages of message-processing models like actors is that the traditional concurrency problems (primarily synchronization of shared state) are no longer a problem. The actor can keep private state like device’s internal state or active sessions and update it freely without locks. The actor framework ensures that only one message is processed at a time.

   ![Actor Model For IoT Image]({{ site.baseurl }}/images/message-passing.png)

3. **Fault Tolerance**:
   While you are building services which will potentially be used by millions of connected devices, you need a model for coping with information flow. You need abstractions for what happens when devices fail, when information is lost and when services fail. Today, we often take call stacks for granted. But, they were invented in an era where concurrent programming was not as important because multi-CPU systems were not common. Call stacks do not cross threads and hence, do not model asynchronous call chains.

   ![Actor Model For IoT Image]({{ site.baseurl }}/images/error-handling.png)

   The above diagram shows a serious problem. How does the worker thread deal with the situation? It likely cannot fix the issue as it is usually oblivious of the purpose of the failed task. The “caller” thread needs to be notified somehow, but there is no call stack to unwind with an exception. Failure notification can only be done via a side-channel, for example putting an error code where the “caller” thread otherwise expects the result once ready. If this notification is not in place, the “caller” never gets notified of a failure and the task is lost! This is surprisingly similar to how networked systems work where messages/requests can get lost/fail without any notification.

   With Actors, we can organize actors into supervision hierarchy, so, errors in an individual actors don’t bring entire system down.

   ![Actor Model For IoT Image]({{ site.baseurl }}/images/hierarchy.png)

4. **LightWeight**:
   Benchmarking suggests that Akka model can handle 2.5 million actors per gigabyte of heap memory and can process 50 million messages per second on a single machine.

5. **Network Protocol Decoupling**:  With the actor model we can take advantage of fault tolerance to decouple the actor representing the device from the underlying communication protocol. In this way actor representing the device and the device state can be separated from the actor representing the communication protocol to shield the device actor from the network errors and increase the functional cohesiveness of the individual actors

   ![Actor Model For IoT Image]({{ site.baseurl }}/images/protocols.png)

6. **Non-blocking communications**: Internet of Thing applications being reactive and asynchronous is a "must". Most IoT application should be able to handle many connections from devices and all the messages ingested from them.

   Asynchronous messaging is widely used when it comes to machine to machine communication. It means that the sender does not expect an immediate response and that the sender not "block" anything while waiting for the response.Asynchronous communication enables flexibility: An application can send a message out and after that, just keep working on other things

   Actors are uniquely addressable, and have their own independent mailboxes or message queues. They supports non-blocking communications via message passing which makes them suitable for building non blocking and distributed computing systems.

7. **Customization**: All actors have a well-defined lifecycle with refined hooks such as preStart(), postRestart(), and postStop() for lifecycle logic control. In the case of simulating an IoT device, one can easily anchor custom initialization and termination routines to the appropriate hooks.

    <script src="https://gist.github.com/akshantalpm/b0088ca0582333bbd3a22af6e52735f8.js"></script>
    
   The above snippet shows how one might construct a device actor in Scala/Akka that publishes its operational state information to subscribers using the industry standard MQTT (Message Queue Telemetry Transport) publish-subscribe messaging protocol. The intent here isn't to study how to program in Scala or Akka, but to provide a simple example of an Akka actor's easy-to-understand logic flow.

--------------

## Case Studies:

* ### ThingsBoard:
    ThingsBoard is an open-source IoT platform for data collection, processing, visualization, and device management. 

    Being robust, scalable and user friendly, ThingsBoard IoT platform supports various IoT use cases by providing flexible and powerful out-of-the-box features to cut down time to market of your connected products and smart solutions. The platform is device-agnostic, so you can feed and analyze telemetry data from any sensor, connected device or application. ThingsBoard comprehensive features and rich platform APIs allow you to save time and resources on routine IoT tasks and concentrate on specific features of your IoT solution.

    ThingsBoard uses Akka as an actor system implementation, it enables high performance concurrent processing of messages from devices as long as server-side API calls.

    ![Actor Model For IoT Image]({{ site.baseurl }}/images/things-board.png)

    The brief description of each actor’s functionality is listed below:
    * App Actor - responsible for management of tenant actors. An instance of this actor is always present in memory.
    * Tenant Actor - responsible for management of tenant device & rule chain actors. An instance of this actor is always present in memory.
    * Device Actor - maintain state of the device: active sessions, subscriptions, pending RPC commands, etc. Caches current device attributes in memory for performance reasons. An actor is created when the first message from the device is processed. The actor is stopped when there is no messages from devices for a certain time.
    * Rule Chain Actor - process incoming messages, persist them into queue and dispatches them to rule node actors. An instance of this actor is always present in memory.
    * Rule Node Actor - process incoming messages, and report results back to rule chain actor. An instance of this actor is always present in memory.
    * Device Session Manager Actor - responsible for management of device session actors. Creates session actors on a first message with the corresponding session id. Closes session actors when the corresponding session is closed.
    * Session Actor - represents a communication session between a device and ThingsBoard server. Sessions may be synchronous (HTTP, CoAP) and asynchronous (MQTT, CoAP with Observe option).
    * RPC Session Manager Actor - responsible for management of cluster RPC session actors. Creates session actor when a new server is up. Closes session actor when server is down.
    * RPC Session Actor - represents a communication session between two ThingsBoard servers in the cluster mode. Communication is done using HTTP/2 based on gRPC

* ### TMT
    The Thirty Meter Telescope, it’s a telescope that will allow us to see deeper into space and observe cosmic objects with unprecedented sensitivity. TMT uses Akka as an actor system implementation:

    ![Actor Model For IoT Image]({{ site.baseurl }}/images/tmt.png)

    The brief description of each actor’s functionality is listed below:
    * Supervisor Actor - Supervisor actor is the actor first started for any component. The main responsibilities that supervisor performs is as follows:
        * Implement and manage the component lifecycle for the Device Actor and for the rest of the system.
        * Register itself with location service.
        * Provide an administrative interface to the component to the rest of the system. For instance, the Container can perform certain administrative communication with the supervisor such as restart or shutdown.
        * Allow components outside of the supervisor and Device Actor to monitor the lifecycle state of the Device Actor. This is particularly useful for testing. The test needs to know that the component is ready before it starts its test actions.
    * Device Actor - While the supervisor Actor works as the external interface for the component and the manager of Lifecycle, the functional implementation of a component is implemented in a Device Actor, spawned by supervisor actor for any component. 
    * Command Response Manager - The Command Response Manager is responsible for managing and bookkeeping the command status of long running submit commands. The sender of the command (and any component, really) can query the command statuses or subscribe to changes in command statuses.
    * PubSub Manger - A PubSub Manager is used when a component might need to subscribe to the current state of any other component provided it knows the location of that component. 

----------------

## References:
* https://readwrite.com/2014/07/10/akka-jonas-boner-concurrency-distributed-computing-internet-of-things/
