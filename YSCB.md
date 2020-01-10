## Setup and run YCSB tests on the clusters

 1. Steps to set up and run YCSB tests on both clusters are identical. 
  2. On the cluster page on the Azure portal , navigate to the **SSH + Cluster login** and use the Hostname and SSH path to ssh into the
    cluster.  The path should have below format. 
    
    ``` ssh <sshuser>@<clustername>.azurehdinsight.net ```
3. Download the YCSB repository from the below destination  ``` curl -O --location https://github.com/brianfrankcooper/YCSB/releases/download/0.17.0/ycsb-0.17.0.tar.gz
    ```
 3. Run the below steps to create the HBase tables which will be used to load the datasets
 
 - Launch the HBase Shell
```hbase shell ```
- At the HBase shell create the HBase table which would be used to run the tests 
```create 'usertable', {NAME => 'f1', VERSIONS => '1', COMPRESSION => 'LZO'}```
- Exit the HBase shell
```exit```

4. 






<!--stackedit_data:
eyJoaXN0b3J5IjpbMTU0MjA4NDkyNCwtMTU2MTM4MzI3MywxNT
QyMTMzNzAsMTUxMTIxMjI5Nl19
-->