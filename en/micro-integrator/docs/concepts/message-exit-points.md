# Message exit points

A message exit point or an endpoint defines an external destination for a message. Typically, this is the address of a proxy service, which acts as the front end to the actual service.

You can configure the endpoint artifacts with any attributes or semantics needed for communicating with that service. An endpoint could represent a URL,a mailbox, a JMS queue, or a TCP socket, etc. along with the settings needed to connect to it. For example, the endpoint for the simple stock quote sample is `http://localhost:9000/services/SimpleStockQuoteService`.

An endpoint is defined independently of transports, allowing you to use the same endpoint with multiple transports. When you configure a message mediation sequence or a proxy service to handle the incoming message, you can specify which transport to use and the endpoint to which the message will be sent. 

Endpoints can be used in the following ways:

<table>
	<tr>
		<td>Named Endpoints</td>
		<td>
			When you create an Endpoint with a name (named endpoint), you can reuse it by referencing it in another endpoint. For example, if there is an endpoint named <code>foo</code>, you can reference the <code>foo</code> endpoint in any other endpoint using the <code>key</code> attribute: <b><endpoint key="foo"/></b>
		</td>
	</tr>
	<tr>
		<td>Indirect Endpoints</td>
		<td>
			Uses the <code>key</code> attribute to call another endpoint at runtime and delegates the message sending to the called endpoint.</br> Indirect and resolving endpoints are useful when the actual endpoints are stored in the registry.
		</td>
	</tr>
	<tr>
		<td>Resolving Endpoint</td>
		<td>
			Uses an XPath expression to dynamically call another endpoint at runtime. The XPath is evaluated against the current message and the key-expression is calculated at runtime. The resolving endpoint then fetches the actual endpoint using the calculated key and delegates the message sending to the actual endpoint.</br> Indirect and resolving endpoints are useful when the actual endpoints are stored in the registry.
		</td>
	</tr>
	<tr>
		<td>Load-balanced Group</td>
		<td>
			Distributes the messages (load) among a set of listed endpoints or static members by evaluating the load balancing policy and other relevant parameters.
		</td>
	</tr>
</table>

You can configure the following endpoint types.

<table>
	<tr>
		<th>Endpoint</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>Address Endpoint</td>
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
			Allows you to define REST endpoints using <b>URI templates</b> similar to the REST API. The URI templates allow a RESTful URI to contain variables that can be populated during mediation runtime using property values with the <code>uri.var.</code> prefix. An HTTP endpoint can also define the particular HTTP method to use in the RESTful invocation.
		</td>
	</tr>
	<tr>
		<td>Failover Endpoint</td>
		<td>
			If an error occurs in an endpoint during message transmission, the message will be lost. The failed message will not be retried again. With some applications, these message losses are acceptable. However, if even rare message failures are not acceptable, use the <b>Failover</b> endpoint.</br></br>
			A <b>Failover Group</b> is a list of leaf endpoints grouped together for the purpose of passing an incoming message from one to another when a failover occurs. The first endpoint in the failover group is considered the primary endpoint. An incoming message is first directed to the primary endpoint, and all other endpoints in the group serve as backups.</br></br>
			If the primary endpoint fails, the next active endpoint is selected as the primary endpoint, and the failed endpoint is marked as inactive. Thus, failover group ensures that a message is delivered as long as there is at least one active endpoint among the listed endpoints. The Micro Integrator switches back to the primary endpoint as soon as it becomes available. This behaviour is known as dynamic failover.</br></br>
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
			Template endpoints are created based on predefined <b>Endpoint Templates</b>. Parameters of the endpoint template used as the target are copied to the endpoint configuration. However, you can make modifications by removing some of the copied parameters and/or adding new parameters.
		</td>
	</tr>
	<tr>
		<td>WSDL Endpoint</td>
		<td>
			This definition is based on a specified WSDL document. The WSDL document can be specified in 2 ways:
			<ul>
				<li>As a URI.</li>
				<li>As an inlined definition within the endpoint configuration.</li>
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

## Endpoint states

At any given time, the state of the endpoint can be one of the following:

<table>
	<tr>
		<th>State</th>
		<th>Description</th>
	</tr>
	<tr>
		<td>Active</td>
		<td>
			Endpoint is running and handling requests.</br></br>
			When the Micro Integrator starts, endpoints are in the "Active" state and ready to handle messages. If the user does not put the endpoint into the OFF state, it will be in the "Active" state until an error occurs.</br></br>
			The endpoint can be configured to stay in the "Active" state or to go to "Timeout" or "Suspended" based on the error codes you configure for those states. When an error occurs, the endpoint checks to see whether it is a "Timeout" error first, and if not, it checks to see whether it is a "Suspended" error. If the error is not defined for either "Timeout" or "Suspended," the error will be ignored and the endpoint will stay Active.
		</td>
	</tr>
	<tr>
		<td>Timeout</td>
		<td>
			Endpoint encountered an error but can still send and receive messages. If it continues to encounter errors, it will be suspended.</br></br>
			When an endpoint is in the "Timeout" state, it will continue to attempt to receive messages until one message succeeds or the maximum retry setting has been reached. If the maximum is reached at which point the endpoint is marked as "Suspended." If one message succeeds, the endpoint is marked as "Active".</br></br>
			For example, let's assume the number of retries is set to 3. When an error occurs and the endpoint is set to the "Timeout" state, the Micro Integrator can try to send up to three more messages to the endpoint. If the next three messages sent to this endpoint result in an error, the endpoint is put in the "Suspended" state. If one of the messages succeeds before the retry maximum is met, the endpoint will be marked as "Active."
		</td>
	</tr>
	<tr>
		<td>Suspended</td>
		<td>
			Endpoint encountered errors and cannot send or receive messages. Incoming messages to a suspended endpoint result in a fault.</br></br>
			A "Suspended" endpoint cannot send or receive messages. When an endpoint is put into this state, the Enterprise Integrator waits until after an initial duration has elapsed (default is 30 seconds) before attempting to send messages to this endpoint again. If the message succeeds, the endpoint is marked as "Active." If the next message fails, the endpoint is marked as "Suspended" or "Timeout" depending on the error, and the Enterprise Integrator waits before retrying messages using the following formula: <code>Min(current suspension duration * progressionFactor, maximumDuration)</code>.</br></br>
			You configure the initial suspension duration, progression factor, and maximum duration as part of the <b>suspendOnFailure</b> settings. On each retry, the suspension duration increases, up to the maximum duration.
		</td>
	</tr>
	<tr>
		<td>OFF</td>
		<td>
			Endpoint is not active. To put an endpoint into the OFF state, or to move it from OFF to Active, you must use JMX.
		</td>
	</tr>
</table>