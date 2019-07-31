# Transforming Data

## Introduction

The Streaming Integrator allows you to perform a wide range of transformations to the input data received. Some of these 
transformations are carried out via operators that are defined inline within the Siddhi application. For the rest of the
 transformations, you can use Siddhi extensions that are available to be downloaded via the [Siddhi Extension Store](https://store.wso2.com/store/assets/analyticsextension/list).
 Most of these extensions are shipped with the Streaming Integrator by default.

## Transform data using inline operators

The operators that you can configure inline within Siddhi applications in order to carry out data transformations are listed in the [Siddhi Query Guide - Inbuild Aggregation Functions section](https://siddhi.io/en/v5.0/docs/query-guide/#select).

To show how an inline operators are configured, let's consider an example where readings from a sensor that indicates 
the temperature of a room every second are transformed to indicate the average tempertature and the average humidity as at each second.

1. Open the Streaming Integrator Studio and start creating a new Siddhi application. For more information, see [Creating a Siddhi Application](../develop/creating-a-Siddhi-Application.md).
2. Enter a name for the Siddhi application as shown below.<br/>
   `@App:name("<Siddhi_Application_Name>)`<br/>
   
   In this example, let's name the application `TemperatureApp`.
   
3. Let's define the input stream to define the schema based on which data is selected to the streaming integration flow.
    1. In this example, let's assume that each event indicates the device ID, the room ID, and the temperature. Therefore, let's define an input stream as follows:
       ```
       define stream TempStream (deviceID long, roomNo int, temp double);
       ```
       
       !!!info
           For more information about defining input streams to receive events, see the [Consuming Data guide](consuming-messages.md).
           
          
4. To do the required transformation, let's add the query as follows:
    1. Add the `from` clause with the name of the input stream to indicate that the events to be processed are taken from the input stream.
       ```jql
       from TempStream
       ```
    2. Add the `to` clause with the name of the output stream to indicate that the processed events are directed to that stream.
      ```jql
      from TempStream
      to OutputStream;
      ```
    3.Add a `select` clause in a line between the `from` and `to` clauses. To derive the average temperature from the temperature, apply the `avg()` to the `temp` 
    attribute, and then specify `avgTemp` as the name with which the result should be output. 
    
      ```jql
      from TempStream
      select roomNo, deviceID, avg(temp) as avgTemp
      to OutputStream;
      ```
    4. To group by a specific attribute (by the `roomNo` attribute in this example), specify it via the `group by` clause as shown below.
      ```jql
      from TempStream
      select roomNo, deviceID, avg(temp) as avgTemp
      group by roomNo
      to OutputStream;
      ```
    
5. Save the Siddhi application. The completed Siddhi application is as follows.

    ```jql
    @App:name("TemperatureApp")
    @App:description("Description of the plan")
    
    define stream TempStream (deviceID long, roomNo int, temp double);
    
    
    from TempStream
    select roomNo, deviceID, avg(temp) as avgTemp
    group by roomNo
    insert into OutputStream;
    ```

Give an example and point to existing inline math and logical operators.



## Transform data using in-built extensions

The Streaming Integrator offers a variety of options to carry out data transformations via in-built extensions. The 
following table describes the complete list of extensions that provide data transformation functionality.

<table class="tg">
  <tr>
    <th class="tg-0pky">Extension</th>
    <th class="tg-0pky">Description</th>
  </tr>
  <tr>
    <td class="tg-0pky">`<a href="https://siddhi-io.github.io/siddhi-execution-math/api/latest/#log-function">Siddhi-execution-math</a>`</td>
    <td class="tg-0pky">Transforms data by performing mathematical operations.</td>
  </tr>
  <tr>
    <td class="tg-0pky">`<a href="https://siddhi-io.github.io/siddhi-execution-unitconversion/">Siddhi-execution-unitconversion</a>`</td>
    <td class="tg-0pky">Performs unit conversions ranging from length, weight, volume, etc.</td>
  </tr>
  <tr>
    <td class="tg-0pky">`<a href="https://siddhi-io.github.io/siddhi-execution-string/">Siddhi-execution-string</a>`</td>
    <td class="tg-0pky">Performs string manipulations.</td>
  </tr>
  <tr>
    <td class="tg-0pky">`<a href="https://siddhi-io.github.io/siddhi-execution-time/">Siddhi-execution-time`</a></td>
    <td class="tg-0pky">Performs time-based transformations such as converting time zones.</td>
  </tr>
  <tr>
    <td class="tg-0pky">`<a href="https://siddhi-io.github.io/siddhi-execution-map/">Siddhi-execution-map</a>`</td>
    <td class="tg-0pky">Converts events into maps and performs transformations such as concatenating and removing attributes.</td>
  </tr>
  <tr>
    <td class="tg-0pky">`<a href="https://siddhi-io.github.io/siddhi-execution-reorder/">Siddhi-execution-reorder</a>`</td>
    <td class="tg-0pky">Rearranges the order of the incoming event flow.</td>
  </tr>
  <tr>
    <td class="tg-0pky">`<a href="https://siddhi-io.github.io/siddhi-execution-json/">Siddhi-execution-json</a>`</td>
    <td class="tg-0pky">Performs manipulations to JSON strings.</td>
  </tr>
</table>

These extensions are shipped with the Streaming Integrator by default. If you want to use a Siddhi extension that is not shipped by default, see [Downloading and Installing Siddhi Extensions](../admin/downloading-and-Installing-Siddhi-Extensions.md)

## Transform data using custom function calls
To write custom function calls with Siddhi-script-js, follow the procedure below:

!!!
    In this section, you can reuse the `TemperatureApp` Siddhi application that you previously created. For this section,
     assume that you need to derive a unique ID for each temperature reading by combining the room number and the device ID.
    
1. In the `TemperatureApp` Siddhi application, add a script definition as follows.

    ```
    define function <function name>[<language name>] return <return type> {
        <operation of the function>
    };
    ```
    
    In this example, you can write a function that can be used to concatanate the room number and device ID as follows.
    
    ```
    define function concatFn[javascript] return string {
        var str1 = data[0];
        var str2 = data[1];
        var str3 = data[2];
        var responce = str1 + str2 + str3;
        return responce;
    };
    ```

2. Add another Siddhi query to apply the script you wrote to the relevant attributes of the input stream definition.
    ```
    from TempStream
    select concatFn(roomNo,'-',deviceID) as id, temp 
    insert into DeviceTempStream;
    ```
3. Save the Siddhi application.

## Transforming message formats (XML to JSON etc)

These transformations involve converting the message format to a different format after a the message is received, or 
converting the format before publishing the message. This is managed via mapping. For detailed instructions to convert
 message formats via mapping, see the following guides:
 
 - [Consuming Messages - Supported Message Formats](consuming-messages/#supported-message-formats)
 - [Publishing Messages - Supported Message Formats](publishing-data/#supported-message-formats)

