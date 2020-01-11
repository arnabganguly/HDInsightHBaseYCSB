## Setup and run YCSB tests on the clusters

### 1. Login to HDInsight shell

- Steps to set up and run YCSB tests on both clusters are identical. 
- On the cluster page on the Azure portal , navigate to the **SSH + Cluster login** and use the Hostname and SSH path to ssh into the
    cluster.  The path should have below format. 
    
    ``` ssh <sshuser>@<clustername>.azurehdinsight.net ```

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

- To start lets run a write heavy workload to load 1 million rows into previously created HBase table.
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
2020-01-10 16:30:28:875 538 sec: 1000000 operations; 1384.78 current ops/sec; [CLEANUP: Count=8, Max=66623, Min=3, Avg=8370.12, 90=238, 99=66623, 99.9=66623, 99.99=66623] [INSERT: Count=11995, Max=91647, Min=1260, Avg=2811.37, 90=4411, 99=9911, 99.9=22511, 99.99=91455]
[OVERALL], RunTime(ms), 538663
[OVERALL], Throughput(ops/sec), 1856.4482802791356
[TOTAL_GCS_PS_Scavenge], Count, 779
[TOTAL_GC_TIME_PS_Scavenge], Time(ms), 2540
[TOTAL_GC_TIME_%_PS_Scavenge], Time(%), 0.47153786319090046
[TOTAL_GCS_PS_MarkSweep], Count, 0
[TOTAL_GC_TIME_PS_MarkSweep], Time(ms), 0
[TOTAL_GC_TIME_%_PS_MarkSweep], Time(%), 0.0
[TOTAL_GCs], Count, 779
[TOTAL_GC_TIME], Time(ms), 2540
[TOTAL_GC_TIME_%], Time(%), 0.47153786319090046
[CLEANUP], Operations, 8
[CLEANUP], AverageLatency(us), 8370.125
[CLEANUP], MinLatency(us), 3
[CLEANUP], MaxLatency(us), 66623
[CLEANUP], 95thPercentileLatency(us), 66623
[CLEANUP], 99thPercentileLatency(us), 66623
[INSERT], Operations, 1000000
[INSERT], AverageLatency(us), 2145.563448
[INSERT], MinLatency(us), 1128
[INSERT], MaxLatency(us), 305663
[INSERT], 95thPercentileLatency(us), 3389
[INSERT], 99thPercentileLatency(us), 6935
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

### Comparison of the numbers with explanation

dgdg| dgdg | dgd | dfg |
dgdg|dgdg|ddgd|dgdg|
|sfs  |ssfs  |  sfs|sfsfs

 
<!--stackedit_data:
eyJoaXN0b3J5IjpbMTY0OTU2NjY1OSw1NTMzOTY3ODksOTAzNz
QyMjEzLC0xMjg1MTcyNzQ5LC0xMTQxNTU5Njk4LDE2MzYxMTg0
NjQsMTQ3NjUwODMyMyw5ODQyMTQ0NTgsLTIxNDQ1NDU0MjQsLT
I1MjQ3NzkxNywtMTA1MTY1NjU4NywxNzczODgzMzgwLC00NjQ3
NDI0MDcsMTE2MDUwOTExOSwyMzk0NTM5OCwzNjEwMjY0NDMsMT
U5MDc0MjA4NiwxMDUxMTIzOTE5LC0yMDgwMzQ0MzA5LC0xNTQ4
Nzc5MF19
-->