## Setup and run YCSB tests on the clusters

 1. Steps to set up and run YCSB tests on both clusters are identical. 
  2. On the cluster page on the Azure portal , navigate to the **SSH + Cluster login** and use the Hostname and SSH path to ssh into the
    cluster.  The path should have below format. 
    
    ``` ssh <sshuser>@<clustername>.azurehdinsight.net ```

 2. Run the below steps to create the HBase tables which will be used to load the datasets
 
 - Launch the HBase Shell
```hbase shell ```
- Set a parameter for the number of table splits
```
n_splits = 200 # HBase recommends (10 * number of regionservers)
```
- Set the table splits (*10 * Number of Region Servers*) and then create the HBase table which would be used to run the tests 
```
hbase(main):018:0> n_splits = 100
=> 100
hbase(main):019:0> create 'usertable', 'cf', {SPLITS => (1..n_splits).map {|i| "user#{1000+i*(9999-1000)/n_splits}"}}
Created table usertable
Took 18.2968 seconds
=> Hbase::Table - usertable
```

3. Download the YCSB repository from the below destination
  ``` curl -O --location https://github.com/brianfrankcooper/YCSB/releases/download/0.17.0/ycsb-0.17.0.tar.gz ```

4. Unzip the folder to access the contents
```tar xfvz ycsb-0.17.0.tar.gz ```
5. This would create a  ycsb-0.17.0 folder. Move into this folder
``` cd ycsb-0.17.0 ```
6.  To start lets run a write heavy workload to load 1 million rows into previously created HBase tables.
```
run hbase12 -P workloads/workloadb -p columnfamily=f1 -p recordcount=1000000 -p operationcount=100000 -p threadcount=4 -s -cp /etc/hbase/conf | tee -a workloadb.dat

```
7. Explore the output of the cluster 


<!--stackedit_data:
eyJoaXN0b3J5IjpbLTM3NjkzNDc3NiwxMTYwNTA5MTE5LDIzOT
Q1Mzk4LDM2MTAyNjQ0MywxNTkwNzQyMDg2LDEwNTExMjM5MTks
LTIwODAzNDQzMDksLTE1NDg3NzkwLC0xNjcxMDEyMjUsLTE5MT
M0NjE0MjAsLTE1NjEzODMyNzMsMTU0MjEzMzcwLDE1MTEyMTIy
OTZdfQ==
-->