## Setup and run YCSB tests on the clusters

 1. Steps to set up and run YCSB tests on both clusters are identical. 
  2. On the cluster page on the Azure portal , navigate to the **SSH + Cluster login** and use the Hostname and SSH path to ssh into the
    cluster.  The path should have below format. 
    
    ``` ssh <sshuser>@<clustername>.azurehdinsight.net ```

 2. Run the below steps to create the HBase tables which will be used to load the datasets
 
 - Launch the HBase Shell
```hbase shell ```
- At the HBase shell create the HBase table which would be used to run the tests 
```create 'usertable', {NAME => 'f1', VERSIONS => '1', COMPRESSION => 'LZO'}```
- Exit the HBase shell
```exit```

3. Download the YCSB repository from the below destination
  ``` curl -O --location https://github.com/brianfrankcooper/YCSB/releases/download/0.17.0/ycsb-0.17.0.tar.gz ```

4. Unzip the folder to access the contents
```tar xfvz ycsb-0.17.0.tar.gz ```
5. This would create a  ycsb-0.17.0 folder. Move into this folder
``` cd ycsb-0.17.0 ```
6.  To start lets run a data load workload into the HBase tables.
```




<!--stackedit_data:
eyJoaXN0b3J5IjpbMTA1MTEyMzkxOSwtMjA4MDM0NDMwOSwtMT
U0ODc3OTAsLTE2NzEwMTIyNSwtMTkxMzQ2MTQyMCwtMTU2MTM4
MzI3MywxNTQyMTMzNzAsMTUxMTIxMjI5Nl19
-->