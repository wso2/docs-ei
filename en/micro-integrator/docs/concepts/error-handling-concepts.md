# Error Handling

The main role of WSO2 Micro Integrator is to act as the backbone of an organization’s service-oriented architecture. It is the spine through which all the systems and applications within the enterprise (and external applications that integrate with the enterprise) communicate with each other. 

For example, the Micro Integrator often has to deal with many wire-level protocols, messaging standards, and remote APIs. But applications and networks can be full of errors. Applications crash. Network routers and links get into states where they cannot pass messages through with the expected efficiency. These error conditions are very likely to cause a fault or trigger a runtime exception in the Micro Integrator server.

## Handling mediation errors

When you define a mediation sequence, [Fault Sequences](../../concepts/message-processing-units/#fault-sequences) are used to handle messages that are affected by mediation errors. 

## Handling endpoint errors

Errors that can occur at [endpoints](message-exit-points.md) can be specifically configured using the [endpoint error handling properties](../../references/synapse-properties/endpoint-properties/#endpoint-error-handling-properties).

The last step of **message mediation** is to send the message to a service provider through a listening service [endpoint](message-exit-points.md). During this process, transport errors can occur. For example, the connection might time out, or it might be closed by the actual service. Therefore, endpoint error handling is a key part of any successful Micro Integrator deployment.

Messages can fail or be lost due to various reasons in a real TCP network. When an endpoint error occurs, if the Micro Integrator is not configured to accept the error, it will mark the endpoint as failed, which leads to a message failure. By default, the endpoint is marked as failed for quite a long time, which can result in severe message loss.

To avoid message loss, you configure error handling at the [endpoint](message-exit-points.md) level. You should also run a few long-running load tests to discover errors and fine-tune the endpoint configurations for errors that can occur intermittently due to various reasons.

At any given time, the state of the endpoint can be one of the following. During an endpoint error, the endpoint will transition between these states and, if required, will initiate a [fault sequence](../../concepts/message-processing-units/#fault-sequences).

<table>
	<tr>
		<th>State</th>
		<th>Description</th>
	</tr>
	<tr>
		<td id='active_state'>Active</td>
		<td>
			Endpoint is running and handling requests.</br></br>
			When the Micro Integrator starts, endpoints are in "Active" state until the user sets it to <a href="#off_state">OFF</a> state, or until an error occurs.</br></br>
			In the endpoint error handling configuration, error codes are allocated to a particular endpoint state. Therefore, when the error occurs, the endpoint will either remain in <a href="#active_state">Active</a> state or change to <a href="#timeout_state">Timeout</a> or <a href="#suspended_state">Suspended</a> depending on the error code. The endpoint first checks whether the error is a <a href="#timeout_state">Timeout</a> error, and if not, it checks whether it is a <a href="#suspended_state">suspended</a> error. If the error is not defined for either "Timeout" or "Suspended," the error will be ignored and the endpoint will remain active.
		</td>
	</tr>
	<tr>
		<td id="timeout_state">Timeout</td>
		<td>
			Endpoint encountered an error but can still send and receive messages. If it continues to encounter errors, it will be <a href="#suspended_state">suspended</a>.</br></br>
			When an endpoint is in the <a href="#timeout_state">Timeout</a> state, it will continue to attempt to receive messages until one message succeeds or the maximum retry setting has been reached. If the maximum is reached at which point, the endpoint is marked as <a href="#suspended_state">Suspended</a>. If one message succeeds, the endpoint is marked as <a href="#active_state">Active</a>.</br></br>
			For example, let's assume the number of retries is set to 3. When an error occurs and the endpoint is set to the "Timeout" state, the Micro Integrator can try to send up to three more messages to the endpoint. If the next three messages sent to this endpoint result in an error, the endpoint is put in the <a href="#suspended_state">Suspended</a> state. If one of the messages succeeds before the retry maximum is met, the endpoint will be marked as <a href="#active_state">Active</a>.
		</td>
	</tr>
	<tr>
		<td id="suspended_state">Suspended</td>
		<td>
			Endpoint encountered errors and cannot send or receive messages. Incoming messages to a suspended endpoint result in a fault.</br></br>
			When an endpoint is put into this state, the Micro Integrator waits until after an initial duration has elapsed (default is 30 seconds) before attempting to send messages to this endpoint again. If the message succeeds, the endpoint is marked as <a href="#active_state">Active</a>. If the next message fails, the endpoint is marked as <a href="#suspended_state">Suspended</a> or <a href="#timeout_state">Timeout</a> depending on the error, and the Micro Integrator waits before retrying messages using the following formula: <code>Min(current suspension duration * progressionFactor, maximumDuration)</code>.</br></br>
			You configure the initial suspension duration, progression factor, and maximum duration as part of the <b>suspendOnFailure</b> settings. On each retry, the suspension duration increases, up to the maximum duration.
		</td>
	</tr>
	<tr>
		<td id='off_state'>OFF</td>
		<td>
			Endpoint is not active. To put an endpoint into the OFF state, or to move it from OFF to <a href="#active_state">Active</a>, you must use JMX.
		</td>
	</tr>
</table>

See [mediation debugging](debugging-concepts.md) for information on debugging errors when they occur.
	