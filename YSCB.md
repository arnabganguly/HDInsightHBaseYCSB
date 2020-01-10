## Setup and run YCSB tests on the clusters

### 1. Login to HDInsight shell

- Steps to set up and run YCSB tests on both clusters are identical. 
- On the cluster page on the Azure portal , navigate to the **SSH + Cluster login** and use the Hostname and SSH path to ssh into the
    cluster.  The path should have below format. 
    
    ``` ssh <sshuser>@<clustername>.azurehdinsight.net ```

### 2. Create the Table 
- Run the below steps to create the HBase tables which will be used to load the datasets
 
 - Launch the HBase Shell
```hbase shell ```
- Set a parameter for the number of table splits
```
n_splits = 200 # HBase recommends (10 * number of regionservers)
```
- Set the table splits (*10 * Number of Region Servers*) and then create the HBase table which would be used to run the tests 
```
hbase(main):018:0> n_splits = 100
hbase(main):019:0> create 'usertable', 'cf', {SPLITS => (1..n_splits).map {|i| "user#{1000+i*(9999-1000)/n_splits}"}}
```
### 3. Download the YSCB Repo 
- Download the YCSB repository from the below destination
  ``` curl -O --location https://github.com/brianfrankcooper/YCSB/releases/download/0.17.0/ycsb-0.17.0.tar.gz ```

- Unzip the folder to access the contents
```tar xfvz ycsb-0.17.0.tar.gz ```
- This would create a  ycsb-0.17.0 folder. Move into this folder
``` cd ycsb-0.17.0 ```

### 4. Run a workload 

- To start lets run a write heavy workload to load 1 million rows into previously created HBase table.
```
bin/ycsb load hbase12 -P workloads/workloada -p table=usertable -p columnfamily=cf -p recordcount=1000000 -p threadcount=4 -cp /etc/hbase/conf -s | tee -a workloada.dat

```
7. Explore the outcome of the test 



<!--stackedit_data:
eyJoaXN0b3J5IjpbLTEwNTE2NTY1ODcsMTc3Mzg4MzM4MCwtND
Y0NzQyNDA3LDExNjA1MDkxMTksMjM5NDUzOTgsMzYxMDI2NDQz
LDE1OTA3NDIwODYsMTA1MTEyMzkxOSwtMjA4MDM0NDMwOSwtMT
U0ODc3OTAsLTE2NzEwMTIyNSwtMTkxMzQ2MTQyMCwtMTU2MTM4
MzI3MywxNTQyMTMzNzAsMTUxMTIxMjI5Nl19
-->