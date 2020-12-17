# Integrating Data Stores in Streamming Integration

WSO2 Streaming Integrator allows you to incorporate data stores when performing various streaming integration activities. The methods in which this is done include:

- Change data capture
- Storing received data/processed data in data stores
- Performing CRUD operations in data stores

[Performing Real-time Change Data Capture with MySQL] tutorial covers how to perform change data capture in details. Therefore, in this tutorial, let's learn how WSO2 Streaming Integrator can incorporate data stores in streaming operations by performing CRUD operations.

Let's consider the example of a Sweet Factory that stores the following information in three different databases:

- Records of material purchases to be used in production
- Records of material dispatches for production
- The current stock of materials

In order to manage the material stock and to maintain the required records, the Factory Manager needs to do the following activities:

- Record each material purchase in the store for purchases.
- Record each dispatch of material for production in the store for dispatches.
- Update the store with current stock for each material after each purchase and dispatch to keep it up to date.

Recording purchases and dispatches involve inserting new records into data stores. To maintain the current stock records, the Factory Manager needs to retrieve information about both purchases and dispatches, calculate the impact of both on the current stock and then perform an insert/update operation to the store with the stock records. 

To understand how the WSO2 Streaming Integrator performs these operations, follow the topics below.

!!! tip "Before you begin:"
    To try out this tutorial, you need to complete the following:

