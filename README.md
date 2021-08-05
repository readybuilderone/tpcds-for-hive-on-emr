# TPCDS benchmark Kits for Hive on AWS EMR

A TPCDS benchmark test kits for Hive On AWS EMR.

## Overview

This benchmark includes the data generator and set of TPCDS queries for hive, which help you experiment with Hive On AWS EMR at scale. It allows you to experience base Hive performance on large datasets, and gives an easy way to see the impact of Hive tuning parameters and advanced settings.

## How to generate Benchmark Data

We use the official kits from TPC-DS(http://www.tpc.org/) to generate the benchmark data.

### How to Generate Benchmark Data

Step 1: Compile the Data Generator Kits

``` shell
sudo yum install git -y 
git clone https://github.com/readybuilderone/tpcds-for-hive-on-emr.git

sudo yum install gcc make flex bison byacc -y
cd tpcds-for-hive-on-emr/generator-sourcecode

make OS=LINUX -j8 && cd -
```

Step 2: Generate your benchmark data

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



## How to Run the Benchmark On EMR Hive



### Step 1: GenerateCode

### Step 2: 

## FAQ

- How long will it to generate the benchmark data?

- 



## References:



