# XGBoost with Intel(R) OneCCL

## Overview

 XGBoost is an optimized distributed gradient boosting library designed to be highly ***efficient***, ***flexible*** and ***portable***.
 It implements machine learning algorithms under the [Gradient Boosting](https://en.wikipedia.org/wiki/Gradient_boosting) framework.
 Rabit (Reliable Allreduce and Broadcast Interface) is a light weight library that provides a fault tolerant interface of Allreduce and Broadcast. It is designed to support easy implementations of distributed machine learning programs, many of which fall naturally under the Allreduce abstraction.
 [Intel® oneAPI Collective Communications Library (oneCCL)](https://github.com/oneapi-src/oneCCL) provides an efficient implementation of communication patterns used in deep learning.
 This repo replaces Rabit with Intel(R) oneCCL to provide efficient communication patterns in multi-node multi-GPU clusters.

## Getting Started

To use XGBoost with the Intel(R) oneCCL, follow the steps below.

### Prerequisites

We use [Apache Maven](https://maven.apache.org/) to manage and build source code.  The following tools and libraries are also needed to build Intel MLlib:

* JDK 8.0+
* Apache Maven 3.6.2+
* GNU GCC 5.0.0+
* [Open Source Intel® oneAPI Collective Communications Library (oneCCL)](https://github.com/oneapi-src/oneCCL)

### Building

1.  Build oneCCL

To clone and build from open source oneCCL, run the following commands:
```
	$ git clone https://github.com/oneapi-src/oneCCL
    $ cd oneCCL && mkdir build && cd build
	$ cmake ..
	$ make -j install
```

The generated files will be placed in `/your/oneCCL_source_code/build/_install`.
Source setvars.sh as below. It will export CCL_ROOT which is used by XGBoost build later.

```bash
    $ source <_install dir>/env/setvars.sh
```

2.  Build XGBoost with oneCCL

XGBoost build requires g++ >= v5.0, so upgrade the toolchain if needed.
To clone and build XGBoost with oneCCL, run the following commands:
```
    $ git clone --recursive https://github.com/Intel-bigdata/xgboost.git -b oneccl
    $ cd jvm-packages
    $ mvn clean install -DskipTests
```

### Running

#### Prerequisites

* CentOS 7.0+, Ubuntu 18.04 LTS+
* Java JRE 8.0+ Runtime
* Apache Spark 2.4.3

#### Spark Configuration

Users usually run Spark application on __YARN__ with __client__ mode. In that case, you only need to add the following configurations in `spark-defaults.conf` or in `spark-submit` command line before running. 

```
# absolute path of the jars for uploading
spark.jars                /path/to/xgboost4j_x.xx-x.x.x[-SNAPSHOT].jar:/path/to/xgboost4j-spark_x.xx-x.x.x[-SNAPSHOT].jar
```
