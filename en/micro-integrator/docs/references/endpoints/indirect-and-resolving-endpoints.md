# Indirect and Resolving Endpoints

Indirect and Resolving endpoints are endpoint configurations with a key
which refers to an existing endpoint.

### Indirect Endpoint

The **Indirect Endpoint** refers to an actual
[endpoint](_Working_with_Endpoints_) by a key. This endpoint fetches the
actual endpoint at runtime. Then it delegates the message sending to the
actual endpoint. When endpoints are stored in the
[registry](https://docs.wso2.com/display/ADMIN44x/Working+with+the+Registry)
and referred, this endpoint can be used. The `         key        ` is a
static value for this endpoint.

#### XML Configuration

``` html/xml
    <endpoint key="" name="" />
```

#### Example

In the following [Send
mediator](https://docs.wso2.com/display/EI650/Send+Mediator)
configuration, the `         PersonInfoEpr        ` key refers to a
specific endpoint saved in the registry.

``` xml
    <send>
        <endpoint key="PersonInfoEpr"/>
    </send>
```

  

### Resolving Endpoint

The **Resolving Endpoint** refers to an actual endpoint using a dynamic
key. The key is an XPath expression.

1\. The XPath is evaluated against the current message and key is
calculated at run time.  
2. Resolving endpoint fetches the actual endpoint using the calculated
key.  
3. Resolving endpoint delegates the message sending it to the actual
endpoint.

When endpoints are stored in the registry and referred, this endpoint
can be used.

!!! info

The XPath expression specified in a Resolving endpoint configuration
derives an existing [endpoint](_Working_with_Endpoints_) rather than the
URL of the endpoint to which the message is sent. To derive the endpoint
URL to which the message is sent via an XPath expression, use the
[Header
Mediator](https://docs.wso2.com/display/EI650/Header+Mediator#HeaderMediator-ToHeader)
.


#### XML Configuration

``` html/xml
    <endpoint name="" key-expression="" />
```

#### Example

In the following [Send
mediator](https://docs.wso2.com/display/EI650/Send+Mediator)
configuration, the endpoint to which the message is sent is determined
by the `         get-property('Mail')        ` expression.

``` xml
    <send>
        <endpoint key-expression="get-property('Mail')"/>
    </send>
```
