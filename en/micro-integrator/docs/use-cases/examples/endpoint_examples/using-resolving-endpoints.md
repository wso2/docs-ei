# Using Resolving Endpoints

In the following [Send
mediator](https://docs.wso2.com/display/EI650/Send+Mediator)
configuration, the endpoint to which the message is sent is determined
by the `         get-property('Mail')        ` expression.

```
<send>
  <endpoint key-expression="get-property('Mail')"/>
</send>
```
