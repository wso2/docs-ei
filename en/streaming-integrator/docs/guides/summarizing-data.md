# Summarizing Data

## Introduction

Summarizing data refers to obtaining aggregates in an incremental manner for a specified set of time periods.

## Performing clock-time based summarization

Performing clock-time based based summarization involves calculating, storing, and then retrieving aggregations for a 
selected range of time granularities. This process is carried out in two parts:

1. Calculating the aggregations for the selected time granularities and storing the results.
2. Retrieving previously calculated aggregations for selected time granularities. 

To understand data summarization further, consider the scenario where a business that sells multiple brands stores its sales data in a physical database for the purpose of retrieving them later to perform sales analysis. Each sales transaction is received with the following details:
                                                                                                                                                
`symbol`: The symbol that represents the brand of the items sold.
`price`: the price at which each item was sold.
`amount`: The number of items sold.

The Sales Analyst needs to retrieve the total number of items sold of each brand per month, per week, per day etc., and then retrieve these totals for specific time durations to prepare sales analysis reports.
                                                                                                                                                
!!!info
    It is not always required to maintain a physical database for incremental analysis, but it enables you to try out your aggregations with ease.
    
The followinbg sections explain how to calculate and store time-based aggregations for this scenarios, and then retrieve them.


### Calculate and store clock time-based aggregate values
### Retrieve the stored aggregate values

## Summarizing data based on built in windowing criterias
 - siddhi-execution-unique
 
 Other windows will also be explained here.
