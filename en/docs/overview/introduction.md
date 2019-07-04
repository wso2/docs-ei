# Introduction

Integration is at the heart of any digital transformation. By connecting different systems that make up your enterprise, you can build an organization that acts as one seamless digital system.

WSO2 Enterprise Integrator (EI) is an open source product that enables comprehensive integration for cloud native and container-native projects. WSO2 EI enables you to do the following:

- Optimize systems and resources
- Leverage the cloud
- Reuse legacy systems
- Create a connected ecosystem for both your customers and partners.
- Connect enterprise systems to one another
- Make data accessible accross the enterprise
- Provide intuitive and visual development tools for continous integration and continous development
- Help integration developers to create new services and assets

WSO2 EI comprises of profiles that offer different integration capabilities.

The **ESB profile** in WSO2 EI provides its fundamental services through an event-driven and standards-based messaging engine (the bus), which allows integration architects to exploit the value of messaging without writing code. This ESB profile is a step ahead of the previous releases of WSO2 Enterprise Service Bus, as it provides data integration capabilities within the same runtime. This eliminates the need to use a separate data services server for your integration processes.

The following diagram illustrates the message-flow architecture in the ESB profile of WSO2 EI, which is used for implementing integration flows.

![alt text](../images/ESB_architecture1.png "ESB architecture")

This shows how a request propagates to its actual endpoint through the ESB profile. Response handling is the reverse of this operation. Note that the components of the pipes are not in a specific order.

1. An application (client) sends a message to the ESB profile of WSO2 EI.

2. The message is picked up by a transport.

3. The transport sends the message through a message pipe, which handles quality of service aspects such as security. Internally, this pipe is the in-flow and out-flow of the Axis2 engine. The ESB profile can operate in two modes:
   * Mediating Messages - A single pipe is used.
   * Proxy Services - Separate pipes connecting the transport to different proxy services are used.

4. Both message transformation and routing can be considered as a single unit. As the diagram specifies, there is no clear separation between message transformation components and routing components. In the ESB profile of WSO2 EI, this is known as the mediation framework. Some transformations take place before the routing decision has been made while others take place after the routing decision. This is part of the Synapse implementation.

5. The message is injected to the separate pipes depending on the destinations. Here again, quality of service aspects of the messages are determined.

6. The transport layer takes care of the transport protocol transformations that are required before sending the message to the receiver application.

7. The message is sent to the receiver application.
