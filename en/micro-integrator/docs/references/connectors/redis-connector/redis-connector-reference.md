# Redis Connector Reference

To use the Redis connector, add the <redis.init> element in your configuration before carrying out any other Redis operations. 

??? note "redis.init"
    The redis.init operation initializes the connector to interact with Redis.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisHost</td>
            <td>The Redis host name (default localhost).</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>redisPort</td>
            <td>The port on which the Redis server is running (the default port is 6379).</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>redisTimeout</td>
            <td>The server TTL (Time to Live) in milliseconds.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**
    ```xml
    <redis.init>
        <redisHost>{$ctx:redisHost}</redisHost>
        <redisPort>{$ctx:redisPort}</redisPort>
        <redisTimeout>{$ctx:redisTimeout}</redisTimeout>
    </redis.init>
    ```

---

### Connection Commands

??? note "echo"
    The echo operation returns a specified string. See the [related documentation](https://redis.io/commands/echo) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisMessage</td>
            <td>The message that you want to echo.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <redis.echo>
        <redisMessage>{$ctx:redisMessage}</redisMessage>
    </redis.echo>
    ```
    
    **Sample request**

    ```json
    {
        "redisMessage":"sampleMessage"
    }
    ```

??? note "ping"
    The ping operation pings the server to verify whether the connection is still alive. See the [related documentation](https://redis.io/commands/ping) for more information.

    **Sample configuration**

    ```xml
    <redis.ping/>
    ```
    
    **Sample request**

    An empty request can be handled by the ping operation.

??? note "quit"
    The quit operation closes the connection to the server. See the [related documentation](https://redis.io/commands/quit) for more information.

    **Sample configuration**

    ```xml
    <redis.quit/>
    ```
    
    **Sample request**

    An empty request can be handled by the quit operation.

### Hashes

??? note "hDel"
    The hDel operation deletes one or more specified hash fields. See the [related documentation](https://redis.io/commands/hdel) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisKey</td>
            <td>The name of the key where the hash is stored.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>redisFields</td>
            <td>The fields that you want to delete.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <redis.hDel>
        <redisKey>{$ctx:redisKey}</redisKey>
        <redisFields>{$ctx:redisFields}</redisFields>
    </redis.hDel>
    ```
    
    **Sample request**

    ```json
    {
        "redisKey":"sampleKey",
        "redisFields":"sampleField1 sampleField2"
    }
    ```

??? note "hExists"
    The hExists operation determines whether a specified hash field exists. See the [related documentation](https://redis.io/commands/hexists) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisKey</td>
            <td>The name of the key where the hash is stored.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>redisFields</td>
            <td>The fields that determine existence.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <redis.hExists>
        <redisKey>{$ctx:redisKey}</redisKey>
        <redisFields>{$ctx:redisFields}</redisFields>
    </redis.hExists>
    ```
    
    **Sample request**

    ```json
    {
        "redisKey":"sampleKey",
        "redisFields":"sampleField"
    }
    ```

??? note "hGet"
    The hGet operation retrieves the value of a particular field in a hash stored in a specified key. See the [related documentation](https://redis.io/commands/hget) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisKey</td>
            <td>The name of the key where the hash is stored.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>redisFields</td>
            <td>The field for which you want to retrieve the value.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <redis.hGet>
        <redisKey>{$ctx:redisKey}</redisKey>
        <redisFields>{$ctx:redisFields}</redisFields>
    </redis.hGet>
    ```
    
    **Sample request**

    ```json
    {
        "redisKey":"sampleKey",
        "redisFields":"sampleField"
    }
    ```

??? note "hGetAll"
    The hGetAll operation retrieves all the fields and values of a hash stored in a specified key. See the [related documentation](https://redis.io/commands/hgetall) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisKey</td>
            <td>The name of the key where the hash is stored.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <redis.hGetAll>
        <redisKey>{$ctx:redisKey}</redisKey>
    </redis.hGetAll>
    ```
    
    **Sample request**

    ```json
    {
        "redisKey":"sampleKey"
    }
    ```

??? note "hIncrBy"
    The hIncrBy operation increments the integer value of a hash field by the specified amount. See the [related documentation](https://redis.io/commands/hincrby) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisKey</td>
            <td>The name of the key where the hash is stored.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>redisFields</td>
            <td>The hash field for which you want to increment the value.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>redisValue</td>
            <td>The amount by which you want to increment the hash field value.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <redis.hIncrBy>
        <redisKey>{$ctx:redisKey}</redisKey>
        <redisField>{$ctx:redisField}</redisField>
        <redisValue>{$ctx:redisValue}</redisValue>
    </redis.hIncrBy>
    ```
    
    **Sample request**

    ```json
    {
        "redisKey":"sampleKey",
        "redisField":"sampleField",
        "redisValue":"1"
    }
    ```

??? note "hKeys"
    The hKeys operation retrieves all the fields in a hash. See the [related documentation](https://redis.io/commands/hkeys) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisKey</td>
            <td>The name of the key where the hash is stored.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <redis.hKeys>
        <redisKey>{$ctx:redisKey}</redisKey>
    </redis.hKeys>
    ```
    
    **Sample request**

    ```json
    {
        "redisKey":"sampleKey"
    }
    ```

??? note "hLen"
    The hLen operation retrieves the number of fields in a hash. See the [related documentation](https://redis.io/commands/hlen) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisKey</td>
            <td>The name of the key where the hash is stored.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <redis.hKeys>
        <redisKey>{$ctx:redisKey}</redisKey>
    </redis.hKeys>
    ```
    
    **Sample request**

    ```json
    {
        "redisKey":"sampleKey"
    }
    ```

??? note "hMGet"
    The hMGet operation retrieves values associated with each of the specified fields in a hash that is stored in a particular key. See the [related documentation](https://redis.io/commands/hmget) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisKey</td>
            <td>The name of the key where the hash is stored.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>redisFields</td>
            <td>The hash field for which you want to retrieve values.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <redis.hMGet>
        <redisKey>{$ctx:redisKey}</redisKey>
        <redisFields>{$ctx:redisFields}</redisFields>
    </redis.hMGet>
    ```
    
    **Sample request**

    ```json
    {
        "redisKey":"sampleKey",
        "redisFields":"sampleField1 sampleField2"
    }
    ```

??? note "hMSet"
    The hMSet operation sets specified fields to their respective values in the hash stored in a particular key. See the [related documentation](https://redis.io/commands/hmset) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisKey</td>
            <td>The name of the key where the hash is stored.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>redisFieldsValues</td>
            <td>The fields you want to set and their respective values.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <redis.hMSet>
        <redisKey>{$ctx:redisKey}</redisKey>
        <redisFieldsValues>{$ctx:redisFieldsValues}</redisFieldsValues>
    </redis.hMSet>
    ```
    
    **Sample request**

    ```json
    {
        "redisKey":"sampleKey",
        "redisFieldsValues":"sampleField1 sampleValue1 sampleField2 sampleValue2"
    }
    ```

??? note "hSet"
    The hSet operation sets a specific field in a hash to a specified value. See the [related documentation](https://redis.io/commands/hset) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisKey</td>
            <td>The name of the key where the hash is stored.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>redisFields</td>
            <td>The field for which you want to set a value.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>redisValue</td>
            <td>The amount by which you want to increment the hash field value.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <redis.hSet>
        <redisKey>{$ctx:redisKey}</redisKey>
        <redisField>{$ctx:redisField}</redisField>
        <redisValue>{$ctx:redisValue}</redisValue>
    </redis.hSet>
    ```
    
    **Sample request**

    ```json
    {
        "redisKey":"sampleKey",
        "redisField":"sampleField",
        "redisValue":"1"
    }
    ```

??? note "hSetnX"
    The hSetnX operation sets a specified field to a value, only if the field does not already exist in the hash. If field already exists, this operation has no effect. See the [related documentation](https://redis.io/commands/hsetnx) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisKey</td>
            <td>The name of the key where the hash is stored.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>redisFields</td>
            <td>The field for which you want to set a value.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>redisValue</td>
            <td>The amount by which you want to increment the hash field value.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <redis.hSetnX>
        <redisKey>{$ctx:redisKey}</redisKey>
        <redisField>{$ctx:redisField}</redisField>
        <redisValue>{$ctx:redisValue}</redisValue>
    </redis.hSetnX>
    ```
    
    **Sample request**

    ```json
    {
        "redisKey":"sampleKey",
        "redisField":"sampleField",
        "redisValue":"1"
    }
    ```

??? note "hVals"
    The hVals operation retrieves all values in a hash that is stored in a particular key. See the [related documentation](https://redis.io/commands/hvals) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisKey</td>
            <td>The name of the key where the hash is stored.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <redis.hVals>
        <redisKey>{$ctx:redisKey}</redisKey>
    </redis.hVals>
    ```
    
    **Sample request**

    ```json
    {
        "redisKey":"sampleKey"
    }
    ```

### Keys

??? note "del"
    The del operation deletes a specified key if it exists. See the [related documentation](https://redis.io/commands/del) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisKey</td>
            <td>The name of the key that you want to delete.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <redis.del>
        <redisKey>{$ctx:redisKey}</redisKey>
    </redis.del>
    ```
    
    **Sample request**

    ```json
    {
        "redisKey":"sampleKey"
    }
    ```

??? note "exists"
    The exists operation determines whether a specified key exists. See the [related documentation](https://redis.io/commands/exists) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisKey</td>
            <td>The name of the key that you want to determine existence.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <redis.exists>
        <redisKey>{$ctx:redisKey}</redisKey>
    </redis.exists>
    ```
    
    **Sample request**

    ```json
    {
        "redisKey":"sampleKey"
    }
    ```

??? note "expire"
    The expire operation sets a TTL(Time to live) for a key so that the key will automatically delete once it reaches the TTL. The TTL should be specified in seconds. See the [related documentation](https://redis.io/commands/expire) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisKey</td>
            <td>The name of the key that you want to specify a TTL.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>redisSeconds</td>
            <td>The number of seconds representing the TTL that you want to set for the key.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <redis.expire>
        <redisKey>{$ctx:redisKey}</redisKey>
        <redisSeconds>{$ctx:redisSeconds}</redisSeconds>
    </redis.expire>
    ```
    
    **Sample request**

    ```json
    {
        "redisKey":"sampleKey",
        "redisSeconds":"10"
    }
    ```

??? note "expireAt"
    The expireAt operation sets the time after which an existing key should expire. Here the time should be specified as a UNIX timestamp. See the [related documentation](https://redis.io/commands/expireat) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisKey</td>
            <td>The name of the key that you want to set an expiration.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>redisUnixTime</td>
            <td>The time to expire specified in the UNIX timestamp format.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <redis.expire>
        <redisKey>{$ctx:redisKey}</redisKey>
        <redisUnixTime>{$ctx:redisUnixTime}</redisUnixTime>
    </redis.expire>
    ```
    
    **Sample request**

    ```json
    {
        "redisKey":"sampleKey",
        "redisUnixTime":"1293840000"
    }
    ```

??? note "keys"
    The keys operation retrieves all keys that match a specified pattern. See the [related documentation](https://redis.io/commands/keys) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisPattern</td>
            <td>The pattern that you want to match when retrieving keys.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <redis.keys>
        <redisPattern>{$ctx:redisPattern}</redisPattern>
    </redis.keys>
    ```
    
    **Sample request**

    ```json
    {
        "redisPattern":"*"
    }
    ```

??? note "randomKey"
    A sample request with an empty body can be handled by the randomKey operation. See the [related documentation](https://redis.io/commands/randomkey) for more information.
    
    **Sample configuration**

    ```xml
    <redis.randomKey/>
    ```
    
    **Sample request**

    ```json
    {
        "redisPattern":"*"
    }
    ```

??? note "rename"
    The rename operation renames an existing key to a new name that is specified. See the [related documentation](https://redis.io/commands/rename) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisOldKey</td>
            <td>The name of an existing key that you want to rename.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>redisNewKey</td>
            <td>The new name that you want the key to have.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <redis.rename>
        <redisOldKey>{$ctx:redisOldKey}</redisOldKey>
        <redisNewKey>{$ctx:redisNewKey}</redisNewKey>
    </redis.rename>
    ```
    
    **Sample request**

    ```json
    {
        "redisOldKey":"sampleOldKey",
        "redisNewKey":"sampleNewKey"
    }
    ```

??? note "renamenX"
    The renamenX operation renames a key to a new key, only if the new key does not already exist. See the [related documentation](https://redis.io/commands/renamenx) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisOldKey</td>
            <td>The name of an existing key that you want to rename.</td>
            <td>Yes</td>
        </tr>
        <tr>
            <td>redisNewKey</td>
            <td>The new name that you want the key to have.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <redis.renamenX>
        <redisOldKey>{$ctx:redisOldKey}</redisOldKey>
        <redisNewKey>{$ctx:redisNewKey}</redisNewKey>
    </redis.renamenX>
    ```
    
    **Sample request**

    ```json
    {
        "redisOldKey":"sampleOldKey",
        "redisNewKey":"sampleNewKey"
    }
    ```

??? note "ttl"
    The ttl operation retrieves the TTL (Time to Live) value of a specified key. See the [related documentation](https://redis.io/commands/ttl) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisKey</td>
            <td>The name of the key for which you want to retrieve the TTL.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <redis.ttl>
        <redisKey>{$ctx:redisKey}</redisKey>
    </redis.ttl>
    ```
    
    **Sample request**

    ```json
    {
        "redisKey":"sampleKey"
    }
    ```

??? note "type"
    The type operation retrieves the data type of a value stored in a specified key. See the [related documentation](https://redis.io/commands/type) for more information.
    <table>
        <tr>
            <th>Parameter Name</th>
            <th>Description</th>
            <th>Required</th>
        </tr>
        <tr>
            <td>redisKey</td>
            <td>The name of the key that the value is stored.</td>
            <td>Yes</td>
        </tr>
    </table>

    **Sample configuration**

    ```xml
    <redis.type>
        <redisKey>{$ctx:redisKey}</redisKey>
    </redis.type>
    ```
    
    **Sample request**

    ```json
    {
        "redisKey":"sampleKey"
    }
    ```