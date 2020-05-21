# Amazon SQS Connector Configuration

The following operations allow you to work with the Amazon SQS Connector. Click an operation name to see parameter details and samples on how to use it.

---

## Initialize the connector

To use the Amazon SQS connector, add the <amazonsqs.init> element in your configuration before carrying out any other Amazon SQS operations. This uses the standard HTTP Authorization header to pass authentication information. Developers are issued an AWS access key ID and an AWS secret access key when they register. For request authentication, the secret access key and the access key ID elements will be used to compute the signature. The authentication uses the "HmacSHA256" signature method and the signature version "4". Click [here](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/RequestAuthenticationArticle.html) for further information on the authentication process. To use the HTTPS amazon AWS url, you need to import the certificate into the WSO2 EI client keystore.

??? note "init"
    The init operation is used to initialize the connection to Amazon SQS.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>secretAccessKey</td>
            <td>The secret access key (a 40-character sequence).</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>accessKeyId</td>
            <td>The access key ID that corresponds to the secret access key that you used to sign the request (a 20-character, alphanumeric sequence).</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>version</td>
            <td>The version of the API, which is "2009-02-01".</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>region</td>
            <td>The regional endpoint to make your requests (e.g., us-east-1).</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>enableSSL</td>
            <td>Whether the Amazon AWS URL should be HTTP or HTTPS. Set to true if you want the URL to be HTTPS.</td>
            <td>Optional</td>
        </tr>
        <tr>
            <td>contentType</td>
            <td>The content type that is used to generate the signature.</td>
            <td>Optional</td>
        </tr>
        <tr>
            <td>blocking</td>
            <td>Boolean type, this property helps the connector perform blocking invocations to Amazon SQS.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazonsqs.init>
       <secretAccessKey>{$ctx:secretAccessKey}</secretAccessKey>
        <accessKeyId>{$ctx:accessKeyId}</accessKeyId>
        <version>{$ctx:version}</version>
        <region>{$ctx:region}</region>
        <enableSSL>{$ctx:enableSSL}</enableSSL>
        <contentType>{$ctx:contentType}</contentType>
        <blocking>{$ctx:blocking}</blocking>
    </amazonsqs.init>
    ```
    
---

### Messages

??? note "receiveMessage"
    This operation retrieves one or more messages, with a maximum limit of 10 messages, from the specified queue. The default behavior is short poll, where a weighted random set of machines is sampled. This means only the messages on the sampled machines are returned. If the number of messages in the queue is small (less than 1000), it is likely you will get fewer messages than you requested per call. If the number of messages in the queue is extremely small, you might not receive any messages in a particular response. In this case, you should repeat the request. See the [related API documentation](http://docs.aws.amazon.com/AWSSimpleQueueService/latest/APIReference/API_ReceiveMessage.html) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>maxNumberOfMessages</td>
            <td>The maximum number of messages to be returned. Values can be from 1 to 10. Default is 1.</td>
            <td>Optional</td>
        </tr>
        <tr>
            <td>waitTimeSeconds</td>
            <td>The duration (in seconds) for which the call will wait for a message to arrive in the queue before returning. If a message is available, the call will return sooner than WaitTimeSeconds. Long poll support is enabled by using this parameter. For more information, see <a href="http://docs.aws.amazon.com/AWSSimpleQueueService/latest/SQSDeveloperGuide/sqs-long-polling.html">Amazon SQS Long Poll</a>.</td>
            <td>Optional</td>
        </tr>
        <tr>
            <td>messageAttributeNames</td>
            <td>The name of the message attribute. The message attribute name can contain the following characters: A-Z, a-z, 0-9, underscore (_), hyphen (-), and period (.). The name must not start or end with a period, and it should not have successive periods. The name is case sensitive and must be unique among all attribute names for the message. The name can be up to 256 characters long. The name cannot start with "AWS." or "Amazon." (including any case variations), because these prefixes are reserved for use by Amazon Web Services. When using the operation, you can send a list of attribute names to receive, or you can return all of the attributes by specifying "All" or ".*" in your request. You can also use "foo.*" to return all message attributes starting with the "foo" prefix.</td>
            <td>Optional</td>
        </tr>
        <tr>
            <td>visibilityTimeout</td>
            <td>The duration (in seconds) in which the received messages are hidden from subsequent retrieve requests after being retrieved by the request.</td>
            <td>Optional</td>
        </tr>
        <tr>
            <td>attributes</td>
            <td>A list of attributes that need to be returned along with each message.</td>
            <td>Optional</td>
        </tr>
        <tr>
            <td>queueId</td>
            <td>The unique identifier of the queue.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>queueName</td>
            <td>The name of the queue.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazonsqs.receiveMessage>
        <maxNumberOfMessages>{$ctx:maxNumberOfMessages}</maxNumberOfMessages>
        <waitTimeSeconds>{$ctx:waitTimeSeconds}</waitTimeSeconds>
        <messageAttributeNames>{$ctx:messageAttributeNames}</messageAttributeNames>
        <visibilityTimeout>{$ctx:visibilityTimeout}</visibilityTimeout>
        <attributes>{$ctx:attributes}</attributes>
        <queueId>{$ctx:queueId}</queueId>
        <queueName>{$ctx:queueName}</queueName>
    </amazonsqs.receiveMessage>
    ```
    
    **Sample request**

    ```xml
    <root>
        <accessKeyId>AKIAJXHDKJWR2ZDDVPEBTQ</accessKeyId>
        <secretAccessKey>N9VT2P3MdfaL7Li1P3hJu1GTdtOO7Kd7NfPlyYG8f/6</secretAccessKey>
        <version>2009-02-01</version>
        <region>us-east-1</region>
        <queueId>899940420354</queueId>
        <queueName>Test</queueName>
        <maxNumberOfMessages>10</maxNumberOfMessages>
    </root>
    ```