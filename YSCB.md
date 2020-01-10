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
n_splits = 200 # HBase recommends (10 * number of regionservers)```

- Set the table splits and then create the HBase table which would be used to run the tests 
```create 'usertable', {NAME => 'f1', VERSIONS => '1', COMPRESSION => 'LZO'}```
- Exit the HBase shell
```exit

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
eyJoaXN0b3J5IjpbLTE2NzU0MDQwNDgsMzYxMDI2NDQzLDE1OT
A3NDIwODYsMTA1MTEyMzkxOSwtMjA4MDM0NDMwOSwtMTU0ODc3
OTAsLTE2NzEwMTIyNSwtMTkxMzQ2MTQyMCwtMTU2MTM4MzI3My
wxNTQyMTMzNzAsMTUxMTIxMjI5Nl19
-->