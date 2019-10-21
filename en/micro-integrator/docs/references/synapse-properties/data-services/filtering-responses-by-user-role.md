# Filtering Responses by User Role

When you work with data services, you can control access to sensitive
data for specific user roles. This facility is called **Role-based
content filtering** . It filters data where specific data sections are
only accessible to a given type of users.

## Define user role-based result filtering

When you work with data services, you can control access to sensitive
data for specific user roles. This facility is called **Role-based
content filtering** . It filters data where specific data sections are
only accessible to a given type of users.  

Follow the steps below to filter a data service according to a specific
user role.

..........

## Extend role-based filtering via a custom authorization provider

In the Micro Integrator, you can filter content to specific user roles by taking roles from
the primary user store of the server. However, this extension provides
the flexibility for you to develop data services by plugging in a
mechanism to provide those role details from any preferred external
source (e.g., third party identity provider, JWT token etc.). Hence, in
data integration scenarios where data needs to be filtered based on the
user who requests those data, follow the steps below to plug in a custom
authorization provider.

1.  Create a Java Project and create a Java class (e.g.
    `           SampleAuthProvider          ` ), which extends the
    `           org.wso2.carbon.dataservices.core.auth.AuthorizationProvider          `
    interface and the below methods.

    | Method                                                                           | Description                                                                                                                                                                                                                                                                    |
    |----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
    | `               String getUsername(MessageContext msgContext)              `     | This should return the user name (i.e., from the message context, which contains all the HTTP request details.)                                                                                                                                                                |
    | `               String[] getUserRoles(MessageContext msgContext)              `  | This should return the roles of the user returned from the `               getUsername              ` method (e.g., this can be extracted from a JWT or a third party Identity provider etc.)                                                                                  |
    | `               String[] getAllRoles()              `                            | This should return all the roles of the user store, so that you can select which role needs to see which data etc. in the UI when creating the data service.                                                                                                                   |
    | `               void init(Map<String, String> authorizationProps)              ` | This initializes the auth provider (e.g., if you are using a third party identity provider to retrieve roles, you can pass the required parameters such as endpoint URLs, tokens etc. to the provider through this method and do required initializations within this method.) |

    **SampleAuthProvider Class**

    ```java
    public class SampleAuthProvider implements AuthorizationProvider {
        public String[] getUserRoles(MessageContext messageContext) throws DataServiceFault {
            String[] roles = {"user", "manager"};
            return roles;
        }    

        public String[] getAllRoles() throws DataServiceFault {
            String[] roles = {"admin", "client", "user", "manager"};
            return roles;
        }

        public String getUsername(MessageContext messageContext) throws DataServiceFault {
            return "saman";
        }

        public void init(Map<String, String> map) throws DataServiceFault {

        }
    }
    ```

2.  Build the project and place the JAR file in the
    `           <MI_HOME>/lib/          ` directory.

3.  Create the data service.

    !!! Tip
        When creating the data service;
        -   when creating the datasource, enter the **Authorization Provider
            Class** that you created above.     
        -   when adding the output mapping, select the user roles out of the
            ones you defined when creating the Java class.   

When you invoke the data service you created, you will view a response
as shown in the example below.

!!! Info
    Since the sample Java class above returns hard-coded `         “{“user”, “manager”}”        ` roles, the response below returns only the rows those roles can view.
    ```xml
    <Persons xmlns="https://localhost:9443/carbon/ds/addQuery.jsp?queryId=query1">
       <Person>
          <PersonID>4</PersonID>
          <FirstName>john</FirstName>
          <Address>12, seren street, TN</Address>
       </Person>
       <Person>
          <PersonID>1</PersonID>
          <FirstName>Tom</FirstName>
          <Address>34, baker str, London</Address>
       </Person>
       <Person>
          <PersonID>2</PersonID>
          <FirstName>Jack</FirstName>
          <Address>324, Vale str, PN</Address>
       </Person>
       <Person>
          <PersonID>3</PersonID>
          <FirstName>Allan</FirstName>
          <Address>23, St str, NW</Address>
       </Person>
    </Persons>
    ```

You can extend this functionality to extract required roles from the JWT
tokens, or invoke third-party identity providers to fetch roles etc. to
do role-based filtering in data services.
