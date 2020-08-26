# MongoDB - Financials Workshop

The purpose of this workshop is to provide a series of query examples to assist a MongoDB user in how
to utilize a variety of elements of the MongoDB Query Language (MQL) and the MongoDB Aggregation Framework.

## Installation Prerequisites
* MongoDB 4.4

* MongoDB Shell and/or MongoDB Compass to perform the query exercises

## Workshop Outline
1. Generate Financial data
2. Query Examples
3. Aggregation Framework
4. Text Search
5. Change Stream Example

## Generate Financial Data
To generate financial data, use the following [Python Data Generator](https://github.com/blainemincey/generate_sample_data).
After installing the required Python modules, modify the appropriate values in the env.example file and then
rename the file to .env.  Run the script ``generate_financial_data.py``.  A sample JSON document is indicated below.
![](img/samplejson.jpg)

#### Note: Based on the dynamic data that is generated, the filters in the examples may have to be modified accordingly.

## Query Examples
* Basic filter
    * Customers in Texas:
    
        ```
        db.customerAccounts.find({"state":"Texas"}
        ```
      
    * Customers in Texas with Projection:
    
        ```
        db.customerAccounts.find({"state":"Texas"}, 
                {_id: 0, customerId: 1, customerSinceDate: 1})
        ```
      
    * Customers in Texas with Projection sorted by customerSinceDate in descending order:
    
        ```
        db.customerAccounts.find({"state":"Texas"}, 
            {_id: 0, customerId: 1, customerSinceDate: 1}).sort({customerSinceDate: -1})
        ```
    
    * Customers in Texas OR Delaware:
    
        ```
        db.customerAccounts.find({state:{$in:['Texas', 'Delaware']}}, 
            {_id:0, customerId: 1, state: 1} )
        ```
    
    * Customers in Texas OR Delaware and are new customers this year:
    
        ```
        db.customerAccounts.find({state:{$in:['Texas', 'Delaware']}, 
            customerSinceDate: {$gte:ISODate('2020-01-01')}}, 
            {_id: 0, state: 1, customerSinceDate: 1})
        ```  
    
* Filter on embedded object/array
    * Customers with a ONLY a Savings Account:
        ```
        db.customerAccounts.find({ accounts:{$size:1}, "accounts.accountType":"savings" }, 
            {_id:0, name:1, "accounts.accountType":1, "accounts.balance":1})
        ```
    
    * Customers with ONLY a Savings Account and a balance > $25000:
        ```
        db.customerAccounts.find({ accounts:{$size:1}, "accounts.accountType":"savings", "accounts.balance":{$gt:25000} }, 
            {_id:0, name:1, "accounts.accountType":1, "accounts.balance":1})
        ```
    
    * Customers with a Checking Account with a negative balance:
        ```
        db.customerAccounts.find({ "accounts.accountType":"checking", "accounts.balance":{$lt:0} }, 
            {_id:0, name:1, "accounts.accountType":1, "accounts.balance":1})
        ```
    
    * Customers with both a Savings and Checking account:
        ```
        db.customerAccounts.find({ "accounts.accountType":{$all:["savings","checking"]}}, 
            {_id:0, name:1, "accounts.accountType":1, "accounts.balance":1})
        ```
    
    
* Filter based on regular expression
    * Customer with name starting with 'Andrew'
    
    * Customers with a '@gmail.com' email address and a customer for more than 5 years:


* Existence check/Equality filter
    * Customers in NY without a Savings account:
    
    * Customers in NY without a Checking account:
    
    * Customers from a previous bank:
    
* Explain Plan/Indexes
    * Customers with a Savings Account Interest Rate > 3%

## Aggregation Framework
* Basic aggregation
* Complex aggregation
* Statistical/Mathematical aggregation

## Text Search
* Text Search example

## Change Stream Example
** time permitting - change streams/change data capture




















