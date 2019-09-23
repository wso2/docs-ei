# JDBC Message Store Properties

Listed below are the properties used for [creating a JDBC Message Store](../../../develop/creating-artifacts/creating-a-message-store.md).

## Required Properties

The following properties are required when [creating a JDBC Message Store](../../../develop/creating-artifacts/creating-a-message-store.md).

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Name</td>
    <td>
      The name of the message store.
    </td>
  </tr>
  <tr>
    <td>Database Table</td>
    <td>
      The name of the database table.
    </td>
  </tr>
  <tr>
    <td>Driver</td>
    <td>
      The class name of the database driver.
    </td>
  </tr>
  <tr>
    <td>URL</td>
    <td>
      The JDBC URL of the database that the data will be written to.
    </td>
  </tr>
  <tr>
    <td>User</td>
    <td>
      The user name used to connect to the database.
    </td>
  </tr>
  <tr>
    <td>Password</td>
    <td>
      The password used to connect to the database.
    </td>
  </tr>
</table>

## Connection Pool Properties

The syntax of the JDBC message store can be different depending on whether you connect to the database using a connection pool, or using a datasource. Given below are the connection pool properties:

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>store.jdbc.driver </td>
    <td>
      The class name of the database driver.
    </td>
  </tr>
  <tr>
    <td>store.jdbc.connection.url</td>
    <td>
      The database URL.
    </td>
  </tr>
  <tr>
    <td>store.jdbc.username </td>
    <td>
      The user name to access the database.
    </td>
  </tr>
  <tr>
    <td>store.jdbc.password</td>
    <td>
      The password to access the database.
    </td>
  </tr>
  <tr>
    <td>store.jdbc.table  </td>
    <td>
      Table name of the database.
    </td>
  </tr>
</table>

## External Datasource Properties

The syntax of the JDBC message store can be different depending on whether you connect to the database using a connection pool, or using a datasource. Given below are the external datasource properties:

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>store.jdbc.dsName</td>
    <td>
      The name of the datasource to be looked up.
    </td>
  </tr>
  <tr>
    <td>store.jdbc.table</td>
    <td>
      The table name of the database.
    </td>
  </tr>
</table>

## Internal Datasource Properties

To make sure that the datasource appears in the **Datasource Name** list, you need to expose it as a JNDI datasource.

<table>
  <tr>
    <th>Property</th>
    <th>Description</th>
  </tr>
  <tr>
    <td>Name</td>
    <td>The name for the message store.</td>
  </tr>
  <tr>
    <td>Database Table</td>
    <td>
      The name of the database table.
    </td>
  </tr>
  <tr>
    <td>Datasource Name</td>
    <td>
      The class name of the datasource.
    </td>
  </tr>
</table>