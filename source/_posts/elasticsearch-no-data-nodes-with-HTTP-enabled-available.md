title: elasticsearch-no-data-nodes-with-HTTP-enabled-available
date: 2019-01-09 19:10:54
categories: big data
tags: es
---

## problem

when run spark job to write data to es cluster on aws, I got error like this:

```

19/01/09 16:18:41 WARN TaskSetManager: Lost task 0.0 in stage 0.0 (TID 0, ip-10-149-56-178.vpc.internal, executor 1): org.elasticsearch.hadoop.EsHadoopIllegalArgumentException: No data nodes with HTTP-enabled available
 at org.elasticsearch.hadoop.rest.InitializationUtils.filterNonDataNodesIfNeeded(InitializationUtils.java:159)
 at org.elasticsearch.hadoop.rest.RestService.createWriter(RestService.java:583)
 at org.elasticsearch.spark.rdd.EsRDDWriter.write(EsRDDWriter.scala:58)
 at org.elasticsearch.spark.sql.streaming.EsStreamQueryWriter.run(EsStreamQueryWriter.scala:41)
 at org.elasticsearch.spark.sql.streaming.EsSparkSqlStreamingSink$$anonfun$addBatch$2$$anonfun$2.apply(EsSparkSqlStreamingSink.scala:52)
 at org.elasticsearch.spark.sql.streaming.EsSparkSqlStreamingSink$$anonfun$addBatch$2$$anonfun$2.apply(EsSparkSqlStreamingSink.scala:51)
 at org.apache.spark.scheduler.ResultTask.runTask(ResultTask.scala:87)
 at org.apache.spark.scheduler.Task.run(Task.scala:109)
 at org.apache.spark.executor.Executor$TaskRunner.run(Executor.scala:345)
 at java.util.concurrent.ThreadPoolExecutor.runWorker(ThreadPoolExecutor.java:1149)
 at java.util.concurrent.ThreadPoolExecutor$Worker.run(ThreadPoolExecutor.java:624)
 at java.lang.Thread.run(Thread.java:748)

19/01/09 16:18:42 ERROR TaskSetManager: Task 0 in stage 0.0 failed 4 times; aborting job
19/01/09 16:18:42 ERROR MicroBatchExecution: Query [id = a51c13fe-a36a-4308-bfc1-e384e88393dc, runId = bb0701d3-bbee-4938-9fcc-7d499faf0091] terminated with error

```

## solution

` "spark.es.nodes.wan.only" ` need to be `true`

my settings is as below:

```
"spark.es.nodes.client.only" = "false"
"spark.es.nodes.wan.only" = "true"
"spark.es.batch.write.retry.count" = "-1"
"spark.es.port" = "80"
"spark.es.index.auto.create" = "true"

```

