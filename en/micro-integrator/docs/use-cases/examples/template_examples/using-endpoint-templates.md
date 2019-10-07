# Using Endpoint Templates
## Example use case

## Synapse configuration
For example, let's say we have two default endpoints with following hypothetical configurations:

```xml tab='Endpoint 1'
<endpoint  name="ep1">
  <default>
    <suspendOnFailure>
      <errorCodes>10001,10002</errorCodes>
      <progressionFactor>1.0</progressionFactor>
    </suspendOnFailure>
    <markForSuspension>
      <retriesBeforeSuspension>5</retriesBeforeSuspension>
      <retryDelay>0</retryDelay>
    </markForSuspension>
  </default>
</endpoint>
```

```xml tab='Endpoint 2'
<endpoint  name="ep2">
  <default>
    <suspendOnFailure>
      <errorCodes>10001,10003</errorCodes>
      <progressionFactor>2.0</progressionFactor>
    </suspendOnFailure>
    <markForSuspension>
      <retriesBeforeSuspension>3</retriesBeforeSuspension>
      <retryDelay>0</retryDelay>
    </markForSuspension>
  </default>
</endpoint>
```

We can see that these two endpoints have different set of error codes and different progression factors for suspension. Furthermore, the number of retries is different between them. By defining an endpoint template, these two endpoints can be converged to a generalized form. This is illustrated in the following:

```
<template name="ep_template">
  <parameter name="codes"/>
  <parameter name="factor"/>
  <parameter name="retries"/>
  <endpoint name="$name">
    <default>
        <suspendOnFailure>
          <errorCodes>$codes</errorCodes>
          <progressionFactor>$factor</progressionFactor>
        </suspendOnFailure>
        <markForSuspension>
          <retriesBeforeSuspension>$retries</retriesBeforeSuspension>
          <retryDelay>0</retryDelay>
        </markForSuspension>
    </default>
  </endpoint>
</template>
```

!!! Note
    - The Endpoint template uses parameters as inputs. These parameters are then refered using the `$` prefix within the template. Unlike sequence templates, endpoint templates are always parameterized using `$` prefixed values (not XPath expressions). For example, a parameter named `codes` can be referred by `$codes`.
    - `$name` and `$uri` are default parameters that a template can use anywhere within the endpoint template (usually as parameters for endpoint name and address attributes).

Since we have a template defined, we can use template endpoints to create two concrete endpoint instances with different parameter values for this scenario. This is shown below.

``` java tab='Endpoint 1'
<endpoint name="ep1" template="ep_template">
  <parameter name="codes" value="10001,10002" />
  <parameter name="factor" value="1.0" />
  <parameter name="retries" value="5" />
</endpoint>
```

``` java tab='Endpoint 2'
<endpoint name="ep2" template="ep_template">
  <parameter name="codes" value="10001,10003" />
  <parameter name="factor" value="2.0" />
  <parameter name="retries" value="3" />
</endpoint>
```

## Build and run

Create the artifacts:

1. Set up WSO2 Integration Studio.
2. Create an ESB Config project
3. Create the mediation artifacts with the above configuration.
4. Deploy the artifacts in your Micro Integrator.

Set up the back-end service:

........

Invoke the service: