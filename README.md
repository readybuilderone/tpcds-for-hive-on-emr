# TPCDS Benchmark Kits for Hive on AWS EMR

A TPCDS benchmark test kits for Hive On AWS EMR.

## Overview

This benchmark includes the data generator and set of TPCDS queries for hive, which help you experiment with Hive On AWS EMR at scale. It allows you to experience base Hive performance on large datasets, and gives an easy way to see the impact of Hive tuning parameters and advanced settings.

## How to generate Benchmark Data

We use the official kits from TPC-DS(http://www.tpc.org/) to generate the benchmark data.

### Step 1: Compile the Data Generator Kits

``` shell
sudo yum install git -y 
git clone https://github.com/readybuilderone/tpcds-for-hive-on-emr.git

sudo yum install gcc make flex bison byacc -y
cd tpcds-for-hive-on-emr/generator-sourcecode

make OS=LINUX -j8 && cd -
```

### Step 2: Generate your benchmark data

``` shell
mkdir ./<TargetFolder>
cd generator-sourcecode
nohup ./dsdgen -sc <ExpectedDataSizeinGB> -dir ../<TargetFolder> -f&
```

Do to replace the variable of TargetFolder and ExpectedDataSizeinGB with your own.

Eg: if you want to generate 100GB data in data100 folder, the scripts will be:

``` shell
mkdir ./data100
cd generator-sourcecode
nohup ./dsdgen -sc 100 -dir ../data100 -f&
```

It may take quite a long time to generate the benchmark data, you could view your process through this command:

``` shell
ps aux|grep dsdgen
```

## How to upload Benchmark Data to your AWS S3

Before uploading benchmark data to S3, make sure that your current operating environment has permissions to upload data to S3. This can be done either by configuring AK/SK (not recommended) or by granting the corresponding Role to EC2.

```shell
aws s3 cp call_center.dat s3://<YOUR-S3-BUCKET>/tpcds/call_center/call_center.dat
aws s3 cp catalog_page.dat s3://<YOUR-S3-BUCKET>/tpcds/catalog_page/catalog_page.dat
aws s3 cp catalog_returns.dat s3://<YOUR-S3-BUCKET>/tpcds/catalog_returns/catalog_returns.dat
aws s3 cp catalog_sales.dat s3://<YOUR-S3-BUCKET>/tpcds/catalog_sales/catalog_sales.dat
aws s3 cp customer_address.dat s3://<YOUR-S3-BUCKET>/tpcds/customer_address/customer_address.dat
aws s3 cp customer.dat s3://<YOUR-S3-BUCKET>/tpcds/customer/customer.dat
aws s3 cp customer_demographics.dat s3://<YOUR-S3-BUCKET>/tpcds/customer_demographics/customer_demographics.dat
aws s3 cp date_dim.dat s3://<YOUR-S3-BUCKET>/tpcds/date_dim/date_dim.dat
aws s3 cp dbgen_version.dat s3://<YOUR-S3-BUCKET>/tpcds/dbgen_version/dbgen_version.dat
aws s3 cp household_demographics.dat s3://<YOUR-S3-BUCKET>/tpcds/household_demographics/household_demographics.dat
aws s3 cp income_band.dat s3://<YOUR-S3-BUCKET>/tpcds/income_band/income_band.dat
aws s3 cp inventory.dat s3://<YOUR-S3-BUCKET>/tpcds/inventory/inventory.dat
aws s3 cp item.dat s3://<YOUR-S3-BUCKET>/tpcds/item/item.dat
aws s3 cp promotion.dat s3://<YOUR-S3-BUCKET>/tpcds/promotion/promotion.dat
aws s3 cp reason.dat s3://<YOUR-S3-BUCKET>/tpcds/reason/reason.dat
aws s3 cp ship_mode.dat s3://<YOUR-S3-BUCKET>/tpcds/ship_mode/ship_mode.dat
aws s3 cp store.dat s3://<YOUR-S3-BUCKET>/tpcds/store/store.dat
aws s3 cp store_returns.dat s3://<YOUR-S3-BUCKET>/tpcds/store_returns/store_returns.dat
aws s3 cp store_sales.dat s3://<YOUR-S3-BUCKET>/tpcds/store_sales/store_sales.dat
aws s3 cp time_dim.dat s3://<YOUR-S3-BUCKET>/tpcds/time_dim/time_dim.dat
aws s3 cp warehouse.dat s3://<YOUR-S3-BUCKET>/tpcds/warehouse/warehouse.dat
aws s3 cp web_page.dat s3://<YOUR-S3-BUCKET>/tpcds/web_page/web_page.dat
aws s3 cp web_returns.dat s3://<YOUR-S3-BUCKET>/tpcds/web_returns/web_returns.dat
aws s3 cp web_sales.dat s3://<YOUR-S3-BUCKET>/tpcds/web_sales/web_sales.dat
aws s3 cp web_site.dat s3://<YOUR-S3-BUCKET>/tpcds/web_site/web_site.dat

```

Do to replace variable <YOUR-S3-BUCKET> to your own S3 bucket name, and create the tpcds folder in your bucket before you upload the data.

## How to Run the Benchmark On EMR Hive

SSH to master node of your EMR cluster, and use the HQL scripts in benchmark-sql/create-tpcdsdb.hql to create the database and tables in EMR cluster.

``` shell
hive -f create-tpcdsdb.hql
```

Do to replace variable <YOUR-BUCKET> in the script and make sure the path of S3 is correct. Once finished, using the show tables command to check the newly created tables:

``` shell
hive> show tables;
OK
call_center
catalog_page
catalog_returns
catalog_sales
customer
customer_address
customer_demographics
date_dim
dbgen_version
household_demographics
income_band
inventory
item
promotion
reason
ship_mode
store
store_returns
store_sales
time_dim
warehouse
web_page
web_returns
web_sales
web_site
Time taken: 0.345 seconds, Fetched: 25 row(s)


```

When finished, you could run the benchmark using tpc-ds benchmark queries in benchmark-sql/queries folder.



## References:

- http://www.tpc.org/tpcds/presentations/tpcds_workload_analysis.pdf
- https://github.com/ActianCorp/Vector-TPC-DS-Scripts 

