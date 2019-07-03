# Spring Mediator

!!! info

Note

Please note that this feature is deprecated.


  

The **Spring Mediator** exposes a spring bean as a mediator. The Spring
Mediator creates an instance of a mediator, which is managed by Spring.
This Spring bean must implement the `         Mediator        `
interface for it to act as a Mediator.

!!! info

Note the following:

-   Spring support in the ESB profile is based on Spring version 2.5.
-   The Spring mediator is a [content
    aware](ESB-Mediators_119131045.html#ESBMediators-Content-awareness)
    mediator.


------------------------------------------------------------------------

[Syntax](#SpringMediator-Syntax) \|
[Configuration](#SpringMediator-Configuration) \|
[Examples](#SpringMediator-Examples)

------------------------------------------------------------------------

### Syntax

``` java
    <spring:spring bean="exampleBean" key="string"/>
```

The attributes of the \< `         spring        ` \> element:

-   **key** - References the Spring ApplicationContext/Configuration
    (i.e. spring configuration XML) used for the bean. This
    `          key         ` can be a [registry key or local entry
    key](https://docs.wso2.com/display/EI650/Working+with+Local+Registry+Entries)
    .
-   **bean** - Is used for looking up a Spring bean from the spring
    Application Context. Therefore, a `          bean         ` with the
    same name must be in the given spring configuration. In addition,
    `          bean         ` must implement the
    `          Mediator         ` interface.

------------------------------------------------------------------------

### Configuration

These are the options for the Spring Mediator:

-   **Bean** - Is used for looking up a Spring bean from the spring
    Application Context.
-   **Key** - The
    [Registry](https://docs.wso2.com/display/EI650/Product+Administration#ProductAdministration-Configuringtheregistry)
    reference to the spring Application-Context/Configuration used for
    the bean. You can select it by clicking the "Configuration Registry"
    or "Governance Registry" links.

------------------------------------------------------------------------

### Examples

``` java
    <spring:spring bean="springtest" key="conf/sample/resources/spring/springsample.xml"/>
```

In the above configuration, the spring XML is in the registry and it can
be looked up using the registry key
`         conf/sample/resources/spring/springsample.xml        ` . This
spring XML (i.e `         springsample.xml        ` ) must contain a
bean with the name `         springtest        ` . The following figure
shows an example that can be used as the registry resource -
`         springsample.xml        ` .

``` java
    <!DOCTYPE beans PUBLIC "-//SPRING//DTD BEAN//EN" "http://www.springframework.org/dtd/spring-beans.dtd">
       <beans>
          <bean id="springtest" class="org.apache.synapse.mediators.spring.SpringTestBean" singleton="false">
             <property name="testProperty" value="100"/>
          </bean>
       </beans>
```

Also, you need to b uild the JAR file of the following Spring Bean class
and place it in the
`         <EI_HOME>/repository/components/lib/        ` directory.

``` java
    package org.apache.synapse.mediators.spring;
    
    import org.apache.synapse.MessageContext;
    import org.apache.synapse.mediators.AbstractMediator;
    
    public class SpringTestBean extends AbstractMediator{
       private String testProperty;
    
       public void setTestProperty(String testProperty){
          this.testProperty = testProperty;
       }
    
       public boolean mediate(MessageContext mc) {
                // Do somthing useful..
                // Note the access to the Synapse Message context
                return true;
       }
    }
```

For more examples, see [Mediating with
Spring](https://docs.wso2.com/display/EI650/Mediating+with+Spring) .
