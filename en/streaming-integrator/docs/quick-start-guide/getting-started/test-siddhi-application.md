# Step 5: Test the Siddhi Application

Any Siddhi application created for a business purpose needs to be tested before it is used to process actual data in order to ensure that it functions as expected. 

In [Step 2: Create a Siddhi Application](create-the-siddhi-application.md), you created the `SweetFactoryApp` Siddhi application to capture production data from the `production` MySQL database table as change data in real time, do a simple transformation, and then publish the output to a file. To see whether the Siddhi application actually functions as described, insert a record in the `production` database table.

To do this, issue the following command in the MySQL console.

`insert into SweetProductionTable values('chocolate',100.0);`

Then open the `/Users/foo/productions.csv` file. The following record should be displayed.

![Updated File](../../images/quick-start-guide-101/updated-file.png)
    
## What's Next?

To update the `SweetFactoryApp` Siddhi application you created so that it can publish to Kafka topics via the Kafka extension you installed, proceed to [Step 6: Update the Siddhi Application](update-the-siddhi-application.md)