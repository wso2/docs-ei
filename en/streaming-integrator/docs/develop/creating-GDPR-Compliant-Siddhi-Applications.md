# Creating GDPR Compliant Siddhi Applications

The obfuscation/removal of such PII (Personally Identifiable
Information) can be handled in WSO2 Stream Processor by Siddhi
Applications that can either modify or remove records that contain the
PII. These Siddhi Applications can be written in  a way to match the
original queries that captured data for persistence so that the same
data can be modified or removed as required. For more information about
writing Siddhi Queries, see [Siddhi Query
Guide](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/)
[.](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/)

The following sections explain how obfuscation/deletion of sensitive
data can be managed via Siddhi queries in a custom Siddhi application
developed based on a specific user case.

-   [Obfuscating
    PII](#CreatingGDPRCompliantSiddhiApplications-ObfuscatingPII)
-   [Deleting PII](#CreatingGDPRCompliantSiddhiApplications-DeletingPII)

## Obfuscating PII

Let's consider a Siddhi application that includes the following store
definition to persist streaming data.

    define table customerTable (customerId string, customerName string, entryVector int);

In this example, the customer ID is considered PII, and a customer with
the `         XXX        ` ID wants that ID to be hidden in the system
so that he/she cannot be personally identified with it. Therefore, you
need to obfuscate the value for the `         customerId        `
attribute. This can be done by creating an algorithm to create a hashed
value or a pseudonym to replace a specific value for the
`         customerId        ` attribute.

Let's consider that such an algorithm exists (e.g., as a function named
`         anonymize        ` ). To invoke this function, you need to add
a new query to the Siddhi application as shown in the sample below.

`           define table customerTable (customerId string, customerName string, entryVector int);          `

`           define stream UpdateStream (customerId string);          `

  

`           from UpdateStream          `

`           select *          `

**`            update customerTable           `**

**`            set customerTable.customerName = anonymize(customerTable.customerName)           `**

**`            on customerTable.customerId == XXX;           `**

  

In the above Siddhi application, the query in bold is triggered when a
new event is received in the `         UpdateStream        ` stream
where the value for the `         customerId        ` attribute is XXX.
 Once it is triggered, the `         XXX        ` customer ID is
replaced with a pseudonym.

For more information about writing custom functions, see [Siddhi Query
Guide - Writing Custom
Extensions](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#writing-custom-extensions)
.

## Deleting PII

Let's assume that the customer ID in the scenario described above needs
to be deleted. To do this, you can write a Siddhi query to delete the
value for the `         customerId        ` attribute when is equal to
`         XXX        ` as shown below.

`           define table customerTable (customerId string, customerName string, entryVector int);          `

`           define stream DeleteStream (customerId string);          `

  

**`            from DeleteStream           `**

**`            delete customerTable           `**

**`            on customerTable.customerId == customerId;           `**

`                     `

In the above Siddhi application, the query in bold is triggered when a
new event is received in the `         DeleteStream        ` stream
where the value for the `         customerId        ` attribute is
XXX. Once it is triggered, the `         XXX        ` customer ID is
deleted.

For more information about the Delete operator used here, see [Siddhi
Query Guide -
Delete](https://siddhi-io.github.io/siddhi/documentation/siddhi-4.x/query-guide-4.x/#delete)
.

  
