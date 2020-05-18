# Amazon S3 Connector Configuration

The following operations allow you to work with the Amazon S3 Connector. Click an operation name to see parameter details and samples on how to use it.

---

## Initialize the connector

To use the Amazon S3 connector, add the <amazons3.init> element in your configuration before carrying out any Amazon S3 operations. This Amazon S3 configuration authenticates with Amazon S3 by specifying the AWS access key ID and secret access key ID, which are used for every operation. The signature is used with every request and thus differs based on the request the user makes.

??? note "init"
    The init operation is used to initialize the connection to Amazon S2.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>accessKeyId</td>
            <td>AWS access key ID.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>secretAccessKey</td>
            <td>AWS secret access key.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>region</td>
            <td>Region which is used select a regional endpoint to make requests.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>methodType</td>
            <td>Type of the HTTP method.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>contentLength</td>
            <td>Length of the message without the headers according to RFC 2616.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>contentType</td>
            <td>The content type of the resource in case the request content in the body.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>addCharset</td>
            <td>To add the char set to ContentType header. Set to true to add the charset in the ContentType header of POST and HEAD methods.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>host</td>
            <td>For path-style requests, the value is s3.amazonaws.com. For virtual-style requests, the value is BucketName.s3.amazonaws.com.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>isXAmzDate</td>
            <td>The current date and time according to the requester.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>bucketName</td>
            <td>Name of the bucket required.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>blocking</td>
            <td>The blocking parameter helps the connector to perform the blocking invocations to Amazon S3.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>privateKeyFilePath</td>
            <td>Path of AWS private Key File.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>keyPairId</td>
            <td>Key pair ID of AWS cloud front.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>policyType</td>
            <td>Policy for the URL signing. It can be custom or canned policy.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>urlSign</td>
            <td>Specify whether to create Signed URL or not. It can be true or false.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>dateLessThan</td>
            <td>Can access the object before this specific date only.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>dateGreaterThan</td>
            <td>Can access the object before this specific date only.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>ipAddress</td>
            <td>IP address for creating Policy.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>contentMD5</td>
            <td>Base64 encoded 128-bit MD5 digest of the message without the headers according to RFC 1864.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>expect</td>
            <td>This header can be used only if a body is sent to not to send the request body until it recieves an acknowledgment.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>isXAmzDate</td>
            <td>The current date and time according to the requester.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>xAmzSecurityToken</td>
            <td>The security token based on whether using Amazon DevPay operations or temporary security credentials.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>xAmzAcl</td>
            <td>Sets the ACL of the bucket using the specified canned ACL.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>xAmzGrantRead</td>
            <td>Allows the specified grantee or grantees to list the objects in the bucket.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>xAmzGrantWrite</td>
            <td>Allows the specified grantee or grantees to create, overwrite, and delete any object in the bucket.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>xAmzGrantReadAcp</td>
            <td>Allows the specified grantee or grantees to read the bucket ACL.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>xAmzGrantWriteAcp</td>
            <td>Allows the specified grantee or grantees to write the ACL for the applicable bucket.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>xAmzGrantFullControl</td>
            <td>Allows the specified grantee or grantees the READ, WRITE, READ_ACP, and WRITE_ACP permissions on the bucket.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>xAmzMeta</td>
            <td>Field names prefixed with x-amz-meta- contain user-specified metadata.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>xAmzServeEncryption</td>
            <td>Specifies server-side encryption algorithm to use when Amazon S3 creates an object.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>xAmzStorageClass</td>
            <td>Storage class to use for storing the object.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>xAmzWebsiteLocation</td>
            <td>Amazon S3 stores the value of this header in the object metadata.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>xAmzMfa</td>
            <td>The value is the concatenation of the authentication device's serial number, a space, and the value that is displayed on your authentication device.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>xAmzCopySource</td>
            <td>The name of the source bucket and key name of the source object, separated by a slash.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>xAmzCopySourceRange</td>
            <td>The range of bytes to copy from the source object.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>xAmzMetadataDirective</td>
            <td>Specifies whether the metadata is copied from the source object or replaced with metadata provided in the request.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>xAmzCopySourceIfMatch</td>
            <td>Copies the object if its entity tag (ETag) matches the specified tag.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>xAmzCopySourceIfNoneMatch</td>
            <td>Copies the object if its entity tag (ETag) is different than the specified ETag.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>xAmzCopySourceIfUnmodifiedSince</td>
            <td>Copies the object if it hasn't been modified since the specified time.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>xAmzCopySourceIfModifiedSince</td>
            <td>Copies the object if it has been modified since the specified time.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>xAmzServerSideEncryption</td>
            <td>Specifies the server-side encryption algorithm to use when Amazon S3 creates the target object.</td>
            <td>Yes</td>
        </tr>
    </table>

    > **Note**: You need to pass the bucketName within init configuration only if you use the bucketURL in path-style (e.g., BucketName.s3.amazonaws.com). For the virtual-style bucketUrl (e.g., s3.amazonaws.com) you should not pass the bucketName.

    **Sample configuration**

    ```xml
    <amazons3.init>
        <accessKeyId>{$ctx:accessKeyId}</accessKeyId>
        <secretAccessKey>{$ctx:secretAccessKey}</secretAccessKey>
        <methodType>{$ctx:methodType}</methodType>
        <region>{$ctx:region}</region>
        <contentType>{$ctx:contentType}</contentType>
        <addCharset>{$ctx:addCharset}</addCharset>
        <bucketName>{$ctx:bucketName}</bucketName>
        <isXAmzDate>{$ctx:isXAmzDate}</isXAmzDate>
        <expect>{$ctx:expect}</expect>
        <contentMD5>{$ctx:contentMD5}</contentMD5>
        <xAmzSecurityToken>{$ctx:xAmzSecurityToken}</xAmzSecurityToken>
        <contentLength>{$ctx:contentLength}</contentLength>
        <host>{$ctx:host}</host>
        <xAmzAcl>{$ctx:xAmzAcl}</xAmzAcl>
        <xAmzGrantRead>{$ctx:xAmzGrantRead}</xAmzGrantRead>
        <xAmzGrantWrite>{$ctx:xAmzGrantWrite}</xAmzGrantWrite>
        <xAmzGrantReadAcp>{$ctx:xAmzGrantReadAcp}</xAmzGrantReadAcp>
        <xAmzGrantWriteAcp>{$ctx:xAmzGrantWriteAcp}</xAmzGrantWriteAcp>
        <xAmzGrantFullControl>{$ctx:xAmzGrantFullControl}</xAmzGrantFullControl>
        <uriRemainder>{$ctx:uriRemainder}</uriRemainder>
        <xAmzCopySource>{$ctx:xAmzCopySource}</xAmzCopySource>
        <xAmzCopySourceRange>{$ctx:xAmzCopySourceRange}</xAmzCopySourceRange>
        <xAmzCopySourceIfMatch>{$ctx:xAmzCopySourceIfMatch}</xAmzCopySourceIfMatch>
        <xAmzCopySourceIfNoneMatch>{$ctx:xAmzCopySourceIfNoneMatch}</xAmzCopySourceIfNoneMatch>
        <xAmzCopySourceIfUnmodifiedSince>{$ctx:xAmzCopySourceIfUnmodifiedSince}</xAmzCopySourceIfUnmodifiedSince>
        <xAmzCopySourceIfModifiedSince>{$ctx:xAmzCopySourceIfModifiedSince}</xAmzCopySourceIfModifiedSince>
        <cacheControl>{$ctx:cacheControl}</cacheControl>
        <contentEncoding>{$ctx:contentEncoding}</contentEncoding>
        <expires>{$ctx:expires}</expires>
        <xAmzMeta>{$ctx:xAmzMeta}</xAmzMeta>
        <xAmzServeEncryption>{$ctx:xAmzServeEncryption}</xAmzServeEncryption>
        <xAmzStorageClass>{$ctx:xAmzStorageClass}</xAmzStorageClass>
        <xAmzWebsiteLocation>{$ctx:xAmzWebsiteLocation}</xAmzWebsiteLocation>
    </amazons3.init>
    ```
    
---

### Buckets

??? note "getBuckets"
    The getBuckets implementation of the GET operation returns a list of all buckets owned by the authenticated sender of the request. To authenticate a request, use a valid AWS Access Key ID that is registered with Amazon S3. Anonymous requests cannot list buckets, and a user cannot list buckets that were not created by that particular user. When calling init before this operation, the following headers should be removed: xAmzAcl, x AmzGrantRead, xAmzGrantWrite, xAmzGrantReadAcp, xAmzGrantWriteAcp, and xAmzGrantFullControl. See the [related API documentation](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTServiceGET.html) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>apiUrl</td>
            <td>Amazon S3 API URL, e.g., http://s3.amazonaws.com</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazons3.getBuckets>
        <apiUrl>{$ctx:apiUrl}</apiUrl>
        <region>{$ctx:region}</region>
    <amazons3.getBuckets>
    ```
    
    **Sample request**

    ```xml
    <getBuckets>
        <accessKeyId>AKIAXXXXXXXXXXQM7G5EA</accessKeyId>
        <secretAccessKey>qHZBBzXXXXXXXXXXDYQc4oMQMnAOj+34XXXXXXXXXXO2s</secretAccessKey>
        <methodType>GET</methodType>
        <contentLength></contentLength>
        <contentType>application/xml</contentType>
        <contentMD5></contentMD5>
        <expect>100-continue</expect>
        <host>s3.amazonaws.com</host>
        <region>us-east-1</region>
        <isXAmzDate>true</isXAmzDate>
        <xAmzSecurityToken></xAmzSecurityToken>
        <apiUrl>https://s3.amazonaws.com</apiUrl>
    </getBuckets>
    ```

??? note "createBucket"
    The createBucket implementation of the PUT operation creates a new bucket. To create a bucket, the user should be registered with Amazon S3 and have a valid AWS Access Key ID to authenticate requests. Anonymous requests are never allowed to create buckets. By creating the bucket, the user becomes the owner of the bucket. Not every string is an acceptable bucket name. For information on bucket naming restrictions, see [Working with Amazon S3 Buckets](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html). By default, the bucket is created in the US Standard region. The user can optionally specify a region in the request body. For example, if the user resides in Europe, the user will probably find it advantageous to create buckets in the EU (Ireland) region. For more information, see [How to Select a Region for Your Buckets](http://docs.aws.amazon.com/AmazonS3/latest/dev/UsingBucket.html#access-bucket-intro). See the [related API documentation](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketPUT.html) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>bucketUrl</td>
            <td>The URL of the bucket.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>websiteConfig</td>
            <td>Website configuration information. For information on the elements you use in the request to specify the website configuration, see [here](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketPUTwebsite.html).</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazons3.createBucketWebsiteConfiguration>
        <bucketUrl>{$ctx:bucketUrl}</bucketUrl>
        <websiteConfig>{$ctx:websiteConfig}</websiteConfig>
    </amazons3.createBucketWebsiteConfiguration>
    ```
    
    **Sample request**

    ```xml
    <createBucketWebsiteConfiguration>
        <accessKeyId>AKIXXXXXXXXXXA</accessKeyId>
        <secretAccessKey>qHZXXXXXXQc4oMQMnAOj+340XXxO2s</secretAccessKey>
        <region>us-east-2</region>
        <methodType>PUT</methodType>
        <contentLength>256</contentLength>
        <contentType>application/xml</contentType>
        <contentMD5></contentMD5>
        <expect></expect>
        <host>s3.us-east-2.amazonaws.com</host>
        <isXAmzDate>true</isXAmzDate>
        <xAmzSecurityToken></xAmzSecurityToken>
        <bucketName>signv4test</bucketName>
        <bucketUrl>http://s3.us-east-2.amazonaws.com/signv4test</bucketUrl>
        <websiteConfig>
            <IndexDocument>
                <Suffix>index2.html</Suffix>
            </IndexDocument>
            <ErrorDocument>
                <Key>Error2.html</Key>
            </ErrorDocument>
            <RoutingRules>
                <RoutingRule>
                    <Condition>
                        <KeyPrefixEquals>docs/</KeyPrefixEquals>
                    </Condition>
                    <Redirect>
                        <ReplaceKeyPrefixWith>documents/</ReplaceKeyPrefixWith>
                    </Redirect>
                </RoutingRule>
                <RoutingRule>
                    <Condition>
                        <KeyPrefixEquals>images/</KeyPrefixEquals>
                    </Condition>
                    <Redirect>
                        <ReplaceKeyPrefixWith>documents/</ReplaceKeyPrefixWith>
                    </Redirect>
                </RoutingRule>
            </RoutingRules>
        </websiteConfig>
    </createBucketWebsiteConfiguration>
    ```

??? note "createBucketPolicy"
    The createBucketPolicy implementation of the PUT operation adds or replaces a policy on a bucket. If the bucket already has a policy, the one in this request completely replaces it. To perform this operation, you must be the bucket owner.

    If you are not the bucket owner but have PutBucketPolicy permissions on the bucket, Amazon S3 returns a 405 Method Not Allowed. In all other cases, for a PUT bucket policy request that is not from the bucket owner, Amazon S3 returns 403 Access Denied. There are restrictions about who can create bucket policies and which objects in a bucket they can apply to.

    When calling init before this operation, the following headers should be removed: xAmzAcl, x AmzGrantRead, xAmzGrantWrite, xAmzGrantReadAcp, xAmzGrantWriteAcp and xAmzGrantFullControl. See the [related API documentation](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketPUTpolicy.html) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>bucketUrl</td>
            <td>The URL of the bucket.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>bucketPolicy</td>
            <td>Policy of the bucket.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazons3.createBucketPolicy>
        <bucketUrl>{$ctx:bucketUrl}</bucketUrl>
        <bucketPolicy>{$ctx:bucketPolicy}</bucketPolicy>
    </amazons3.createBucketPolicy>
    ```
    
    **Sample request**

    ```json
    {
        "accessKeyId": "AKXXXXXXXXX5EAS",
        "secretAccessKey": "qHXXXXXXNMDYadDdsQMnAOj+3XXXXPs",
        "region":"us-east-2",
        "methodType": "PUT",
        "contentType": "application/json",
        "bucketName": "signv4test",
        "isXAmzDate": "true",
        "bucketUrl": "http://s3.us-east-2.amazonaws.com/signv4test",
        "contentMD5":"",
        "xAmzSecurityToken":"",
        "host":"s3.us-east-2.amazonaws.com",
        "expect":"",
        "contentLength":"",
        "bucketPolicy": {
                    "Version":"2012-10-17",
                    "Statement":[{
                        "Sid":"AddPerm",
                            "Effect":"Allow",
                        "Principal": {
                                "AWS": "*"
                            },
                         "Action":["s3:GetObject"],
                        "Resource":["arn:aws:s3:::signv4test/*"]
                        }]
                    }
    }
    ```

??? note "createBucketACL"
    The createBucketACL operation uses the ACL sub-resource to set the permissions on an existing bucket using access control lists (ACL). See the [related API documentation](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketPUTacl.html) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>bucketUrl</td>
            <td>The URL of the bucket.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>ownerId</td>
            <td>The ID of the bucket owner.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>ownerDisplayName</td>
            <td>The screen name of the bucket owner.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>accessControlList</td>
            <td>Container for ACL information, which includes the following:
                <ul>
                    <li>Grant: Container for the grantee and permissions.
                        <ul>
                            <li>Grantee: The subject whose permissions are being set.
                                <ul>
                                    <li>ID: ID of the grantee.</li>
                                    <li>DisplayName: Screen name of the grantee.</li>
                                </ul>
                            </li>
                            <li>Permission: Specifies the permission to give to the grantee.</li>
                        </ul>
                    </li>
                </ul>
            </td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazons3.createBucketACL>
        <bucketUrl>{$ctx:bucketUrl}</bucketUrl>
        <ownerId>{$ctx:ownerId}</ownerId>
        <ownerDisplayName>{$ctx:ownerDisplayName}</ownerDisplayName>
        <accessControlList>{$ctx:accessControlList}</accessControlList>
    </amazons3.createBucketACL>
    ```
    
    **Sample request**

    ```xml
    <createBucketACL>
        <accessKeyId>AKIXXXXXXXXXG5EA</accessKeyId>
        <secretAccessKey>qHZXXXXXXXDYQc4oMQXXXOj+340pXXX23s</secretAccessKey>
        <methodType>PUT</methodType>
        <contentType>application/xml</contentType>
        <addCharset>false</addCharset>
        <contentLength></contentLength>
        <contentMD5></contentMD5>
        <bucketName>signv4test</bucketName>
        <isXAmzDate>true</isXAmzDate>
        <xAmzSecurityToken></xAmzSecurityToken>
        <host>s3.us-east-2.amazonaws.com</host>
        <region>us-east-2</region>
        <expect></expect>
        <bucketUrl>http://s3.us-east-2.amazonaws.com/signv4test</bucketUrl>
        <ownerId>9a48e6b16816cc75df306d35bb5d0bd0778b61fbf49b8ef4892143197c84a867</ownerId>
        <ownerDisplayName>admin+aws+connectors+secondary</ownerDisplayName>
        <accessControlList>
            <Grants>
                <Grant>
                    <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="CanonicalUser">
                        <ID>9a48e6b16816cc75df306d35bb5d0bd0778b61fbf49b8ef4892143197c84a867</ID>
                        <DisplayName>admin+aws+connectors+secondary</DisplayName>
                    </Grantee>
                    <Permission>FULL_CONTROL</Permission>
                </Grant>
                <Grant>
                    <Grantee xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance" xsi:type="Group">
                        <URI xmlns="">http://acs.amazonaws.com/groups/global/AllUsers</URI>
                    </Grantee>
                    <Permission xmlns="">READ</Permission>
                </Grant>
            </Grants>
        </accessControlList>
    </createBucketACL>
    ```

??? note "createBucketLifecycle"
    The createBucketLifecycle operation uses the acl subresource to set the permissions on an existing bucket using access control lists (ACL). See the [related API documentation](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketPUTlifecycle.html) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>bucketUrl</td>
            <td>The URL of the bucket.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>lifecycleConfiguration</td>
            <td>Container for lifecycle rules, which includes the following:
                <ul>
                    <li>Rule: Container for a lifecycle rule.
                        <ul>
                            <li>ID: Unique identifier for the rule. The value cannot be longer than 255 characters.</li>
                            <li>Prefix: Object key prefix identifying one or more objects to which the rule applies.</li>
                            <li>Status: If Enabled, Amazon S3 executes the rule as scheduled. If Disabled, Amazon S3 ignores the rule.</li>
                            <li>Transition: This action specifies a period in the objects' lifetime when Amazon S3 should transition them to the STANDARD_IA or the GLACIER storage class.
                                <ul>
                                    <li>Days: Specifies the number of days after object creation when the specific rule action takes effect.</li>
                                    <li>StorageClass: Specifies the Amazon S3 storage class to which you want the object to transition.</li>
                                </ul>
                            </li>
                            <li>Expiration: This action specifies a period in an object's lifetime when Amazon S3 should take the appropriate expiration action.
                                <ul>
                                    <li>Days: Specifies the number of days after object creation when the specific rule action takes effect.</li>
                                </ul>
                            </li>
                        </ul>
                    </li>
                </ul>
            </td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazons3.createBucketLifecycle>
        <bucketUrl>{$ctx:bucketUrl}</bucketUrl>
        <lifecycleConfiguration>{$ctx:lifecycleConfiguration}</lifecycleConfiguration>
    </amazons3.createBucketLifecycle>
    ```
    
    **Sample request**

    ```xml
    <createBucketLifecycle>
        <accessKeyId>AKXXXXXXXXXXX5EA</accessKeyId>
        <secretAccessKey>qHXXXXXXXXXXXqQc4oMQMnAOj+33XXXXXDPO2s</secretAccessKey>
        <region>us-east-2</region>
        <methodType>PUT</methodType>
        <contentType>application/xml</contentType>
        <isXAmzDate>true</isXAmzDate>
        <bucketUrl>http://s3.us-east-2.amazonaws.com/signv4test</bucketUrl>
        <contentLength>0</contentLength>
        <contentMD5></contentMD5>
        <bucketName>signv4test</bucketName>
        <host>s3.us-east-2.amazonaws.com</host>
        <expect></expect>
        <xAmzSecurityToken></xAmzSecurityToken>
        <xAmzAcl>public-read</xAmzAcl>
        <xAmzGrantRead></xAmzGrantRead>
        <xAmzGrantWrite></xAmzGrantWrite>
        <xAmzGrantReadAcp></xAmzGrantReadAcp>
        <xAmzGrantWriteAcp></xAmzGrantWriteAcp>
        <xAmzGrantFullControl></xAmzGrantFullControl>
        <lifecycleConfiguration>
            <Rule>
                <ID>id1</ID>
                <Prefix>documents/</Prefix>
                <Status>Enabled</Status>
                <Transition>
                    <Days>30</Days>
                    <StorageClass>GLACIER</StorageClass>
                </Transition>
            </Rule>
            <Rule>
                <ID>id2</ID>
                <Prefix>logs/</Prefix>
                <Status>Enabled</Status>
                <Expiration>
                    <Days>365</Days>
                </Expiration>
            </Rule>
        </lifecycleConfiguration>
    </createBucketLifecycle>
    ```

??? note "createBucketReplication"
    The createBucketReplication operation uses the acl subresource to set the permissions on an existing bucket using access control lists (ACL). See the [related API documentation](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketPUTreplication.html) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>bucketUrl</td>
            <td>The URL of the bucket.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>rules</td>
            <td>Container for replication rules, which includes the following:
                <ul>
                    <li>Rule: Container for information about a particular replication rule.
                        <ul>
                            <li>ID: Unique identifier for the rule. The value cannot be longer than 255 characters.</li>
                            <li>Prefix: Object key prefix identifying one or more objects to which the rule applies.</li>
                            <li>Status: The rule is ignored if status is not Enabled..</li>
                            <li>Destination: Container for destination information.
                                <ul>
                                    <li>Bucket:Amazon resource name (ARN) of the bucket where you want Amazon S3 to store replicas of the object identified by the rule.</li>
                                </ul>
                            </li>
                        </ul>
                    </li>
                </ul>
            </td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazons3.createBucketReplication>
        <role>{$ctx:role}</role>
        <bucketUrl>{$ctx:bucketUrl}</bucketUrl>
        <rules>{$ctx:rules}</rules>
    </amazons3.createBucketReplication>
    ```
    
    **Sample request**

    ```xml
    <createBucketReplication>
        <accessKeyId>AKIXXXXXHXQXXG5XX</accessKeyId>
        <secretAccessKey>qHXXBXXXXASYQc4oMCEOj+343HD82s</secretAccessKey>
        <region>us-east-2</region>
        <methodType>PUT</methodType>
        <contentType>application/xml</contentType>
        <isXAmzDate>true</isXAmzDate>
        <bucketUrl>http://s3.us-east-2.amazonaws.com/signv4test</bucketUrl>
        <contentLength>0</contentLength>
        <contentMD5></contentMD5>
        <bucketName>signv4test</bucketName>
        <host>s3.us-east-2.amazonaws.com</host>
        <expect></expect>
        <xAmzSecurityToken></xAmzSecurityToken>
        <xAmzAcl>public-read</xAmzAcl>
        <xAmzGrantRead></xAmzGrantRead>
        <xAmzGrantWrite></xAmzGrantWrite>
        <xAmzGrantReadAcp></xAmzGrantReadAcp>
        <xAmzGrantWriteAcp></xAmzGrantWriteAcp>
        <xAmzGrantFullControl></xAmzGrantFullControl>
        <role>arn:aws:iam::35667example:role/CrossRegionReplicationRoleForS3</role>
        <rules>
            <Rule>
                <ID>id1</ID>
                <Prefix>documents/</Prefix>
                <Status>Enabled</Status>
                <Destination>
                    <Bucket>arn:aws:s3:::signv4testq23aa1</Bucket>
                </Destination>
            </Rule>
        </rules>
    </createBucketReplication>
    ```

??? note "createBucketTagging"
    The createBucketTagging operation uses the tagging subresource to add a set of tags to an existing bucket. Use tags to organize your AWS bill to reflect your own cost structure. To do this, sign up to get your AWS account bill with tag key values included. Then, to see the cost of combined resources, organize your billing information according to resources with the same tag key values. For example, you can tag several resources with a specific application name, and then organize your billing information to see the total cost of that application across several services. See the [related API documentation](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketPUTtagging.html) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>bucketUrl</td>
            <td>The URL of the bucket.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>tagSet</td>
            <td>Container for a set of tags, which includes the following:
                <ul>
                    <li>Tag: Container for tag information.
                        <ul>
                            <li>Key: Name of the tag.</li>
                            <li>Value: Value of the tag.</li>
                        </ul>
                    </li>
                </ul>
            </td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazons3.createBucketTagging>
        <bucketUrl>{$ctx:bucketUrl}</bucketUrl>
        <tagSet>{$ctx:tagSet}</tagSet>
    </amazons3.createBucketTagging>
    ```
    
    **Sample request**

    ```xml
    <createBucketTagging>
        <accessKeyId>AKIXXXXXHXQXXG5XX</accessKeyId>
        <secretAccessKey>qHXXBXXXXASYQc4oMCEOj+343HD82s</secretAccessKey>
        <region>us-east-2</region>
        <methodType>PUT</methodType>
        <contentType>application/xml</contentType>
        <isXAmzDate>true</isXAmzDate>
        <bucketUrl>http://s3.us-east-2.amazonaws.com/signv4test</bucketUrl>
        <contentLength>0</contentLength>
        <contentMD5></contentMD5>
        <bucketName>signv4test</bucketName>
        <host>s3.us-east-2.amazonaws.com</host>
        <expect></expect>
        <xAmzSecurityToken></xAmzSecurityToken>
        <xAmzAcl>public-read</xAmzAcl>
        <xAmzGrantRead></xAmzGrantRead>
        <xAmzGrantWrite></xAmzGrantWrite>
        <xAmzGrantReadAcp></xAmzGrantReadAcp>
        <xAmzGrantWriteAcp></xAmzGrantWriteAcp>
        <xAmzGrantFullControl></xAmzGrantFullControl>
        <tagSet>
            <Tag>
                <Key>Project</Key>
                <Value>Project One</Value>
            </Tag>
            <Tag>
                <Key>User</Key>
                <Value>jsmith</Value>
            </Tag>
        </tagSet>
    </createBucketTagging>
    ```

??? note "createBucketRequestPayment"
    The createBucketRequestPayment operation uses the requestPayment subresource to set the request payment configuration of a bucket. By default, the bucket owner pays for downloads from the bucket. This configuration parameter enables the bucket owner (only) to specify that the person requesting the download will be charged for the download. See the [related API documentation](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTrequestPaymentPUT.html) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>bucketUrl</td>
            <td>The URL of the bucket.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>payer</td>
            <td>Specifies who pays for the download and request fees.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazons3.createBucketRequestPayment>
        <bucketUrl>{$ctx:bucketUrl}</bucketUrl>
        <payer>{$ctx:payer}</payer>
    </amazons3.createBucketRequestPayment>
    ```
    
    **Sample request**

    ```xml
    <createBucketRequestPayment>
        <accessKeyId>AKXXXXXXXXXXX5EA</accessKeyId>
        <secretAccessKey>qHXXXXXXXXXXXqQc4oMQMnAOj+33XXXXXDPO2s</secretAccessKey>
        <region>us-east-2</region>
        <methodType>PUT</methodType>
        <contentType>application/xml</contentType>
        <isXAmzDate>true</isXAmzDate>
        <bucketUrl>http://s3.us-east-2.amazonaws.com/signv4test</bucketUrl>
        <contentLength>0</contentLength>
        <contentMD5></contentMD5>
        <bucketName>signv4test</bucketName>
        <host>s3.us-east-2.amazonaws.com</host>
        <expect></expect>
        <xAmzSecurityToken></xAmzSecurityToken>
        <xAmzAcl>public-read</xAmzAcl>
        <xAmzGrantRead></xAmzGrantRead>
        <xAmzGrantWrite></xAmzGrantWrite>
        <xAmzGrantReadAcp></xAmzGrantReadAcp>
        <xAmzGrantWriteAcp></xAmzGrantWriteAcp>
        <xAmzGrantFullControl></xAmzGrantFullControl>
        <payer>Requester</payer>
    </createBucketRequestPayment>
    ```

??? note "createBucketVersioning"
    The createBucketVersioning operation uses the requestPayment subresource to set the request payment configuration of a bucket. By default, the bucket owner pays for downloads from the bucket. This configuration parameter enables the bucket owner (only) to specify that the person requesting the download will be charged for the download. See the [related API documentation](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketPUTVersioningStatus.html) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>bucketUrl</td>
            <td>The URL of the bucket.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>status</td>
            <td>Sets the versioning state of the bucket.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>mfaDelete</td>
            <td>Specifies whether MFA Delete is enabled in the bucket versioning configuration.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazons3.createBucketVersioning>
        <bucketUrl>{$ctx:bucketUrl}</bucketUrl>
        <status>{$ctx:status}</status>
        <mfaDelete>{$ctx:mfaDelete}</mfaDelete>
    </amazons3.createBucketVersioning>
    ```
    
    **Sample request**

    ```xml
    <createBucketVersioning>
        <accessKeyId>AKIXXXXXHXQXXG5XX</accessKeyId>
        <secretAccessKey>qHXXBXXXXASYQc4oMCEOj+343HD82s</secretAccessKey>
        <region>us-east-2</region>
        <methodType>PUT</methodType>
        <contentType>application/xml</contentType>
        <isXAmzDate>true</isXAmzDate>
        <bucketUrl>http://s3.us-east-2.amazonaws.com/signv4test</bucketUrl>
        <contentLength>0</contentLength>
        <contentMD5></contentMD5>
        <bucketName>signv4test</bucketName>
        <host>s3.us-east-2.amazonaws.com</host>
        <expect></expect>
        <xAmzSecurityToken></xAmzSecurityToken>
        <xAmzAcl>public-read</xAmzAcl>
        <xAmzGrantRead></xAmzGrantRead>
        <xAmzGrantWrite></xAmzGrantWrite>
        <xAmzGrantReadAcp></xAmzGrantReadAcp>
        <xAmzGrantWriteAcp></xAmzGrantWriteAcp>
        <xAmzGrantFullControl></xAmzGrantFullControl>
        <status>Enabled</status>
    </createBucketVersioning>
    ```

??? note "deleteBucket"
    The deleteBucket implementation of the DELETE operation deletes the bucket named in the URI. All objects (including all object versions and Delete Markers) in the bucket must be deleted before the bucket itself can be deleted. When calling init before this operation, the following headers should be removed: xAmzAcl, x AmzGrantRead, xAmzGrantWrite, xAmzGrantReadAcp, xAmzGrantWriteAcp, and xAmzGrantFullControl. See the [related API documentation](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketDELETE.html) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>bucketUrl</td>
            <td>The URL of the bucket.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazons3.deleteBucket>
        <bucketUrl>{$ctx:bucketUrl}</bucketUrl>
    <amazons3.deleteBucket>
    ```
    
    **Sample request**

    ```xml
    <deleteBucket>
        <accessKeyId>AKIAIGURZM7SDFGJ7TRO6KSFSQ</accessKeyId>
        <secretAccessKey>asAX8CJoDKzeOgfdgd0Ve5dMCFk4STUFDdfgdgRHkGX6m0CcY</secretAccessKey>
        <methodType>DELETE</methodType>
        <region>us-east-2</region>
        <contentLength></contentLength>
        <contentType>application/xml</contentType>
        <contentMD5></contentMD5>
        <expect></expect>
        <isXAmzDate>true</isXAmzDate>
        <xAmzSecurityToken></xAmzSecurityToken>
        <bucketName>signv4test</bucketName>
        <host>s3.us-east-2.amazonaws.com</host>
        <bucketUrl>http://s3.us-east-2.amazonaws.com/signv4test</bucketUrl>
    </deleteBucket>
    ```

??? note "deleteBucketPolicy"
    The deleteBucketPolicy implementation of the DELETE operation deletes the policy on a specified bucket. To use the operation, you must have DeletePolicy permissions on the specified bucket and be the bucket owner. If there are no DeletePolicy permissions, Amazon S3 returns a 403 Access Denied error. If there is the correct permission, but you are not the bucket owner, Amazon S3 returns a 405 Method Not Allowed error. If the bucket does not have a policy, Amazon S3 returns a 204 No Content error. There are restrictions about who can create bucket policies and which objects in a bucket they can apply to. When calling init before this operation, the following headers should be removed: xAmzAcl, x AmzGrantRead, xAmzGrantWrite, xAmzGrantReadAcp, xAmzGrantWriteAcp, and xAmzGrantFullControl. See the [related API documentation](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketDELETE.html) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>bucketUrl</td>
            <td>The URL of the bucket.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazons3.deleteBucketPolicy>
        <bucketUrl>{$ctx:bucketUrl}</bucketUrl>
    <amazons3.deleteBucketPolicy>
    ```
    
    **Sample request**

    ```xml
    <deleteBucketPolicy>
        <accessKeyId>AKIAQEIGURZSDFDM7GJ7TRO6KQ</accessKeyId>
        <secretAccessKey>asAX8CJoDvcvKzeOd0Ve5dMjkjCFk4STUFDRHkGX6m0CcY</secretAccessKey>
        <methodType>DELETE</methodType>
        <contentType>application/xml</contentType>
        <contentLength>256</contentLength>
        <contentMD5></contentMD5>
        <isXAmzDate>true</isXAmzDate>
        <xAmzSecurityToken></xAmzSecurityToken>
        <expect></expect>
        <region>us-east-2</region>
        <bucketName>signv4test</bucketName>
        <host>s3.us-east-2.amazonaws.com</host>
        <bucketUrl>http://s3.us-east-2.amazonaws.com/signv4test</bucketUrl>
    </deleteBucketPolicy>
    ```

??? note "deleteBucketCors"
    The deleteBucketCors operation deletes the cors configuration information set for the bucket. To use this operation, you must have permission to perform the s3:PutCORSConfiguration action. The bucket owner has this permission by default and can grant this permission to others. See the [related API documentation](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketDELETEcors.html) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>bucketUrl</td>
            <td>The URL of the bucket.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazons3.deleteBucketCors>
        <bucketUrl>{$ctx:bucketUrl}</bucketUrl>
    <amazons3.deleteBucketCors>
    ```
    
    **Sample request**

    ```xml
    <deleteBucketCors>
        <accessKeyId>AKIAIGURZMSDFD7GJ7TRO6KQDFD</accessKeyId>
        <secretAccessKey>asAX8CJoDKzeOd0Ve5dfgdgdfMCFk4STUFDRHSFSDkGX6m0CcY</secretAccessKey>
        <methodType>PUT</methodType>
        <contentType>application/xml</contentType>
        <isXAmzDate>true</isXAmzDate>
        <contentLength>0</contentLength>
        <contentMD5></contentMD5>
        <region>us-east-2</region>
        <bucketName>signv4test</bucketName>
        <host>s3.us-east-2.amazonaws.com</host>
        <bucketUrl>http://s3.us-east-2.amazonaws.com/signv4test</bucketUrl>
        <expect></expect>
        <xAmzSecurityToken></xAmzSecurityToken>
        <xAmzAcl>public-read</xAmzAcl>
        <xAmzGrantRead></xAmzGrantRead>
        <xAmzGrantWrite></xAmzGrantWrite>
        <xAmzGrantReadAcp></xAmzGrantReadAcp>
        <xAmzGrantWriteAcp></xAmzGrantWriteAcp>
        <xAmzGrantFullControl></xAmzGrantFullControl>
    </deleteBucketCors>
    ```

??? note "deleteBucketLifecycle"
    The deleteBucketLifecycle operation deletes the lifecycle configuration from the specified bucket. Amazon S3 removes all the lifecycle configuration rules in the lifecycle subresource associated with the bucket. Your objects never expire, and Amazon S3 no longer automatically deletes any objects on the basis of rules contained in the deleted lifecycle configuration. To use this operation, you must have permission to perform the s3:PutLifecycleConfiguration action. By default, the bucket owner has this permission and the bucket owner can grant this permission to others. See the [related API documentation](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketDELETElifecycle.html) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>bucketUrl</td>
            <td>The URL of the bucket.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazons3.deleteBucketLifecycle>
        <bucketUrl>{$ctx:bucketUrl}</bucketUrl>
    <amazons3.deleteBucketLifecycle>
    ```
    
    **Sample request**

    ```xml
    <deleteBucketLifecycle>
        <accessKeyId>AKIAIGURZMSDFD7GJ7TRO6KQDFD</accessKeyId>
        <secretAccessKey>asAX8CJoDKzeOd0Ve5dfgdgdfMCFk4STUFDRHSFSDkGX6m0CcY</secretAccessKey>
        <methodType>PUT</methodType>
        <contentType>application/xml</contentType>
        <isXAmzDate>true</isXAmzDate>
        <contentLength>0</contentLength>
        <contentMD5></contentMD5>
        <region>us-east-2</region>
        <bucketName>signv4test</bucketName>
        <host>s3.us-east-2.amazonaws.com</host>
        <bucketUrl>http://s3.us-east-2.amazonaws.com/signv4test</bucketUrl>
        <expect></expect>
        <xAmzSecurityToken></xAmzSecurityToken>
        <xAmzAcl>public-read</xAmzAcl>
        <xAmzGrantRead></xAmzGrantRead>
        <xAmzGrantWrite></xAmzGrantWrite>
        <xAmzGrantReadAcp></xAmzGrantReadAcp>
        <xAmzGrantWriteAcp></xAmzGrantWriteAcp>
        <xAmzGrantFullControl></xAmzGrantFullControl>
    </deleteBucketLifecycle>
    ```

??? note "deleteBucketReplication"
    The deleteBucketReplication operation deletes the replication sub-resource associated with the specified bucket. This operation requires permission for the s3:DeleteReplicationConfiguration action. See the [related API documentation](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketDELETEreplication.html) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>bucketUrl</td>
            <td>The URL of the bucket.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazons3.deleteBucketReplication>
        <bucketUrl>{$ctx:bucketUrl}</bucketUrl>
    <amazons3.deleteBucketReplication>
    ```
    
    **Sample request**

    ```xml
    <deleteBucketReplication>
        <accessKeyId>AKIAIGURZMSDFD7GJ7TRO6KQDFD</accessKeyId>
        <secretAccessKey>asAX8CJoDKzeOd0Ve5dfgdgdfMCFk4STUFDRHSFSDkGX6m0CcY</secretAccessKey>
        <methodType>PUT</methodType>
        <contentType>application/xml</contentType>
        <isXAmzDate>true</isXAmzDate>
        <contentLength>0</contentLength>
        <contentMD5></contentMD5>
        <region>us-east-2</region>
        <bucketName>signv4test</bucketName>
        <host>s3.us-east-2.amazonaws.com</host>
        <bucketUrl>http://s3.us-east-2.amazonaws.com/signv4test</bucketUrl>
        <expect></expect>
        <xAmzSecurityToken></xAmzSecurityToken>
        <xAmzAcl>public-read</xAmzAcl>
        <xAmzGrantRead></xAmzGrantRead>
        <xAmzGrantWrite></xAmzGrantWrite>
        <xAmzGrantReadAcp></xAmzGrantReadAcp>
        <xAmzGrantWriteAcp></xAmzGrantWriteAcp>
        <xAmzGrantFullControl></xAmzGrantFullControl>
    </deleteBucketReplication>
    ```

??? note "deleteBucketTagging"
    The deleteBucketTagging operation uses the tagging sub-resource to remove a tag set from the specified bucket. To use this operation, you must have permission to perform the s3:PutBucketTagging action. By default, the bucket owner has this permission and can grant this permission to others. See the [related API documentation](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketDELETEtagging.html) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>bucketUrl</td>
            <td>The URL of the bucket.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazons3.deleteBucketTagging>
        <bucketUrl>{$ctx:bucketUrl}</bucketUrl>
    <amazons3.deleteBucketTagging>
    ```
    
    **Sample request**

    ```xml
    <deleteBucketTagging>
        <accessKeyId>AKIAIGURZMSDFD7GJ7TRO6KQDFD</accessKeyId>
        <secretAccessKey>asAX8CJoDKzeOd0Ve5dfgdgdfMCFk4STUFDRHSFSDkGX6m0CcY</secretAccessKey>
        <methodType>PUT</methodType>
        <contentType>application/xml</contentType>
        <isXAmzDate>true</isXAmzDate>
        <contentLength>0</contentLength>
        <contentMD5></contentMD5>
        <region>us-east-2</region>
        <bucketName>signv4test</bucketName>
        <host>s3.us-east-2.amazonaws.com</host>
        <bucketUrl>http://s3.us-east-2.amazonaws.com/signv4test</bucketUrl>
        <expect></expect>
        <xAmzSecurityToken></xAmzSecurityToken>
        <xAmzAcl>public-read</xAmzAcl>
        <xAmzGrantRead></xAmzGrantRead>
        <xAmzGrantWrite></xAmzGrantWrite>
        <xAmzGrantReadAcp></xAmzGrantReadAcp>
        <xAmzGrantWriteAcp></xAmzGrantWriteAcp>
        <xAmzGrantFullControl></xAmzGrantFullControl>
    </deleteBucketTagging>
    ```

??? note "deleteBucketWebsiteConfiguration"
    The deleteBucketWebsiteConfiguration operation removes the website configuration for a bucket. Amazon S3 returns a 207 OK response upon successfully deleting a website configuration on the specified bucket. It will give a 200 response if the website configuration you are trying to delete does not exist on the bucket, and a 404 response if the bucket itself does not exist. This DELETE operation requires the S3: DeleteBucketWebsite permission. By default, only the bucket owner can delete the website configuration attached to a bucket. However, bucket owners can grant other users permission to delete the website configuration by writing a bucket policy granting them the S3: DeleteBucketWebsite permission. When calling init before this operation, the following headers should be removed: xAmzAcl, x AmzGrantRead, xAmzGrantWrite, xAmzGrantReadAcp, xAmzGrantWriteAcp, and xAmzGrantFullControl. See the [related API documentation](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketDELETEwebsite.html) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>bucketUrl</td>
            <td>The URL of the bucket.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazons3.deleteBucketWebsiteConfiguration>
        <bucketUrl>{$ctx:bucketUrl}</bucketUrl>
    <amazons3.deleteBucketWebsiteConfiguration>
    ```
    
    **Sample request**

    ```xml
    <deleteBucketWebsiteConfiguration>
        <accessKeyId>AKIAIGURZM7GDFDJ7TRO6KQDFD</accessKeyId>
        <secretAccessKey>asAdfsX8CJoDKzeOd0Ve5dMCdfsdFk4STUFDRHkdsfGX6m0CcY</secretAccessKey>
        <methodType>DELETE</methodType>
        <contentType>application/xml</contentType>
        <contentLength></contentLength>
        <region>us-east-2</region>
        <bucketName>signv4test</bucketName>
        <host>s3.us-east-2.amazonaws.com</host>
        <bucketUrl>http://s3.us-east-2.amazonaws.com/signv4test</bucketUrl>
        <contentMD5></contentMD5>
        <isXAmzDate>true</isXAmzDate>
        <xAmzSecurityToken></xAmzSecurityToken>
        <expect></expect>
    </deleteBucketWebsiteConfiguration>
    ```

??? note "getObjectsInBucket"
    The getObjectsInBucket implementation of the GET operation returns some or all (up to 1000) of the objects in a bucket. The request parameters act as selection criteria to return a subset of the objects in a bucket. To use this implementation of the operation, the user must have READ access to the bucket. When calling init before this operation, the following headers should be removed: xAmzAcl, x AmzGrantRead, xAmzGrantWrite, xAmzGrantReadAcp, xAmzGrantWriteAcp, and xAmzGrantFullControl. See the [related API documentation](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketDELETEwebsite.html) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>bucketUrl</td>
            <td>The URL of the bucket.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>delimiter</td>
            <td>A delimiter is a character used to group keys. All keys that contain the same string between the prefix, if specified, and the first occurrence of the delimiter after the prefixes are grouped under a single result element CommonPrefixes. If the prefix parameter is not specified, the substring starts at the beginning of the key. The keys that are grouped under the CommonPrefixesresult element are not returned elsewhere in the response.</td>
            <td>Optional</td>
        </tr>
        <tr>
            <td>encodingType</td>
            <td>Requests Amazon S3 to encode the response and specifies the encoding method to use. An object key can contain any Unicode character. However, XML 1.0 parser cannot parse some characters such as characters with an ASCII value from 0 to 10. For characters that are not supported in XML 1.0, this parameter can be added to request Amazon S3 to encode the keys in the response.</td>
            <td>Optional</td>
        </tr>
        <tr>
            <td>marker</td>
            <td>Specifies the key to start with when listing objects in a bucket. Amazon S3 lists objects in alphabetical order.</td>
            <td>Optional</td>
        </tr>
        <tr>
            <td>maxKeys</td>
            <td>Sets the maximum number of keys returned in the response body. The response might contain fewer keys but will never contain more.</td>
            <td>Optional</td>
        </tr>
        <tr>
            <td>prefix</td>
            <td>Limits the response to keys that begin with the specified prefix.</td>
            <td>Optional</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazons3.getObjectsInBucket>
        <bucketUrl>{$ctx:bucketUrl}</bucketUrl>
        <delimiter>{$ctx:delimiter}</delimiter>
        <encodingType>{$ctx:encodingType}</encodingType>
        <marker>{$ctx:marker}</marker>
        <maxKeys>{$ctx:maxKeys}</maxKeys>
        <prefix>{$ctx:prefix}</prefix>
    </amazons3.getObjectsInBucket>
    ```
    
    **Sample request**

    ```xml
    <getObjectsInBucket>
        <accessKeyId>AKIXXXXXHXQXXG5XX</accessKeyId>
        <secretAccessKey>qHXXBXXXXASYQc4oMCEOj+343HD82s</secretAccessKey>
        <region>us-east-2</region>
        <methodType>GET</methodType>
        <contentType>application/xml</contentType>
        <contentLength></contentLength>
        <contentMD5></contentMD5>
        <bucketName>signv4test</bucketName>
        <isXAmzDate>true</isXAmzDate>
        <xAmzSecurityToken></xAmzSecurityToken>
        <host>s3.us-east-2.amazonaws.com</host>
        <expect></expect>
        <bucketUrl>http://s3.us-east-2.amazonaws.com/signv4test</bucketUrl>
        <prefix>t</prefix>
        <marker>obj</marker>
        <expect/>
        <maxKeys>3</maxKeys>
        <encodingType>url</encodingType>
        <delimiter>images</delimiter>
    </getObjectsInBucket>
    ```

??? note "getBucketLifeCycle"
    The getBucketLifeCycle operation returns the lifecycle configuration information set on the bucket. To use this operation, permissions should be given to perform the s3:GetLifecycleConfiguration action. The bucket owner has this permission by default and can grant this permission to others. There is usually some time lag before lifecycle configuration deletion is fully propagated to all the Amazon S3 systems. When calling init before this operation, the following headers should be removed: xAmzAcl, xAmzGrantRead, xAmzGrantWrite, xAmzGrantReadAcp, xAmzGrantWriteAcp, and xAmzGrantFullControl. See the [related API documentation](http://docs.aws.amazon.com/AmazonS3/latest/API/RESTBucketDELETEwebsite.html) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>bucketUrl</td>
            <td>The URL of the bucket.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <amazons3.getBucketLifeCycle>
        <bucketUrl>{$ctx:bucketUrl}</bucketUrl>
    </amazons3.getObjectsInBucket>
    ```
    
    **Sample request**

    ```xml
    <getBucketLifeCycle>
        <accessKeyId>AKIXXXXXHXQXXG5XX</accessKeyId>
        <secretAccessKey>qHXXBXXXXASYQc4oMCEOj+343HD82s</secretAccessKey>
        <region>us-east-2</region>
        <methodType>GET</methodType>
        <contentType>application/xml</contentType>
        <contentLength></contentLength>
        <contentMD5></contentMD5>
        <bucketName>signv4test</bucketName>
        <isXAmzDate>true</isXAmzDate>
        <xAmzSecurityToken></xAmzSecurityToken>
        <host>s3.us-east-2.amazonaws.com</host>
        <expect></expect>
        <bucketUrl>http://s3.us-east-2.amazonaws.com/signv4test</bucketUrl>
    </getBucketLifeCycle>
    ```