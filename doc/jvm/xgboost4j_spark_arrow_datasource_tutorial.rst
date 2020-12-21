#####################################################
XGBoost4J-Spark With Arrow Datasource Tutorial
#####################################################


********************************************
Prerequisites
********************************************

Build Arrow Datasource Jar
===================================
You can refer `Unified Arrow Data Source <https://github.com/Intel-bigdata/OAP/blob/master/oap-data-source/arrow/README.md>`_  to compile the spark-arrow-datasource*.jar.

Build And Install Apache Arrow
===================================

.. code-block:: none

  // build arrow-cpp
  git clone --branch native-sql-engine-clean https://github.com/Intel-bigdata/arrow.git
  cd arrow/cpp
  mkdir build
  cd build
  cmake -DCMAKE_BUILD_TYPE=Release -DARROW_DEPENDENCY_SOURCE=BUNDLED -DARROW_PARQUET=ON -DARROW_HDFS=ON -DARROW_BOOST_USE_SHARED=ON -DARROW_JNI=ON -DARROW_WITH_SNAPPY=ON -DARROW_WITH_PROTOBUF=ON -DARROW_DATASET=ON ..
  make
  make install

Setting up ARROW_HOME variable, add the following command to ``~/.bashrc`` file.

.. code-block:: none

  export ARROW_HOME=/usr/local

Building Java package from XGBoost Source Code
================================================

.. code-block:: none

  git clone https://github.com/Intel-bigdata/xgboost.git -b 5667+5774
  cd arrow/java
  mvn clean  package -DskipTests -Dmaven.test.skip=true

Then you can get the ``xgboost*.jar`` and ``xgboost-spark*.jar`` in ``./xgboost4j-spark/target/xgboost4j-spark_2.12-1.3.0-SNAPSHOT.jar`` and ``./xgboost4j/target/xgboost4j_2.12-1.3.0-SNAPSHOT.jar`` paths.

Download Spark 3.0.0
================================================
Currently xgboost spark with arrow datasource optimization works on the Spark 3.0.0 version.

.. code-block:: none

  wget http://archive.apache.org/dist/spark/spark-3.0.0/spark-3.0.0-bin-hadoop2.7.tgz
  tar -xf ./spark-3.0.0-bin-hadoop2.7.tgz
  export SPARK_HOME=``pwd``/spark-3.0.0-bin-hadoop2.7


********************************************
Get Started
********************************************
You can refer `Unified Arrow Data Source <https://github.com/Intel-bigdata/OAP/blob/master/oap-data-source/arrow/README.md>`_  to deploy the ``spark-arrow-datasource*.jar``. And the deploy approach of the ``xgboost*.jar`` and ``xgboost-spark*.jar`` is same with the upstream XGBoost without any additional operations.

********************************************
Note
********************************************
You don't need to use the ``VectorAssembler`` to assemble ``feature`` columns before training. Currently this optimization doesn't support ``limit``, ``coalesce`` and other sql operators, and we will support more operators in the future.
