# Recipient List Endpoint

A Recipient List endpoint can contain multiple child endpoints or member
elements. It routes cloned copies of messages to each child recipient.

This will assume that all immediate child endpoints are identical in
state (state is replicated) or state is not maintained at those
endpoints.

### XML Configuration

!!! info

You can configure the Recipient List endpoint using XML. Click on the
"Switch to source view" link in the "Recipient List Endpoint" page.


``` html/xml
    <recipientlist>
        <endpoint .../>+
    </recipientlist>
```

  
See the detailed information about the Endpoints properties in [Adding
an
Endpoint](Working-with-Endpoints-via-Tooling_119131929.html#WorkingwithEndpointsviaTooling-AddEndpoint)
.
