## Setup and run YCSB tests on the clusters

### 1. Login to HDInsight shell

- Steps to set up and run [YCSB tests](https://github.com/brianfrankcooper/YCSB/wiki) on both clusters are identical. 
- On the cluster page on the Azure portal , navigate to the **SSH + Cluster login** and use the Hostname and SSH path to ssh into the
    cluster.  The path should have below format. 
    
    ``` 
    ssh <sshuser>@<clustername>.azurehdinsight.net 
    ```
    
![Clusterdeploy](https://github.com/arnabganguly/HDInsightHBaseYCSB/blob/master/images/image012.png)



### 2. Create the Table 
- Run the below steps to create the HBase tables which will be used to load the datasets
 
 - Launch the HBase Shell and set a parameter for the number of table splits. Set the table splits (*10 * Number of Region Servers*)
 - Create the HBase table which would be used to run the tests
 - Exit the HBase shell  
```
hbase(main):018:0> n_splits = 100
hbase(main):019:0> create 'usertable', 'cf', {SPLITS => (1..n_splits).map {|i| "user#{1000+i*(9999-1000)/n_splits}"}}
hbase(main):020:0> exit
```
### 3. Download the YSCB Repo 
- Download the YCSB repository from the below destination
```
$ curl -O --location https://github.com/brianfrankcooper/YCSB/releases/download/0.17.0/ycsb-0.17.0.tar.gz 
  ```

- Unzip the folder to access the contents
```
$ tar xfvz ycsb-0.17.0.tar.gz 
```
- This would create a  ycsb-0.17.0 folder. Move into this folder
``` $ cd ycsb-0.17.0 
```

### 4. Run a write heavy workload in both clusters  

 - Use the below command to initiate a write heavy workload with the below parameters
   - *workloads/workloada* : Indicates that the append workload *workloada* needs to be run
   - *table*: Populate the name of your HBase table created earlier
   - *columnfamily*: Populate the value of the HBase columfamily name from the table you created
   - *recordcount* : Number of records to be inserted( we use 1 Million)
   -  *threadcount*: Number of threads( this can be varied, but needs to be kept constant across experiments) 
   - *-cp /etc/hbase/conf*: Pointer to HBase config settings
   - *-s | tee -a*  : Provide a file name to write your output. 
  
 
```
 bin/ycsb load hbase12 -P workloads/workloada -p table=usertable -p columnfamily=cf -p recordcount=1000000 -p threadcount=4 -cp /etc/hbase/conf -s | tee -a workloada.dat
 ```

- Run the  write heavy workload to load 1 million rows into previously created HBase table.
>  Ignore the warnings that you may see after submitting the command. 

#### HDInsight HBase with accelerated writes
```
$ bin/ycsb load hbase12 -P workloads/workloada -p table=usertable -p columnfamily=cf -p recordcount=1000000 -p threadcount=4 -cp /etc/hbase/conf -s | tee -a workloada.dat

2020-01-10 16:21:40:213 10 sec: 15451 operations; 1545.1 current ops/sec; est completion in 10 minutes [INSERT: Count=15452, Max=120319, Min=1249, Avg=2312.21, 90=2625, 99=7915, 99.9=19551, 99.99=113855]
2020-01-10 16:21:50:213 20 sec: 34012 operations; 1856.1 current ops/sec; est completion in 9 minutes [INSERT: Count=18560, Max=305663, Min=1230, Avg=2146.57, 90=2341, 99=5975, 99.9=11151, 99.99=296703]
....
2020-01-10 16:30:10:213 520 sec: 972048 operations; 1866.7 current ops/sec; est completion in 15 seconds [INSERT: Count=18667, Max=91199, Min=1209, Avg=2140.52, 90=2469, 99=7091, 99.9=22591, 99.99=66239]
2020-01-10 16:30:20:214 530 sec: 988005 operations; 1595.7 current ops/sec; est completion in 7 second [INSERT: Count=15957, Max=38847, Min=1257, Avg=2502.91, 90=3707, 99=8303, 99.9=21711, 99.99=38015]
...
...
2020-01-11 00:22:06:192 564 sec: 1000000 operations; 1792.97 current ops/sec; [CLEANUP: Count=8, Max=80447, Min=5, Avg=10105.12, 90=268, 99=80447, 99.9=80447, 99.99=80447] [INSERT: Count=8512, Max=16639, Min=1200, Avg=2042.62, 90=2323, 99=6743, 99.9=11487, 99.99=16495]
[OVERALL], RunTime(ms), 564748
[OVERALL], Throughput(ops/sec), 1770.7012685303887
[TOTAL_GCS_PS_Scavenge], Count, 871
[TOTAL_GC_TIME_PS_Scavenge], Time(ms), 3116
[TOTAL_GC_TIME_%_PS_Scavenge], Time(%), 0.5517505152740692
[TOTAL_GCS_PS_MarkSweep], Count, 0
[TOTAL_GC_TIME_PS_MarkSweep], Time(ms), 0
[TOTAL_GC_TIME_%_PS_MarkSweep], Time(%), 0.0
[TOTAL_GCs], Count, 871
[TOTAL_GC_TIME], Time(ms), 3116
[TOTAL_GC_TIME_%], Time(%), 0.5517505152740692
[CLEANUP], Operations, 8
[CLEANUP], AverageLatency(us), 10105.125
[CLEANUP], MinLatency(us), 5
[CLEANUP], MaxLatency(us), 80447
[CLEANUP], 95thPercentileLatency(us), 80447
[CLEANUP], 99thPercentileLatency(us), 80447
[INSERT], Operations, 1000000
[INSERT], AverageLatency(us), 2248.752362
[INSERT], MinLatency(us), 1120
[INSERT], MaxLatency(us), 498687
[INSERT], 95thPercentileLatency(us), 3623
[INSERT], 99thPercentileLatency(us), 7375
[INSERT], Return=OK, 1000000

```

#### Explore the outcome of the test - Accelerated writes and Regular 

-  The test took 538663(8.97 Minutes) milliseconds to run
-  Return=OK, 1000000 indicates that all 1 Million inputs were were successfully written, **
- Write throughput was at 1856 operations per second
- 95% of the inserts had a latency of 3389 milliseconds
- Few inserts took more time , perhaps they were blocked by region severs due to the high workload

### HDInsight HBase without accelerated writes
```
2020-01-10 23:58:20:475 2574 sec: 1000000 operations; 333.72 current ops/sec; [CLEANUP: Count=8, Max=79679, Min=4, Avg=9996.38, 90=239, 99=79679, 99.9  =79679, 99.99=79679] [INSERT: Count=1426, Max=39839, Min=6136, Avg=9289.47, 90=13071, 99=27535, 99.9=38655, 99.99=39839]
[OVERALL], RunTime(ms), 2574273
[OVERALL], Throughput(ops/sec), 388.45918828344935
[TOTAL_GCS_PS_Scavenge], Count, 908
[TOTAL_GC_TIME_PS_Scavenge], Time(ms), 3208
[TOTAL_GC_TIME_%_PS_Scavenge], Time(%), 0.12461770760133055
[TOTAL_GCS_PS_MarkSweep], Count, 0
[TOTAL_GC_TIME_PS_MarkSweep], Time(ms), 0
[TOTAL_GC_TIME_%_PS_MarkSweep], Time(%), 0.0
[TOTAL_GCs], Count, 908
[TOTAL_GC_TIME], Time(ms), 3208
[TOTAL_GC_TIME_%], Time(%), 0.12461770760133055
[CLEANUP], Operations, 8
[CLEANUP], AverageLatency(us), 9996.375
[CLEANUP], MinLatency(us), 4
[CLEANUP], MaxLatency(us), 79679
[CLEANUP], 95thPercentileLatency(us), 79679
[CLEANUP], 99thPercentileLatency(us), 79679
[INSERT], Operations, 1000000
[INSERT], AverageLatency(us), 10285.497832
[INSERT], MinLatency(us), 5568
[INSERT], MaxLatency(us), 1307647
[INSERT], 95thPercentileLatency(us), 18751
[INSERT], 99thPercentileLatency(us), 33759
[INSERT], Return=OK, 1000000

```

### Comparison of the HBase Write numbers with explanation 

| Parameter |Unit |With Accelerated writes  | Without Accelerated writes |
|--|--|--|--|
| [OVERALL], RunTime(ms) |  Milliseconds| 567478 | 2574273 |
| [OVERALL], Throughput(ops/sec) |  Operations/sec| 1770 | 388 |
| [INSERT], Operations |  # of Operations| 1000000 | 1000000 |
| [INSERT], 95thPercentileLatency(us) |  Microseconds| 3623 | 18751 |
| [INSERT], 99thPercentileLatency(us) | Microseconds| 7375 | 33759 |
| [INSERT], Return=OK |  # of records| 1000000 | 1000000 |


 - [OVERALL], RunTime(ms) : Total execution time in milliseconds
 - [OVERALL], Throughput(ops/sec) : Number of operations/sec across all threads
 - [INSERT], Operations: Total number of insert operations,with associated average, min, max, 95th and 99th percentile latencies below
 - [INSERT], 95thPercentileLatency(us): 95% of INSERT operations have a data point below this value
 - [INSERT], 99thPercentileLatency(us): 99% of INSERT operations have a data point below this value
 - [INSERT], Return=OK: Record OK indicates that all INSERT operations were succesfull with the count alongside

### Try out a few other workloads and compare
**Read Mostly**(95% Read & 5% Write) : workloadb 
```
bin/ycsb run hbase12 -P workloads/workloadb -p table=usertable -p columnfamily=cf -p recordcount=1000000 -p operationcount=100000 -p threadcount=4 -cp /etc/hbase/conf -s | tee -a workloadb.dat
```

| Parameter |Unit |With Accelerated writes  | Without Accelerated writes |
|--|--|--|--|
| [OVERALL], RunTime(ms) |  Milliseconds| 292029 | 374379 |
| [OVERALL], Throughput(ops/sec) |  Operations/sec| 3424 | 2537 |
| [READ], Operations |  Operations/sec| 949833 | 949586|
| [UPDATE], Operations|  Operations/sec| 50167 | 50414|
| [READ], 95thPercentileLatency(us) |  Microseconds| 1401 | 3395 |
| [READ], 99thPercentileLatency(us) | Microseconds| 1387 | 3611 |
| [READ], Return=OK |  # of records| 949833 | 949586 |


**Read Only** : workloadc
```
bin/ycsb run hbase12 -P workloads/workloadc -p table=usertable -p columnfamily=cf -p recordcount=1000000 -p operationcount=100000 -p threadcount=4 -cp /etc/hbase/conf -s | tee -a workloadc.dat
```


| Parameter |Unit |With Accelerated writes  | Without Accelerated writes |
|--|--|--|--|
| [OVERALL], RunTime(ms) |  Milliseconds| 272031 | 253256 |
| [OVERALL], Throughput(ops/sec) |  Operations/sec| 3676 | 3948 |
| [READ], Operations |  Operations/sec| 1000000 | 1000000 |
| [READ], 95thPercentileLatency(us) |  Microseconds| 1385 | 1410|
| [READ], 99thPercentileLatency(us) | Microseconds| 3215 | 3723 |
| [READ], Return=OK |  # of records| 1000000 | 1000000 |
 
<!--stackedit_data:
eyJoaXN0b3J5IjpbLTE1OTQxMDk5OSwxMjY2NDI5NSwyMDYwND
E1MTQyLC0xMTIwNTc2MTkwLDMxNTY3OTE5NSw5OTU2MjQzMyw1
NTMzOTY3ODksOTAzNzQyMjEzLC0xMjg1MTcyNzQ5LC0xMTQxNT
U5Njk4LDE2MzYxMTg0NjQsMTQ3NjUwODMyMyw5ODQyMTQ0NTgs
LTIxNDQ1NDU0MjQsLTI1MjQ3NzkxNywtMTA1MTY1NjU4NywxNz
czODgzMzgwLC00NjQ3NDI0MDcsMTE2MDUwOTExOSwyMzk0NTM5
OF19
-->