# Filtering Responses by User Role

When you work with data services, you can control access to sensitive
data for specific user roles. This facility is called **Role-based
content filtering**. It filters data where specific data sections are
only accessible to a given type of users.

## Define user role-based result filtering

Follow the steps below to filter a data service according to a specific user role.

1. [Secure the dataservice](../../../../develop/creating-artifacts/data-services/securing-data-services) using `UsernameToken` for user authentication.
2. Add `requiredRoles` attribute to the output mapping with the comma separated list of user roles.
    ```xml
    <query id="getEmployeesQuery" useConfig="datasource">
      <sql>select EmployeeNumber,FirstName,Email from Employees</sql>
      <result element="Elements" rowName="Element">
         <element column="EmployeeNumber" name="EmployeeNumber" requiredRoles="admin, role1" xsdType="string"/>
         <element column="FirstName" name="FirstName"/>
         <element column="Email" name="Email" requiredRoles="admin"/>
      </result>
    </query>
    ```
