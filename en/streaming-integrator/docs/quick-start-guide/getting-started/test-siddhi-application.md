# Step 4: Test the Siddhi Application

Any Siddhi application created for a business purpose needs to be tested before it is used to process actual data in order to ensure that it functions as expected. 

In [Step 2: Create a Siddhi Application](create-the-siddhi-application.md), you created the `SweetFactoryApp` Siddhi application to capture production data from the `production` MySQL database table as change data in real time, do a simple transformation, and then publish the output to a file. To see whether the Siddhi application actually functions as described, insert a record in the `production` database table.

To do this, issue the following command in the MySQL console.

`insert into SweetProductionTable values('chocolate',100.0);`

Then open the `/Users/foo/productions.csv` file. The following record should be displayed.

![Updated File](../../images/quick-start-guide-101/updated-file.png)
    
## What's Next?

When you are designing Siddhi applications, you may need to use functions, transports, formats that the WSO2 Streaming Integrator does not support by default. In such situations, you can extend the Streaming Integrator by installing the required Siddhi extensions. To try out installing a Siddhi extension that is not shipped with WSO2 Streaming Integrator by default, proceed to [Step 6: Extend Streaming Integrator](extend-si.md).