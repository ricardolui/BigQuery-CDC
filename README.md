# BigQuery-CDC
This repo contains an example of how to stream UPSERTs natively into BigQuery.

[BigQuery change data capture (CDC)](https://cloud.google.com/bigquery/docs/change-data-capture) updates your BigQuery tables by processing and applying streamed updates to existing data. This synchronization is accomplished through upsert and delete row operations, which are streamed in real-time by the BigQuery Storage Write API. 

Documentation on how to use the Storage Write API can be found [HERE](https://cloud.google.com/bigquery/docs/write-api).

The blog post which provides context into BigQuery CDC and streaming upserts can be found [HERE](https://cloud.google.com/blog/products/data-analytics/bigquery-gains-change-data-capture-functionality).

Please refer and copy the files from the *cdc-example* folder.

**Pre-Requirements**
- Create bigquery dataset
```
    CREATE SCHEMA `CDC_Demo_Dataset` OPTIONS (
    location = 'YOUR_LOCATION'),
```
- Create bigquery table with right schema
```
#Replace CDC_Demo_Dataset with your preferred dataset name
CREATE TABLE `CDC_Demo_Dataset.Customer_Records` (
 Customer_ID INT64 PRIMARY KEY NOT ENFORCED,
 Customer_Enrollment_Date DATE,
 Customer_Name STRING,
 Customer_Address STRING,
 Customer_Tier STRING,
 Active_Subscriptions JSON)
CLUSTER BY
 Customer_ID
OPTIONS(max_staleness = INTERVAL 15 MINUTE);
```
- Re-run the sampe_data.proto with the right protobuf version > 2.19.0
```
protoc --python_out=. sample_data.proto
```

- Now change the variable for project name on cdc_python_script.py to your project-id
- Run the code
```
python cdc_python_script.py
```
- comment lines #118 and uncomment #119 and re-run
- comment lines #119 and uncomment #120  and re-run
- Check the modifications
