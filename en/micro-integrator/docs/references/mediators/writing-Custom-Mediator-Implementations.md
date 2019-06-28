# Writing Custom Mediator Implementations

The following information concerning writing custom mediators
implementations is available:

-   [MessageContext
    Interface](#WritingCustomMediatorImplementations-MessageContextInterface)
-   [Mediator
    Interface](#WritingCustomMediatorImplementations-MediatorInterface)
-   [Leaf and Node Mediators, List Mediators and Filter
    Mediators](#WritingCustomMediatorImplementations-LeafandNodeMediators,ListMediatorsandFilterMediators)

### MessageContext Interface

The
[`          MessageContext         `](http://synapse.apache.org/apidocs/org/apache/synapse/MessageContext.html)
interface is the primary interface of the Synapse API. This essentially
defines the per-message context passed through the chain of mediators,
for each and every message received and processed by Synapse. Each
message instance is wrapped within a `         MessageContext        `
instance, and the message context is set with the references to the
[`          SynapseConfiguration         `](http://synapse.apache.org/apidocs/org/apache/synapse/config/SynapseConfiguration.html)
and
[`          SynapseEnvironment         `](http://synapse.apache.org/apidocs/org/apache/synapse/core/SynapseEnvironment.html)
.

The `         SynapseConfiguration        ` holds the global
configuration model that defines mediation rules, local registry entries
and other configuration.

The `         SynapseEnvironment        ` gives access to the underlying
SOAP implementation used - Apache Axis2.

A typical mediator would need to manipulate the
`         MessageContext        ` by referring to the
`         SynapseConfiguration        ` .

!!! info

Note

It is strongly recommended that the
`         SynapseConfiguration        ` is not updated by mediator
instances as it is shared by all messages and may be updated by Synapse
administration or configuration modules.


Mediator instances may store local message properties into the
`         MessageContext        ` for later retrieval by successive
mediators.

!!! info

Tip

Extending the
[`          AbstractMediator         `](http://synapse.apache.org/apidocs/org/apache/synapse/mediators/AbstractMediator.html)
class is the easier way to write a new mediator rather than implementing
the
[`          Mediator         `](http://synapse.apache.org/apidocs/org/apache/synapse/Mediator.html)
interface.


##### MessageContext Interface

``` java
    package org.apache.synapse;
    
    import ...
    
    public interface MessageContext {
    
        /**
         * Get a reference to the current SynapseConfiguration
         *
         * @return the current synapse configuration
         */
        public SynapseConfiguration getConfiguration();
    
        /**
         * Set or replace the Synapse Configuration instance to be used. May be used to
         * programmatically change the configuration at runtime etc.
         *
         * @param cfg The new synapse configuration instance
         */
        public void setConfiguration(SynapseConfiguration cfg);
    
        /**
         * Returns a reference to the host Synapse Environment
         * @return the Synapse Environment
         */
        public SynapseEnvironment getEnvironment();
    
        /**
         * Sets the SynapseEnvironment reference to this context
         * @param se the reference to the Synapse Environment
         */
        public void setEnvironment(SynapseEnvironment se);
    
        /**
         * Get the value of a custom (local) property set on the message instance
         * @param key key to look up property
         * @return value for the given key
         */
        public Object getProperty(String key);
    
        /**
         * Set a custom (local) property with the given name on the message instance
         * @param key key to be used
         * @param value value to be saved
         */
        public void setProperty(String key, Object value);
    
        /**
         * Returns the Set of keys over the properties on this message context
         * @return a Set of keys over message properties
         */
        public Set getPropertyKeySet();
    
        /**
         * Get the SOAP envelope of this message
         * @return the SOAP envelope of the message
         */
        public SOAPEnvelope getEnvelope();
    
        /**
         * Sets the given envelope as the current SOAPEnvelope for this message
         * @param envelope the envelope to be set
         * @throws org.apache.axis2.AxisFault on exception
         */
        public void setEnvelope(SOAPEnvelope envelope) throws AxisFault;
    
        /**
         * SOAP message related getters and setters
         */
        public ....get/set()...
    }
```

The `         MessageContext        ` interface is based on the Axis2
[`          MessageContext         `](http://axis.apache.org/axis2/java/core/api/org/apache/axis2/context/MessageContext.html)
interface and uses the
[`          EndpointReference         `](http://axis.apache.org/axis2/java/core/api/org/apache/axis2/addressing/EndpointReference.html)
and
[`          SOAPEnvelope         `](http://ws.apache.org/axiom/apidocs/org/apache/axiom/soap/SOAPEnvelope.html)
classes/interfaces. The purpose of this interface is to capture a
message as it flows through the system. As you see the message payload
is represented using the SOAP infoset. Binary messages can be embedded
in the Envelope using MTOM (SOAP Message Transmission Optimization
Mechanism) or SwA (SOAP with Attachments) using the AXIOM (AXis Object
Model) object model.

### Mediator Interface

The second key interface for mediator writers is the
`         Mediator        ` interface.

``` java
    package org.apache.synapse;
    
    import org.apache.synapse.MessageContext;
    
    /**
     * All Synapse mediators must implement this Mediator interface. As a message passes
     * through the synapse system, each mediator's mediate() method is invoked in the
     * sequence/order defined in the SynapseConfiguration.
     */
    public interface Mediator {
    
        /**
         * Invokes the mediator passing the current message for mediation. Each
         * mediator performs its mediation action, and returns true if mediation
         * should continue, or false if further mediation should be aborted.
         *
         * @param synCtx the current message for mediation
         * @return true if further mediation should continue
         */
        public boolean mediate(MessageContext synCtx);
    
        /**
         * This is used for debugging purposes and exposes the type of the current
         * mediator for logging and debugging purposes
         * @return a String representation of the mediator type
         */
        public String getType();
    
        /**
         * This is used to check whether the tracing should be enabled on the current mediator or not
         * @return value that indicate whether tracing is on, off or unset
         */
        public int getTraceState();
    
        /**
         * This is used to set the value of tracing enable variable
         * @param traceState Set whether the tracing is enabled or not
         */
        public void setTraceState(int traceState);
    }
```

A mediator can read and/or modify the message in any suitable manner -
adjusting the routing headers or changing the message body. If the
`         mediate()        ` method returns "false", it signals to the
Synapse processing model to stop further processing of the message. For
example, if the mediator is a security agent, it may decide that this
message is dangerous and should not be processed further. This is
generally the exception as mediators are usually designed to co-operate
to process the message onwards.

### Leaf and Node Mediators, List Mediators and Filter Mediators

Mediators may be `         Node        ` mediators (they can contain
child mediators) or `         Leaf        ` mediators (mediators that
does not hold any other child mediators). A `         Node        `
mediator must implement the
[`          org.apache.synapse.mediators.ListMediator         `](https://synapse.apache.org/apidocs/org/apache/synapse/mediators/ListMediator.html)
interface listed below or extend from
[`          org.apache.synapse.mediators.AbstractListMediator         `](http://synapse.apache.org/apidocs/org/apache/synapse/mediators/AbstractListMediator.html)
.

##### The ListMediator Interface

``` java
    package org.apache.synapse.mediators;
    
    import java.util.List;
    
    /**
    * The List mediator executes a given sequence/list of child mediators
    */
    public interface ListMediator extends Mediator {
        /**
        * Appends the specified mediator to the end of this mediator's (children) list
        * @param m the mediator to be added
        * @return true (as per the general contract of the Collection.add method)
        */
        public boolean addChild(Mediator m);
    
        /**
        * Appends all of the mediators in the specified collection to the end of this mediator's (children)
        * list, in the order that they are returned by the specified collection's iterator
        * @param c the list of mediators to be added
        * @return true if this list changed as a result of the call
        */
        public boolean addAll(List c);
    
        /**
        * Returns the mediator at the specified position
        * @param pos index of mediator to return
        * @return the mediator at the specified position in this list
        */
        public Mediator getChild(int pos);
    
        /**
        * Removes the first occurrence in this list of the specified mediator
        * @param m mediator to be removed from this list, if present
        * @return true if this list contained the specified mediator
        */
        public boolean removeChild(Mediator m);
    
        /**
        * Removes the mediator at the specified position in this list
        * @param pos the index of the mediator to remove
        * @return the mediator previously at the specified position
        */
        public Mediator removeChild(int pos);
    
        /**
        * Return the list of mediators of this List mediator instance
        * @return the child/sub mediator list
        */
        public List getList();
    }
```

A `         ListMediator        ` implementation should call
`         super.mediate(synCtx)        ` to process its sub mediator
sequence.

A
[`          FilterMediator         `](http://synapse.apache.org/apidocs/org/apache/synapse/mediators/FilterMediator.html)
is a `         ListMediator        ` that executes its sequence of sub
mediators on successful outcome of a test condition. The Mediator
instance that performs filtering should implement the
`         FilterMediator        ` interface.

##### FilterMediator Interface

``` java
    package org.apache.synapse.mediators;
    
    import org.apache.synapse.MessageContext;
    
    /**
     * The filter mediator is a list mediator, which executes the given (sub) list of mediators
     * if the specified condition is satisfied
     *
     * @see FilterMediator#test(org.apache.synapse.MessageContext)
     */
    public interface FilterMediator extends ListMediator {
    
        /**
         * Should return true if the sub/child mediators should execute. i.e. if the filter
         * condition is satisfied
         * @param synCtx
         * @return true if the configured filter condition evaluates to true
         */
        public boolean test(MessageContext synCtx);
    }
```
