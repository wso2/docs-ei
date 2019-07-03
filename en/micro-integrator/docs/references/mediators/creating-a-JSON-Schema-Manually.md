# Creating a JSON Schema Manually

You need to have two input and output JSON Schema files for a [Data
Mapping configuration](_Data_Mapper_Mediator_) as shown in the example
below.

![example mapping
configuration](attachments/119131355/119131378.png "example mapping configuration")  

Therefore, you can use the WSO2 Data Mapper Diagram Editor to create a
JSON Schema manually by adding elements to the Data Mapper tree view as
explained below.

-   [Components of a JSON
    Schema](#CreatingaJSONSchemaManually-ComponentsofaJSONSchema)
-   [Adding the root
    element](#CreatingaJSONSchemaManually-Addingtherootelement)
-   [Adding a child
    element](#CreatingaJSONSchemaManually-Addingachildelement)
-   [Setting a nullable
    element](#CreatingaJSONSchemaManually-Settinganullableelement)
-   [Editing an element](#CreatingaJSONSchemaManually-Editinganelement)
-   [Deleting an
    element](#CreatingaJSONSchemaManually-Deletinganelement)

### Components of a JSON Schema

There are the following four types of components in a JSON Schema:

-   Arrays
-   Objects
-   Fields
-   Attributes

### Adding the root element

When creating the tree you need to first add the root element. The root
element can be either an object or an array. Right-click on the
**Input** or **Output** box and then click **Add new Root Element** as
shown below, to add the root element.

![select add new root element
option](attachments/119131355/119131377.png "select add new root element option")

Add the following details to create the root element.

-   **Name:** name of the root element
-   **Schema Type:** type of the element (i.e. array or object)
-   **ID:** ID of the root element to uniquely identify it
-   **isNullable:** whether the element can be a nullable (i.e. not
    available in the payload (optional)  
    **Namespaces:** prefix and URL of the namespace (optional)
-   **Required:** child elements required to be there in the payload
    (optional)
-   **Schema Value:** custom URI of the Schema

![enter details of the root
element](attachments/119131355/119131359.png "enter details of the root element"){width="600"}

You view the root element added to the **Input** box as shown below.

![](attachments/119131355/119131366.png)  

### Adding a child element

You can add Arrays, Objects, Fields and Attributes as child elements as
explained below.

-   [**Adding an Object as a Child
    element**](#cb656408b4eb42c3b5a202a7dc8b902c)
-   [**Adding an Array as a child
    element**](#1566612f37594a7eb1df1f04758fda97)
-   [**Adding a Field as a child
    element**](#7a2f3fdb94394560b37584f62ddf7e01)
-   [**Adding an Attribute as a child
    element**](#41c388262ba148c2b75b72cd6bccdd0e)

Right-click on the parent element and click **Add new Object** , to add
an Object as shown below.

![add an object as a child
element](attachments/119131355/119131373.png "add an object as a child element")

Add the following details to create an Object as a child element.

-   **Name:** name of the child element
-   **Schema Type:** type of the element (i.e. object)
-   **ID:** ID of the child element to uniquely identify it
-   **isNullable:** whether the element can be a nullable (i.e. not
    available in the payload (optional)  
    **Namespaces:** prefix and URL of the namespace (optional)
-   **Required:** child elements required to be there in the payload
    (optional)
-   **Schema Value:** custom URI of the Schema

        !!! info
    
        If the object has element identifiers, then select the checkbox and
        add the value, type, and URL of the identifier.
    

-   **Object holds a value:** if the object holds a value or not

        !!! info
    
        If the object holds a value, then select the checkbox and select the
        data type.
    

![details to add an object child
element](attachments/119131355/119131357.png "details to add an object child element")

You view the child Object element added to the root element as shown
below.

!!! info

The element identifier is added as an attribute to the element.


  

![child element added to Input
box](attachments/119131355/119131367.png "child element added to Input box")

Right-click on the parent element and click **Add new Array** , to add
an Array as shown below.

![adding an array as a child
element](attachments/119131355/119131375.png "adding an array as a child element")

You need to add the following details to create an Array as a child
element.

-   **Name:** name of the child element
-   **Schema Type:** type of the element (i.e. array)
-   **ID:** ID of the child element to uniquely identify it
-   **isNullable:** whether the element can be a nullable (i.e. not
    available in the payload (optional)  
    **Namespaces:** prefix and URL of the namespace (optional)
-   **Required:** child elements required to be there in the payload
    (optional)
-   **Schema Value:** custom URI of the Schema

        !!! info
    
        If the array has element identifiers, then select the checkbox and
        add the value, type, and URL of the identifier.
    

-   **Object holds a value:** if the object holds a value or not

        !!! info
    
        If the array holds a value, then select the checkbox and select the
        data type.
    

![entering details to add the Array child
element](attachments/119131355/119131356.png "entering details to add the Array child element"){width="600"}

You view the child Array element added to the root element as shown
below.

!!! info

The element identifier is added as an attribute to the element.


![](attachments/119131355/119131368.png)

Right-click on the parent element and click **Add new Field** , to add a
Field as shown below.

![add Field child
element](attachments/119131355/119131372.png "add Field child element")

You need to add the following details to create a Field as a child
element.

-   **Name:** name of the child element
-   **Schema Type:** type of the element
-   **ID:** ID of the child element to uniquely identify it
-   **isNullable:** whether the element can be a nullable (i.e. not
    available in the payload (optional)  
    **Namespaces:** prefix and URL of the namespace (optional)
-   **Required:** child elements required to be there in the payload
    (optional)
-   **Schema Value:** custom URI of the Schema

![entering details to add field child
element](attachments/119131355/119131365.png "entering details to add field child element"){width="600"}

You view the child Field element added to the root element as shown
below.

![added field
element](attachments/119131355/119131364.png "added field element")

Right-click on the parent element and click **Add new Attribute** , to
add an attribute as shown below.

![add Attribute child
element](attachments/119131355/119131371.png "add Attribute child element")

You need to add the following details to create an Attribute as a child
element.

-   **Name:** name of the child element
-   **Schema Type:** type of the element
-   **ID:** ID of the child element to uniquely identify it
-   **isNullable:** whether the element can be a nullable (i.e. not
    available in the payload (optional)  
    **Namespaces:** prefix and URL of the namespace (optional)
-   **Required:** child elements required to be there in the payload
    (optional)
-   **Schema Value:** custom URI of the Schema

![entering details to add attribute child
element](attachments/119131355/119131363.png "entering details to add attribute child element"){width="600"}

You view the child Attribute element added to the root element as shown
below.

![attribute added to root
element](attachments/119131355/119131362.png "attribute added to root element")

### Setting a nullable element

You can set an element as a nullable element, so that it is not required
to have that element in the payload. Right-click on the parent element
and click **Enable Nullable** , to enable an element to make it
nullable as shown below.

![enable nullable
element](attachments/119131355/119131361.png "enable nullable element")

Right-click on the parent element and click **Disable Nullable** , to
make it required in the payload as shown below.

![disable the element being
nullable](attachments/119131355/119131358.png "disable the element being nullable")

### Editing an element

Right-click on any element and click **Edit Object** , to edit and
update the field values as required as shown below.

![edit an
element](attachments/119131355/119131369.png "edit an element")

### Deleting an element

Right-click on any element and click **Delete from Model** , to delete
it as shown below.

!!! info

This deletes the element with its child nodes.


![deleting an
element](attachments/119131355/119131360.png "deleting an element")
