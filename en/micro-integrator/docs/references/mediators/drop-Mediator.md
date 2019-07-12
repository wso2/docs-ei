# Drop Mediator

The **Drop Mediator** stops the processing of the current message . This
mediator is useful for ensuring that the message is sent only once and
then dropped by the ESB profile . If you have any mediators defined
after the `         <drop/>        ` element, they will not be executed,
because `         <drop/>        ` is considered to be the end of the
message flow.

When the Drop mediator is within the `         In        ` sequence, it
sends an HTTP 202 Accepted response to the client when it stops the
message flow. When the Drop mediator is within the
`         Out        ` sequence before the Send mediator, no response is
sent to the client.

!!! info

The Drop mediator is a
[content-unaware](ESB-Mediators_119131045.html#ESBMediators-Content-awareness)
mediator.


------------------------------------------------------------------------

[Syntax](#DropMediator-Syntax) \|
[Configuration](#DropMediator-Configuration) \|
[Example](#DropMediator-Example)

------------------------------------------------------------------------

### Syntax

The drop token refers to a \< `         drop        ` \> element, which
is used to stop further processing of a message:

``` java
    <drop/>
```

### Configuration

As with other mediators, after adding the drop mediator to a sequence,
you can click its up and down arrows to move its location in the
sequence.

### Example

You can use the drop mediator for messages that do not meet the filter
criteria in case the client is waiting for a response to ensure the
message was received by the ESB profile . For example:

``` html/xml
    <definitions xmlns="http://ws.apache.org/ns/synapse">
      <sequence name="main">
         <in>
         <!-- filtering of messages with XPath and regex matches -->
           <filter source="get-property('To')" regex=".*/StockQuote.*">
              <then>
                 <send>
                    <endpoint>
                       <address uri="http://localhost:9000/services/SimpleStockQuoteService"/>
                   </endpoint>
                 </send>
              </then>
              <else>
                 <drop/>
              </else>
         </filter>
    ...
```

In this scenario, if the message doesn't meet the filter condition, it
is dropped, and the HTTP 202 Accepted response is sent to the client. If
you did not include the drop mediator, the client would not receive any
response.
