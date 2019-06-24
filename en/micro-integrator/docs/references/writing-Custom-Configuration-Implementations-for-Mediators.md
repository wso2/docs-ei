# Writing Custom Configuration Implementations for Mediators

You can write your own custom configurator for the
`         Mediator        ` implementation you write without relying on
the [Class Mediator](_Class_Mediator_) or Spring extension for its
initialization.

-   **The** `                     MediatorFactory                   `
    **implementation** - Defines how to digest a custom XML
    configuration element to be used to create and configure the custom
    mediator instance.
-   T **he**
    `                     MediatorSerializer                   `
    **implementation** - Defines how a configuration should be
    serialized back into an XML configuration.

The custom `         MediatorFactory        ` ,
`         MediatorSerializer        ` implementations and the mediator
class/es must be bundled as an OSGi bundle exporting these classes and
placed into the \< `         EI_HOME>/dropins        ` folder, so that
the Synapse runtime could find and load the definition.

A custom JAR file must bundle your classes implementing the
`         Mediator        ` interface,
`         MediatorSerializer        ` and the
`         MediatorFactory        ` interfaces. It should also contain
two text files named
`         org.apache.synapse.config.xml.MediatorFactory        ` and
`         org.apache.synapse.config.xml.MediatorSerializer        `
which will contain the fully qualified name(s) of your
`         MediatorFactory        ` and
`         MediatorSerializer        ` implementation classes. Any
dependencies should be made available through OSGi bundles in the same
plugins directory.

The `         MediatorFactory        ` interface listing, which you
should implement, is given below and its
`         getTagQName()        ` method must define the fully qualified
element of interest for custom configuration. The Synapse initialization
will call back to this `         MediatorFactory        ` instance
through the `         createMediator(OMElement elem)        ` method
passing in this XML element, so that an instance of the mediator could
be created utilizing the custom XML specification and returned.

##### [The MediatorFactory Interface](http://svn.apache.org/viewvc/synapse/trunk/java/modules/core/src/main/java/org/apache/synapse/config/xml/MediatorFactory.java?view=markup)

``` java
    package org.apache.synapse.config.xml;
    
    import ...
    
    /**
     * A mediator factory capable of creating an instance of a mediator through a given
     * XML should implement this interface
     */
    public interface MediatorFactory {
        /**
         * Creates an instance of the mediator using the OMElement
         * @param elem
         * @return the created mediator
         */
        public Mediator createMediator(OMElement elem);
    
        /**
         * The QName of this mediator element in the XML config
         * @return QName of the mediator element
         */
        public QName getTagQName();
    }
```

##### [The MediatorSerializer Interface](http://svn.apache.org/viewvc/synapse/trunk/java/modules/core/src/main/java/org/apache/synapse/config/xml/MediatorSerializer.java?view=markup)

``` java
    package org.apache.synapse.config.xml;
    
    import ...
    
    /**
     * Interface which should be implemented by mediator serializers. Does the
     * reverse of the MediatorFactory
     */
    public interface MediatorSerializer {
    
        /**
         * Return the XML representation of this mediator
         * @param m mediator to be serialized
         * @param parent the OMElement to which the serialization should be attached
         * @return the serialized mediator XML
         */
        public OMElement serializeMediator(OMElement parent, Mediator m);
    
        /**
         * Return the class name of the mediator which can be serialized
         * @return the class name
         */
        public String getMediatorClassName();
    }
```
