# Step 4: Run the Siddhi Application

In this step, let's run the `SweetFactoryApp` Siddhi application that you created, tested and deployed.

To do this, insert a record in the `production` database table by issuing the following command in the MySQL console.

`insert into SweetProductionTable values('chocolate',100.0);`

Then open the `/Users/foo/productioninserts.csv` file. The following record should be displayed.

![Updated File](../../images/quick-start-guide-101/updated-file.png)
    
!!! tip "What's Next?"
    Now you can try extending the `SweetFactoryApp` Siddhi application to perform more streaming integration activities. To try this, proceed to [Step 5: Update the Siddhi Application](update-the-siddhi-application.md).