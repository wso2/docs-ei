# FIX Transport

**FIX (Financial Information eXchang) transport** implementation is a
module developed under the Apache Synapse project. This transport is
mainly used in conjunction with proxy services. This transport supports JMX.

FIX transport does not support any global parameters. All the FIX
configuration parameters should be specified at service level. QuickFix 4J configuration parameters can be found
[here](http://www.quickfixengine.org/quickfix/doc/html/configuration.html)

## Service Level FIX Parameters

!!! Info
	In transport parameter tables, literals displayed in italic mode under the "Possible Values" column should be considered as fixed literal constant values. Those values can be directly put in transport configurations.

<table>
   <thead>
      <tr>
         <th>
          Property
         </th>
         <th>
           Description
         </th>
      </tr>
   </thead>
   <tbody>
      <tr>
         <td>
			transport.fix.AcceptorConfigURL
         </td>
         <td>
            URL to the Quickfix/J acceptor configuration file (see notes below).</br></br> This is required for receiving messages over FIX.
         </td>
      </tr>
      <tr>
         <td>
            transport.fix.InitiatorConfigURL
         </td>
         <td>
            URL to the Quickfix/J initiator configuration file (see notes below).</br></br>
            Required for sendingmessages over FIX
         </td>
      </tr>
      <tr>
         <td>
            transport.fix.AcceptorLogFactory
         </td>
         <td>
            Log factory implementation to be used for the FIX acceptor (Determines how logging is done at the acceptor level).</br></br>
            Possible values are <code>console</code>, <code>file</code>, and <code>jdbc</code>.</br></br>
            Logging is disabled by default.
         </td>
      </tr>
      <tr>
         <td>
            transport.fix.InitiatorLogFactory
         </td>
         <td>
            Log factory implementation to be used for the FIX acceptor (Determines how logging is done at the acceptor level).</br></br>
            Possible values are <code>console</code>, <code>file</code>, and <code>jdbc</code>.</br></br>
            Logging is disabled by default.
         </td>
      </tr>
      <tr>
         <td>
            transport.fix.AcceptorMessageStore
         </td>
         <td>
            Message store mechanism to be used with the acceptor (Determines how the FIX message store is maintained).</br></br>
            Possible values are <code>memory</code>, <code>file</code>, <code>sleepycat</code>, and <code>jdbc</code>.</br></br>
            The default value is <code>memory</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.fix.InitiatorMessageStore
         </td>
         <td>
            Message store mechanism to be used with the initiator (Determines how the FIX message store is maintained).</br></br>
            Possible values are <code>memory</code>, <code>file</code>, <code>sleepycat</code>, and <code>jdbc</code>.</br></br>
            The default value is <code>memory</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.fix.ResponseDeliverToCompID
         </td>
         <td>
            If the response FIX messages should be delivered to a location different from the location the request was originated use this property to set the DeliverToCompID field of the FIX messages.
         </td>
      </tr>
      <tr>
         <td>
            transport.fix.ResponseDeliverToSubID
         </td>
         <td>
            If the response FIX messages should be delivered to a location different from the location the request was  originated use this property to set the DeliverToSubID field of the FIX messages.
         </td>
      </tr>
      <tr>
         <td>
            transport.fix.ResponseDeliverToLocationID
         </td>
         <td>
            If the response FIX messages should be delivered to a location different from the location the request was originated use this property to set the DeliverToLocationID field of the FIX messages.
         </td>
      </tr>
      <tr>
         <td>
            transport.fix.endAllToInSequence
         </td>
         <td>
            By default, all received FIX messages (including responses) will be directed to the in sequence of the proxy service. Use this property to override that behavior.<br/><br/>
            By default, this setting is <code>true</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.fix.BeginStringValidation
         </td>
         <td>
            Whether the transport should validate BeginString values when forwrding FIX messages across sessions.<br/><br/>
            By default, this setting is <code>true</code>.
         </td>
      </tr>
      <tr>
         <td>
            transport.fix.DropExtraResponses
         </td>
         <td>
            In situation where the FIX recipient sends multiple responses per request use this parameter to drop excessive responses and use only the first one.<br/><br/>
            By default, this setting is <code>false</code>.
         </td>
      </tr>
   </tbody>
</table>