# Dynamic Load-balance Endpoint

The **Dynamic Load-balance** **Endpoint** is an
[endpoint](_Working_with_Endpoints_) that distributes its messages
(load) among application members by evaluating the load-balancing policy
and any other relevant parameters. These application members will be
discovered using the `         membershipHandler        ` class, which
generally uses a group communication mechanism to discover the
application members. The `         class        ` attribute of the
`         membershipHandler        ` element should be an implementation
of
`         org.apache.synapse.core.LoadBalanceMembershipHandler        `
. You can specify `         membershipHandler        ` properties using
the `         property        ` elements. The `         policy        `
attribute of the `         dynamicLoadbalance        ` element specifies
the load-balancing policy (algorithm) to be used for selecting the next
member that will receive the message.

!!! info

Currently only the `         roundRobin        ` policy is supported.


The `         failover        ` attribute determines if the next member
should be selected once the currently selected member has failed and
defaults to true.

### Dynamic Load-balance Endpoint Configuration

Currently the Dynamic Load-balance Endpoint does not support configuring
it using a UI. However, you can configure it using its XML source code
in the **Source View** . Click **Source View** under **Service Bus** in
the **Main** menu tab of the Enterprise Integrator management console,
to add a Dynamic Load-balance Endpoint. The syntax is as follows.

``` html/xml
    <dynamicLoadbalance [policy="roundRobin"] [failover="true|false"]>
        <membershipHandler class="impl of org.apache.synapse.core.LoadBalanceMembershipHandler">
          <property name="name" value="value"/>
        </membershipHandler>
    </dynamicLoadbalance>
```

See [Sample 57: Dynamic Load Balancing between Three
Nodes](https://docs.wso2.com/display/EI620/Sample+57%3A+Dynamic+Load+Balancing+between+Three+Nodes)
for an example.

  
