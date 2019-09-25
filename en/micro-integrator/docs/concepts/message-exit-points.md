# Message exit points

A message exit point or an endpoint defines an external destination for a message. Typically, this is the address of a proxy service, which acts as the front end to the actual service. You can configure the endpoint artifacts with any attributes or semantics needed for communicating with that service. An endpoint could represent a URL, a mailbox, a JMS queue, a TCP socket, etc. along with the settings needed for the connection. 

For example, the endpoint for the simple stock quote sample is `http://localhost:9000/services/SimpleStockQuoteService`.

Endpoints are independant of transports, which allows you to use the same endpoint with multiple transports. When you configure a message mediation sequence or a proxy service to handle the incoming message, you can specify which transport to use and the endpoint to which the message will be sent. 

## Classification of Endpoints

### Named Endpoints

A named endpoint can be any one of the [listed endpoints](#list-of-endpoints). You can reuse these endpoints by referencing them in another endpoint by name using the `key` attribute. For example, if you have an [address endpoint](#address_endpoint) named `foo`, you can reference the `foo` endpoint in an [indirect](#indirect-and-resolving-endpoints) using the `key` attribute: `<endpoint key="foo">`

### Indirect Endpoints

Indirect endpoints are endpoint configurations that refer [named endpoints](#named-endpoints) using a key. The <code>key</code> attribute calls the [named endpoint](#named-endpoints) at runtime and delegates the message sending to the called endpoint.

``` xml tab='Actual Endpoint'
<endpoint name="address">
   <address uri="http://localhost:9000/services/SimpleStockQuoteService" format="soap11"/>
</endpoint>
```

``` xml tab='Indirect Endpoint'
<endpoint key="address">
```

Indirect endpoints are useful when the actual endpoints are stored in the [registry](registry-concepts.md).

### Resolving Endpoints

The resolving endpoint refers to an actual endpoint using a dynamic key (which is an XPath expression). The XPath expression dynamically calls another endpoint at runtime. The XPath is evaluated against the current message and the key-expression is calculated at runtime. The resolving endpoint then fetches the actual endpoint using the calculated key and delegates the message sending to the actual endpoint. Shown below is an example of a resolving endpoint.

```xml
<send>
	<endpoint key-expression="get-property('Mail')"/>
</send>
```

## List of Endpoints

You can configure the following endpoint types.

<table>
	<tr>
		<th>Endpoint</th>
		<th>Description</th>
	</tr>
	<tr>
		<td id='address_endpoint'>Address Endpoint</td>
		<td>
			Specifies the address URL or EPR (Endpoint Reference) and other attributes of the configuration.
		</td>
	</tr>
	<tr>
		<td>Default Endpoint</td>
		<td>
			Defined for adding QoS and other configurations to the endpoint that is resolved by the <b>To</b> address of the message context. All the configurations such as the message format for the endpoint, the method to optimize attachments, and security policies for the endpoint can be specified as in the <b>Address Endpoint</b>. This endpoint differs from the address endpoint because the <b>URI</b> property is not present in the default endpoint.
		</td>
	</tr>
	<tr>
		<td>HTTP Endpoint</td>
		<td>
			Allows you to define REST endpoints using <b>URI templates</b> similar to the <a href="../../concepts/message-entry-points/#rest-apis">REST API</a>. The URI templates allow a RESTful URI to contain variables that can be populated during mediation runtime using <a href="../../references/mediators/property-Mediator">property</a> values with the <code>uri.var.</code> prefix. An HTTP endpoint can also define the particular HTTP method to use in the RESTful invocation.
		</td>
	</tr>
	<tr>
		<td>Failover Endpoint</td>
		<td>
			If an error occurs in an endpoint during message transmission, the message will be lost. The failed message will not be retried again. With some applications, these message losses are acceptable. However, if even rare message failures are not acceptable, use the <b>Failover</b> endpoint.</br></br>
			A <b>Failover Group</b> is a list of leaf endpoints grouped together for the purpose of passing an incoming message from one to another when a failover occurs. The first endpoint in the failover group is considered the primary endpoint. An incoming message is first directed to the primary endpoint, and all other endpoints in the group serve as backups.</br></br>
			If the primary endpoint fails, the next active endpoint is selected as the primary endpoint, and the failed endpoint is marked as inactive. Thus, failover group ensures that a message is delivered as long as there is at least one active endpoint among the listed endpoints. The Micro Integrator switches back to the primary endpoint as soon as it becomes available. This behaviour is known as <b>dynamic failover</b>.</br></br>
			<b>Note</b>: An endpoint failure occurs when an endpoint is unable to invoke a service. An endpoint, which responds with an error is not considered a failed endpoint.
		</td>
	</tr>
	<tr>
		<td>Dynamic Load-Balance Endpoint</td>
		<td>
			This endpoint distributes its messages (load) among application members by evaluating the load-balancing policy and any other relevant parameters.</br></br> These application members will be discovered using the <b>membershipHandler</b> class, which generally uses a group communication mechanism to discover the application members. The <b>class</b> attribute of the <b>membershipHandler</b> element should be an implementation of <b>org.apache.synapse.core.LoadBalanceMembershipHandler</b>. You can specify <b>membershipHandler</b> properties using the <b>property</b> elements. The <b>policy </b> attribute of the <b>dynamicLoadbalance</b> element specifies the load-balancing policy (algorithm) to be used for selecting the next member that will receive the message.</br></br>
			<b>Note</b>: Currently only the <b>roundRobin</b> policy is supported. The <b>failover</b> attribute determines if the next member should be selected once the currently selected member has failed and defaults to true.
		</td>
	</tr>
	<tr>
		<td>Template Endpoint</td>
		<td>
			Template endpoints are created based on predefined <a href="../../concepts/message-processing-units/#templates">Endpoint Templates</a>. Parameters of the endpoint template used as the target are copied to the endpoint configuration. However, you can make modifications by removing some of the copied parameters and/or adding new parameters.
		</td>
	</tr>
	<tr>
		<td>WSDL Endpoint</td>
		<td>
			This definition is based on a specified WSDL document. The WSDL document can be specified in 2 ways:
			<ul>
				<li>As a URI.</li>
				<li>As an inline definition within the endpoint configuration.</li>
			</ul>
		</td>
	</tr>
	<tr>
		<td>Recepient List Endpoint</td>
		<td>
			This endpoint contains multiple child endpoints or member elements. It routes cloned copies of messages to each child recipient. This will assume that all immediate child endpoints are identical in state (state is replicated) or state is not maintained at those endpoints.
		</td>
	</tr>
</table>