# JMS MapMessage support

In the ESB Profile of WSO2 Enterprise Integrator (WSO2 EI), the JMS
transport supports producing and consuming JMS
[MapMessage](http://docs.oracle.com/javaee/1.4/api/javax/jms/MapMessage.html)
objects, which send a set of name/value pairs.

## Producing a MapMessage

To send a MapMessage from a proxy service or API to a queue, construct
an XML payload using the [PayloadFactor
mediator](https://docs.wso2.com/display/EI650/PayloadFactory+Mediator)
(or another method) in the following structure, and send it to a JMS
endpoint:

``` java
    <JMSMap xmlns="http://axis.apache.org/axis2/java/transports/jms/map-payload">
        <name1>value1</name1>
        <name2>value2</name2>
        <name3>value3</name3>
    </JMSMap>
```

The JMS sender will then produce the equivalent MapMessage object:

``` java
    MapMessage message = session.createMapMessage();           
    message.setString("name1", "value1");
    message.setString("name2", "value2");
    message.setString("name3", "value3");
```

## Consuming a MapMessage

When a proxy service receives a JMS MapMessage via a JMS broker, it will
convert it to an XML message like this:

``` java
    <JMSMap xmlns="http://axis.apache.org/axis2/java/transports/jms/map-payload">
        <name1>value1</name1>
        <name2>value2</name2>
        <name3>value3</name3>
    </JMSMap>
```
