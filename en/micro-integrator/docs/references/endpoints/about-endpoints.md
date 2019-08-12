# Endpoints

An **endpoint** defines an external destination for an outgoing message
through WSO2 Enterprise Integrator. Typically, the endpoint is the
address of a proxy service, which acts as the front end to the actual
service. For example, the endpoint for the simple stock quote sample is
`         http:        ` `         //localhost        `
`         :9000        `
`         /services/SimpleStockQuoteService        ` .

**Named endpoints**

You can use the `         name        ` attribute to create a named
endpoint. You can reuse a named endpoint by referencing it in another
endpoint using the `         key        ` attribute. For example, if
there is an endpoint named *foo* , you can reference the *foo* endpoint
in any other endpoint where you want to use *foo* :

    <endpoint key="foo"/>

This approach allows you to reuse existing endpoints in multiple places.