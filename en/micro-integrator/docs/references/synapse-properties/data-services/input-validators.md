# Input Validators

Validators are added to individual input mappings in a query. Input
validation allows data services to validate the input parameters in a
request and stop the execution of the request if the input doesn’t meet
the required criteria. The ESB profile of WSO2 EI provides a set of
built-in validators for some of the most common use cases. It also
provides an extension mechanism to write custom validators.

## Long Range validator

Validates if an integer value is in the specified range. The validator
requires a minimum and a maximum value to set the range. 

## Double Range validator

Validates if a floating point is in the specified range. The validator
requires a minimum and a maximum value to set the range. 

## Length validator

Validates the string length of a given parameter against a specified
length.

## Pattern validator

Validates the string value of the parameter against a given regular
expression.

## Custom validators

Used to add your own validation logic by implementing the interface
`         org.wso2.carbon.dataservices.core.validation.Validator        `
. The definition of the interface is as follows:

```java
public interface Validator {
   public void validate(ValidationContext context, String name, ParamValue value) throws ValidationException;
}
```

If the validation fails, the validate method in the interface by default
throws an exception of type `         ValidationException        ` . The
parameters of the method are as follows:

-   **context** : Is of type `          ValidationContext         ` ,
    which contains information about the full set of parameters passed
    into the request. When the validation logic depends on other
    parameters, the validation context can be used to check the
    names/values of the rest of the parameters.
-   **name** : A string value that represents the name of the parameter
    to be validated.
-   **value** : Is of type `          ParamType         ` , which
    represents the value of the parameter to be validated. It is either
    `          SCALAR         ` or `          ARRAY         ` .

If you need to provide properties when initializing the custom
validator, it is necessary to implement the
`         org.wso2.carbon.dataservices.core.validation.ValidatorExt        `
interface. This extends the Validator interface as shown below.

```xml
public interface ValidatorExt extends Validator {
    public void init(Map<String, String> props);
}
```

The init method initializes the set of properties provided for the
custom validator.

After creating a custom validator class, package it in a JAR file and
store it in the server's classpath location for external libraries
(which is the `         <MI_HOME>/lib        ` directory)
