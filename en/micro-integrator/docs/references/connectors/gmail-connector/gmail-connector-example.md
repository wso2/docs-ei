# Gmail Connector Example

The Gmail connector allows you to access the [Gmail REST API](https://developers.google.com/gmail/api/v1/reference/) through WSO2 ESB. Gmail is a free, Web-based e-mail service provided by Google.

## What you'll build

This example explains how to use the Gmail Connector along with File Connector to perform the following operations. 

* Create a file using File Connector
* Append content to the File
* Send the file using the Gmail Connector as an attachment. 

In this scenario, we have the healthcare backend running. Once we invoke our API, it has the data to call the backend service which is the category of the doctor in the payload. Once the data is received, it will generate an appointment reservation text file with the details of patient and the appointment. This file will be sent to the user provided email as an attachment. 


To implement this, you need to do the following.

1. [Generate Gmail access tokens](https://docs.wso2.com/display/ESBCONNECTORS/Configuring+Gmail+Operations).
2. Start the backend [healthcare service](https://github.com/wso2/docs-ei/blob/7.0.0/en/micro-integrator/docs/assets/attach/quick-start-guide/MI_QSG_HOME.zip) by running the jar file downloaded using `java -jar DoctorInfo.jar`
3. Configure the connector in WSO2 Integration Studio.

## Configure the connector in WSO2 Integration Studio

1. Open WSO2 Integration Studio and create an ESB Solution Project.
2. Our project would look similar to the following (source view).

```
<?xml version="1.0" encoding="UTF-8"?>
<api context="/reserve" name="ReserveAppointment" xmlns="http://ws.apache.org/ns/synapse">
    <resource methods="POST">
        <inSequence>
            <property expression="json-eval($.category)" name="uri.var.category" scope="default" type="STRING"/>
            <property expression="json-eval($.patientName)" name="patientName" scope="default" type="STRING"/>
            <property expression="json-eval($.age)" name="age" scope="default" type="STRING"/>
            <property expression="json-eval($.gender)" name="gender" scope="default" type="STRING"/>
            <property expression="json-eval($.email)" name="email" scope="default" type="STRING"/>
            <call>
                <endpoint key="GrandOak"/>
            </call>
            <property expression="json-eval($.doctors.doctor[0].name)" name="doctorName" scope="default" type="STRING"/>
            <property expression="json-eval($.doctors.doctor[0].time)" name="time" scope="default" type="STRING"/>
            <property expression="json-eval($.doctors.doctor[0].hospital)" name="hospital" scope="default" type="STRING"/>
            <fileconnector.create>
                <source>/../newpack/reservation.txt</source>
                <inputContent>{$ctx:patientName}</inputContent>
            </fileconnector.create>
            <fileconnector.append>
                <destination>/../newpack/reservation.txt</destination>
                <inputContent>{$ctx:age}</inputContent>
            </fileconnector.append>
            <fileconnector.append>
                <destination>/../newpack/reservation.txt</destination>
                <inputContent>{$ctx:gender}</inputContent>
            </fileconnector.append>
            <fileconnector.append>
                <destination>/../newpack/reservation.txt</destination>
                <inputContent>Doctor Name:</inputContent>
            </fileconnector.append>
            <fileconnector.append>
                <destination>/../newpack/reservation.txt</destination>
                <inputContent>{$ctx:doctorName}</inputContent>
            </fileconnector.append>
            <fileconnector.append>
                <destination>/../newpack/reservation.txt</destination>
                <inputContent>" "</inputContent>
            </fileconnector.append>
            <fileconnector.append>
                <destination>/../newpack/reservation.txt</destination>
                <inputContent>Appointment Time:</inputContent>
            </fileconnector.append>
            <fileconnector.append>
                <destination>/../newpack/reservation.txt</destination>
                <inputContent>" "</inputContent>
            </fileconnector.append>
            <fileconnector.append>
                <destination>/../newpack/reservation.txt</destination>
                <inputContent>{$ctx:time}</inputContent>
            </fileconnector.append>
            <fileconnector.append>
                <destination>/../newpack/reservation.txt</destination>
                <inputContent>" "</inputContent>
            </fileconnector.append>
            <fileconnector.append>
                <destination>/../newpack/reservation.txt</destination>
                <inputContent>Hospital:</inputContent>
            </fileconnector.append>
            <fileconnector.append>
                <destination>/../newpack/reservation.txt</destination>
                <inputContent>{$ctx:hospital}</inputContent>
            </fileconnector.append>
            <gmail.init>
                <userId>user1@gmail.com</userId>
                <accessToken></accessToken>
                <apiUrl>https://www.googleapis.com/gmail</apiUrl>
                <clientId></clientId>
                <clientSecret></clientSecret>
                <refreshToken></refreshToken>
            </gmail.init>
            <gmail.sendMailWithAttachment>
                <subject>Appointment Reservation</subject>
                <to>{$ctx:email}</to>
                <messageBody>Appointment Reservation</messageBody>
                <fileName>/../newpack/reservation.txt</fileName>
                <filePath>reservation.txt</filePath>
            </gmail.sendMailWithAttachment>
            <respond/>
        </inSequence>
        <outSequence/>
        <faultSequence/>
    </resource>
</api>

```

Change following accordingly. 
* fileconnector.create : source
* fileconnector.append : destination
* gmail.sendMailWithAttachment : filename

3. Right-click on the Composite Application Project and click on **Export Project Artifacts and Run**. Select **Run on Micro Integrator**.
4. Micro Integrator will be started and the composite application will be deployed. You can further refer to the application deployed through the CLI tool. Make sure you first export the PATH as below.

    ```
    $ export PATH=/path/to/mi/cli/directory/bin:$PATH\
    ```

## Testing

1. Login.

    ```
    ./mi remote login
    ```

2. Provide default credentials admin for both username and password.
3. In order to view the APIs deployed, execute the following command.

    ```
    ./mi api show
    ```
4. Send the following payload to invoke the api. 
```
{
	"patientName":"John Kenady\n",
	"age":"30\n",
	"gender":"Male\n",
	"category":"Ophthalmologist",
	"email":"isuruuy@gmail.com"
}
```

## What's next

* You can deploy and run your project on [Docker](../../../setup/installation/run_in_docker.md) or [Kubernetes](../../../setup/installation/run_in_kubernetes.md).
