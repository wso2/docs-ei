# In and Out Mediators

!!! info

Note

Please note that this feature is deprecated.


  

The **In** and **Out Mediators** act as predefined filters. Messages
that are in the In path of the EI will traverse through the child
mediators of the In Mediator. Messages that are in the Out path of the
ESB profile will traverse through the child mediators of the Out
Mediator.

!!! info

Note

Do not use these mediators in proxy service sequences, as proxy services
have the predefined sequences \<inSequence\> and \<outSequence\> for
this purpose.


------------------------------------------------------------------------

[Syntax](#InandOutMediators-Syntax) \|
[Configuration](#InandOutMediators-Configuration) \|
[Example](#InandOutMediators-Example)

------------------------------------------------------------------------

### Syntax

In

``` java
    <in>
          mediator+
    </in>
```

Out

``` java
    <out>
          mediator+
    </out>
```

------------------------------------------------------------------------

### Configuration

After adding an In or Out mediator, you add and configure its child
mediators.

------------------------------------------------------------------------

### Example

In the following example, the In mediator has a [Log
mediator](_Log_Mediator_) and a [Filter mediator](_Filter_Mediator_) as
child mediators. The [Log mediator](_Log_Mediator_) logs the messages in
the In path. Then the [Filter mediator](_Filter_Mediator_) filters these
messages and sends the messages which match the filter criteria to
`                   http://localhost:9000                 ` . The
messages are sent via the [Send mediator](_Send_Mediator_) which is
added as a child to the [Filter mediator](_Filter_Mediator_) .

The messages in the Out path are sent via the [Send
mediator](_Send_Mediator_) which is added as a child to the Out
mediator.

``` java
    <sequence name="main" xmlns="http://ws.apache.org/ns/synapse">
          <in>
              <log level="full"/>
              <filter source="get-property('To')" regex="http://localhost:9000.*">
                  <send/>
              </filter>
          </in>
          <out>
              <send/>
          </out>
    </sequence>
```
